# 15 — Security Document
## Employee Performance Intelligence System (EPIS)

---

## 1. Security Architecture

EPIS uses a layered security model. Each layer is independent, so if one is bypassed, the next layer still protects the data.

```
Layer 1: Next.js Middleware (Route Protection)
         → Blocks unauthenticated requests before they reach any page

Layer 2: API Route Handlers (Role Validation)
         → Validates JWT and role on every API call

Layer 3: Supabase Row Level Security (Data Isolation)
         → Database enforces access rules regardless of API logic

Layer 4: Supabase Auth (Token Integrity)
         → JWTs are signed and cannot be tampered with
```

---

## 2. Authentication Security

### 2.1 Supabase Auth

- Passwords are never stored by EPIS — Supabase Auth manages all password hashing (bcrypt)
- JWT tokens are signed with Supabase's private key — cannot be forged
- Tokens contain role in `user_metadata` — injected at user creation, not modifiable by the user
- Token expiry: 1 hour (access token); 7 days (refresh token, auto-renewed by Supabase client)

### 2.2 Session Storage

- The Supabase JS client stores the session in `localStorage` by default
- In V1, this is acceptable for a portfolio project
- In production: use `storage: { getItem, setItem, removeItem }` with `httpOnly` cookies via Supabase SSR helpers

### 2.3 Password Rules

Enforced by Supabase Auth configuration:
- Minimum length: 8 characters
- No maximum restriction
- No complexity requirement enforced at V1 (can be added via Supabase dashboard)

---

## 3. Authorization Security

### 3.1 Middleware Enforcement

Every request to a protected route passes through `middleware.ts`:

```typescript
// middleware checks:
1. Is there a valid session?      → No session = redirect to /login
2. Is the JWT still valid?        → Expired = redirect to /login
3. Does the role match the route? → Wrong role = redirect to /unauthorized
```

### 3.2 API-Level Role Check

Even if middleware is bypassed (e.g., via direct cURL), every API route independently validates the role:

```typescript
const role = session.user.user_metadata.role
if (!['ceo', 'tester'].includes(role)) {
  return NextResponse.json({ error: 'FORBIDDEN' }, { status: 403 })
}
```

### 3.3 Row Level Security (RLS)

RLS is enabled on all Supabase tables. Even if the API layer is compromised, RLS ensures:
- Employees only get their own row
- Managers only get rows from their department
- No raw SQL query can bypass RLS (it applies at the PostgreSQL query execution level)

**Never disable RLS in production.**

---

## 4. Environment Variable Security

### 4.1 Variables Used

| Variable | Used In | Secret? |
|---|---|---|
| `NEXT_PUBLIC_SUPABASE_URL` | Client + Server | No (public, safe) |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Client + Server | No (public, limited by RLS) |
| `SUPABASE_SERVICE_ROLE_KEY` | Server only (if used) | **YES — never expose** |

### 4.2 Rules

- Variables prefixed with `NEXT_PUBLIC_` are exposed in browser bundles — never put secrets there
- `SUPABASE_SERVICE_ROLE_KEY` bypasses RLS — only use in admin scripts, never in API routes
- All variables live in `.env.local` (not committed to Git)
- Production variables are set in Vercel Environment Variables dashboard

### 4.3 `.gitignore`

```
.env
.env.local
.env.production
.env*.local
```

---

## 5. Input Validation & XSS Prevention

### 5.1 Search and Filter Inputs

All user inputs passed to Supabase queries use parameterized queries via the Supabase JS client:

```typescript
// Safe: parameterized query — Supabase client handles escaping
const { data } = await supabase
  .from('employees')
  .select('*')
  .ilike('job_role', `%${searchQuery}%`)  // Supabase escapes this
```

Never use raw string interpolation in SQL:
```typescript
// NEVER DO THIS:
const { data } = await supabase.rpc(`SELECT * FROM employees WHERE name = '${input}'`)
```

### 5.2 Output Rendering

All values from the database are rendered via React's JSX, which automatically escapes HTML:

```tsx
// Safe: React escapes by default
<p>{employee.job_role}</p>

// Unsafe: NEVER USE dangerouslySetInnerHTML with user data
<p dangerouslySetInnerHTML={{ __html: employee.job_role }} />
```

### 5.3 Content Security Policy (CSP)

Add to `next.config.js`:

```javascript
const securityHeaders = [
  { key: 'X-Content-Type-Options', value: 'nosniff' },
  { key: 'X-Frame-Options', value: 'DENY' },
  { key: 'X-XSS-Protection', value: '1; mode=block' },
  { key: 'Referrer-Policy', value: 'strict-origin-when-cross-origin' },
]
```

---

## 6. CSRF Protection

Next.js API routes with the App Router use server-side execution — they are not vulnerable to traditional CSRF because:
- They require a valid Supabase JWT in every request
- JWTs are not automatically sent by browsers on cross-origin requests (unlike cookies)

If cookie-based sessions are implemented in future versions, add CSRF tokens.

---

## 7. Rate Limiting (V2)

Rate limiting is not implemented in V1. For V2, add:

```typescript
// Using Vercel's built-in rate limiting or a middleware like `@upstash/ratelimit`
// Limit: 100 requests per minute per IP on API routes
// Limit: 5 failed login attempts per 15 minutes per IP
```

---

## 8. Security Checklist Before Deployment

- [ ] `.env.local` is in `.gitignore` and not in the repo
- [ ] `SUPABASE_SERVICE_ROLE_KEY` is not used in any API route
- [ ] RLS is enabled on all Supabase tables
- [ ] All API routes check JWT and role before returning data
- [ ] No `dangerouslySetInnerHTML` with user-supplied data
- [ ] Security headers are configured in `next.config.js`
- [ ] No sensitive data in `localStorage` (only theme, sidebar state)
- [ ] Supabase Auth is configured with minimum 8-character passwords
- [ ] Error messages do not expose stack traces to users
- [ ] All environment variables are set in Vercel dashboard, not hardcoded

# 04 — User Roles & Permissions
## Employee Performance Intelligence System (EPIS)

---

## 1. Overview

EPIS uses four user roles. Each role is assigned during user creation and stored in the `users` table in Supabase. The role is embedded in the JWT claims and read by Next.js middleware to control access to routes and API endpoints.

---

## 2. Role Definitions

### 2.1 CEO
The CEO has the highest level of access. They can see all departments, all employees, and all performance data across the entire organization.

**Access:**
- All dashboard pages
- All departments
- All employee records
- Salary data (all employees)
- Attrition analysis (all departments)
- Export functionality

**Cannot do:**
- Modify employee records
- Change other users' passwords
- Access the admin panel (Version 2)

---

### 2.2 Manager
A Manager can only see data for their own department. They cannot see employees or performance data from other departments.

**Access:**
- Manager dashboard
- Employees in their assigned department only
- Department KPIs and charts
- Individual employee profiles (within their department)
- Department-level export

**Cannot do:**
- See employees from other departments
- See organization-wide salary totals
- Access CEO dashboard

---

### 2.3 Employee
An Employee can only see their own profile and personal performance data. They cannot see any other employee's data.

**Access:**
- Employee dashboard (their own profile only)
- Own salary information
- Own performance ratings
- Own training data
- Own satisfaction scores
- Own years/tenure data

**Cannot do:**
- See any other employee's data
- See department aggregates
- Export data

---

### 2.4 Tester / QA
A Tester has read-only access to all dashboards for the purpose of quality assurance testing. They cannot modify any data.

**Access:**
- All dashboard views (CEO, Manager, Employee)
- All departments and employees
- Read-only access across the entire application

**Cannot do:**
- Export data
- Modify any records
- Change settings that affect data

---

## 3. Permission Matrix

| Feature | CEO | Manager | Employee | Tester |
|---|---|---|---|---|
| Login | ✅ | ✅ | ✅ | ✅ |
| CEO Dashboard | ✅ | ❌ | ❌ | ✅ |
| Manager Dashboard | ✅ | ✅ | ❌ | ✅ |
| Employee Dashboard | ✅ | ✅ | ✅ | ✅ |
| View all departments | ✅ | ❌ | ❌ | ✅ |
| View own department | ✅ | ✅ | ❌ | ✅ |
| View all employees | ✅ | ❌ | ❌ | ✅ |
| View dept employees | ✅ | ✅ | ❌ | ✅ |
| View own profile | ✅ | ✅ | ✅ | ✅ |
| View salary (all) | ✅ | ❌ | ❌ | ✅ |
| View own salary | ✅ | ✅ | ✅ | ✅ |
| Export CSV | ✅ | ✅ | ❌ | ❌ |
| Change own password | ✅ | ✅ | ✅ | ✅ |
| Dark mode toggle | ✅ | ✅ | ✅ | ✅ |

---

## 4. Route Protection Rules

| Route | Allowed Roles |
|---|---|
| / | Redirect to login if not authenticated |
| /login | Public (redirect to dashboard if already logged in) |
| /signup | Public |
| /forgot-password | Public |
| /dashboard/ceo | CEO, Tester |
| /dashboard/ceo/employees | CEO, Tester |
| /dashboard/ceo/departments | CEO, Tester |
| /dashboard/ceo/reports | CEO, Tester |
| /dashboard/manager | Manager, CEO, Tester |
| /dashboard/manager/employees | Manager, CEO, Tester |
| /dashboard/manager/performance | Manager, CEO, Tester |
| /dashboard/employee | Employee, Manager, CEO, Tester |
| /dashboard/employee/profile | Employee, Manager, CEO, Tester |
| /profile | All authenticated users |
| /settings | All authenticated users |
| /about | All authenticated users |
| /unauthorized | All users |
| /404 | All users |

---

## 5. Middleware Logic

The middleware runs on every request before the page loads.

```typescript
// middleware.ts (pseudocode)

export async function middleware(request: NextRequest) {
  const token = request.cookies.get('supabase-auth-token')

  // 1. No token → redirect to /login
  if (!token) {
    return NextResponse.redirect('/login')
  }

  // 2. Validate token with Supabase
  const { user, error } = await supabase.auth.getUser(token)
  if (error || !user) {
    return NextResponse.redirect('/login')
  }

  // 3. Get role from users table
  const role = user.user_metadata.role  // or from DB

  // 4. Check route against allowed roles
  const path = request.nextUrl.pathname

  if (path.startsWith('/dashboard/ceo') && role !== 'ceo' && role !== 'tester') {
    return NextResponse.redirect('/unauthorized')
  }

  if (path.startsWith('/dashboard/manager') && role === 'employee') {
    return NextResponse.redirect('/unauthorized')
  }

  // 5. Allow request
  return NextResponse.next()
}
```

---

## 6. Row Level Security (Supabase RLS)

Supabase RLS adds a second layer of data protection at the database level. Even if middleware is bypassed, the database will only return rows the user is allowed to see.

| Table | CEO | Manager | Employee |
|---|---|---|---|
| employees | All rows | Own department rows | Own row only |
| performance | All rows | Own dept employees | Own row only |
| salary | All rows | Own dept employees | Own row only |
| training | All rows | Own dept employees | Own row only |
| departments | All rows | Own department | Own department |
| users | Own row | Own row | Own row |

**Example RLS Policy (Employee table):**
```sql
-- Employee can only see their own record
CREATE POLICY "employee_own_row" ON employees
FOR SELECT USING (
  auth.uid() = user_id
  OR
  EXISTS (
    SELECT 1 FROM users
    WHERE users.id = auth.uid()
    AND users.role IN ('ceo', 'manager', 'tester')
  )
);
```

---

## 7. User Creation Process

Since Version 1 has no admin panel, users are created manually in Supabase:

1. Add email and password via Supabase Auth dashboard
2. Insert a row into the `users` table with the user's ID, role, and linked employee ID
3. The application reads the role on login and redirects accordingly

In Version 2, an admin panel will allow user creation and role assignment from within the app.

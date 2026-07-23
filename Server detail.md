---

## 1. Client Environment Variables

These variables are exposed to the browser and should only contain public information.

```env
NEXT_PUBLIC_SUPABASE_URL="https://tfsdrsthgcnjajwnqixp.supabase.co"
NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY="sb_publishable_7ZmKaHswoiw3RowbBuzkZA_tUqS5CuD"
```

---

## 2. Server Environment Variables

These variables should only be used on the server.

```env
SUPABASE_URL="https://tfsdrsthgcnjajwnqixp.supabase.co"
SUPABASE_PUBLISHABLE_KEY="sb_publishable_7ZmKaHswoiw3RowbBuzkZA_tUqS5CuD"
SUPABASE_SECRET_KEY="sb_secret_mrAI_UAgfqcZ-3Gr40NJzw_ezp7PnXR"
SUPABASE_JWKS_URL="https://tfsdrsthgcnjajwnqixp.supabase.co/auth/v1/.well-known/jwks.json"
```

---

## 3. Direct PostgreSQL Connection

Use these credentials when connecting directly to the PostgreSQL database (pgAdmin, DBeaver, VS Code extensions, etc.).

```text
Connection String = "postgresql://postgres:IBMAPP@2026@db.tfsdrsthgcnjajwnqixp.supabase.co:5432/postgres"

Host     = "db.tfsdrsthgcnjajwnqixp.supabase.co"
Port     = "5432"
Database = "postgres"
User     = "postgres"
Password = "IBMAPP@2026"
```
---

## 4. Session pooler

Only recommended as an alternative to Direct Connection, when connecting via an IPv4 network.
```text
Connection String = "postgresql://postgres.tfsdrsthgcnjajwnqixp:IBMAPP@2026@aws-1-ap-southeast-1.pooler.supabase.com:5432/postgres"

host="aws-1-ap-southeast-1.pooler.supabase.com"
port="5432"
database="postgres"
user="postgres.tfsdrsthgcnjajwnqixp"
```

---

---

## 5. Transaction pooler

Ideal for stateless applications like serverless functions where each interaction with Postgres is brief and isolated.
```text
Connection String = "postgresql://postgres.tfsdrsthgcnjajwnqixp:IBMAPP@2026@aws-1-ap-southeast-1.pooler.supabase.com:6543/postgres"

host="aws-1-ap-southeast-1.pooler.supabase.com"
port="5432"
database="postgres"
user="postgres.tfsdrsthgcnjajwnqixp"
```

---

## 4. Prisma / ORM Connection

### Transaction Pooler (Recommended for Applications)

Used by the application for normal database queries.

```env
Connect to Postgres via the shared transaction-mode pooler (IPv4-only)
DATABASE_URL="postgresql://postgres.tfsdrsthgcnjajwnqixp:IBMAPP@2026@aws-1-ap-southeast-1.pooler.supabase.com:6543/postgres?pgbouncer=true"
```

---

### Drizzle Connection  
```
# Connect to Postgres via the shared transaction-mode pooler (IPv4-only)
DATABASE_URL="postgresql://postgres.tfsdrsthgcnjajwnqixp:IBMAPP@2026@aws-1-ap-southeast-1.pooler.supabase.com:6543/postgres"
```

### Session Pooler (Recommended for Migrations)

Used for database migrations.

```env
Connect to Postgres via the shared session-mode pooler (used for migrations)
DIRECT_URL="postgresql://postgres.tfsdrsthgcnjajwnqixp:IBMAPP@2026@aws-1-ap-southeast-1.pooler.supabase.com:5432/postgres"
```

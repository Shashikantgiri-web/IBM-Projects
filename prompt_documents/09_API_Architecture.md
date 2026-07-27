# 09 — API Architecture
## Employee Performance Intelligence System (EPIS)

---

## 1. Overview

All API endpoints are Next.js Route Handlers located in the `/app/api/` directory. Every endpoint verifies the Supabase JWT before returning data. Supabase RLS provides a second security layer.

**Base URL (Development):** `http://localhost:3000/api`  
**Base URL (Production):** `https://epis.vercel.app/api`

---

## 2. Authentication Header

All protected endpoints require a valid session. The Supabase client automatically handles the session cookie in Next.js. Server-side route handlers use:

```typescript
const supabase = createRouteHandlerClient({ cookies })
const { data: { session } } = await supabase.auth.getSession()
if (!session) return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
```

---

## 3. Standard Response Format

### Success
```json
{
  "data": [...],
  "count": 1470,
  "status": "success"
}
```

### Error
```json
{
  "error": "Unauthorized",
  "message": "You do not have access to this resource.",
  "status": 401
}
```

---

## 4. Endpoints

---

### 4.1 Authentication

#### POST /api/auth/login
Login is handled directly by Supabase Auth client (`supabase.auth.signInWithPassword`). No custom endpoint needed.

#### POST /api/auth/logout
Logout is handled by Supabase Auth client (`supabase.auth.signOut`). No custom endpoint needed.

#### POST /api/auth/forgot-password
Forgot password is handled by Supabase Auth client (`supabase.auth.resetPasswordForEmail`). No custom endpoint needed.

---

### 4.2 Dashboard KPIs

#### GET /api/dashboard/kpis
Returns top-level KPI values for the dashboard.

**Auth:** Required  
**Roles:** CEO (all), Manager (own department), Tester (all)

**Query Parameters:**
| Param | Type | Required | Description |
|---|---|---|---|
| department_id | UUID | No | Filter by department |

**Response:**
```json
{
  "data": {
    "total_employees": 1470,
    "attrition_count": 237,
    "attrition_rate": 16.12,
    "avg_monthly_salary": 6503,
    "avg_performance_rating": 3.15,
    "retained_count": 1233
  },
  "status": "success"
}
```

---

### 4.3 Employees

#### GET /api/employees
Returns a paginated list of employees.

**Auth:** Required  
**Roles:** CEO (all), Manager (own department), Tester (all)

**Query Parameters:**
| Param | Type | Required | Description |
|---|---|---|---|
| page | INT | No | Page number (default: 1) |
| limit | INT | No | Rows per page (default: 25) |
| department_id | UUID | No | Filter by department |
| job_role_id | UUID | No | Filter by job role |
| gender | VARCHAR | No | Filter by gender |
| attrition | BOOLEAN | No | Filter by attrition status |
| search | VARCHAR | No | Search by name |

**Response:**
```json
{
  "data": [
    {
      "id": "uuid",
      "employee_number": "EMP001",
      "first_name": "Alice",
      "last_name": "Smith",
      "department": "Sales",
      "job_role": "Sales Executive",
      "gender": "Female",
      "age": 35,
      "attrition": false,
      "performance_rating": 3,
      "monthly_income": 5993
    }
  ],
  "count": 1470,
  "page": 1,
  "limit": 25,
  "status": "success"
}
```

---

#### GET /api/employees/[id]
Returns a single employee's full profile.

**Auth:** Required  
**Roles:** CEO (any), Manager (own dept only), Employee (own record only), Tester (any)

**Response:**
```json
{
  "data": {
    "id": "uuid",
    "employee_number": "EMP001",
    "first_name": "Alice",
    "last_name": "Smith",
    "age": 35,
    "gender": "Female",
    "marital_status": "Married",
    "department": "Sales",
    "job_role": "Sales Executive",
    "education_level": "Bachelor",
    "attrition": false,
    "over_time": true,
    "distance_from_home": 12,
    "years_at_company": 7,
    "years_in_current_role": 3,
    "years_since_last_promotion": 1,
    "years_with_curr_manager": 4,
    "total_working_years": 12,
    "num_companies_worked": 2,
    "salary": {
      "monthly_income": 5993,
      "percent_salary_hike": 14,
      "stock_option_level": 1
    },
    "performance": {
      "performance_rating": 3,
      "job_involvement": 3,
      "business_travel": "Travel_Rarely"
    },
    "training": {
      "training_times_last_year": 3
    },
    "satisfaction": {
      "job_satisfaction": 4,
      "environment_satisfaction": 3,
      "relationship_satisfaction": 3,
      "work_life_balance": 3
    }
  },
  "status": "success"
}
```

---

### 4.4 Departments

#### GET /api/departments
Returns list of departments with summary stats.

**Auth:** Required  
**Roles:** CEO, Tester

**Response:**
```json
{
  "data": [
    {
      "id": "uuid",
      "name": "Sales",
      "employee_count": 446,
      "attrition_count": 92,
      "attrition_rate": 20.63,
      "avg_salary": 6959,
      "avg_performance": 3.2
    }
  ],
  "status": "success"
}
```

---

### 4.5 Charts

#### GET /api/charts/attrition-by-department
**Auth:** Required | **Roles:** CEO, Manager (own dept), Tester

**Response:**
```json
{
  "data": [
    { "department": "Human Resources", "attrition_rate": 19.05, "count": 12 },
    { "department": "Research & Development", "attrition_rate": 13.84, "count": 133 },
    { "department": "Sales", "attrition_rate": 20.63, "count": 92 }
  ],
  "status": "success"
}
```

---

#### GET /api/charts/attrition-by-age
**Auth:** Required | **Roles:** CEO, Manager, Tester

**Response:**
```json
{
  "data": [
    { "age_group": "18-25", "attrition_count": 38, "total": 85, "rate": 44.7 },
    { "age_group": "26-35", "attrition_count": 112, "total": 554, "rate": 20.2 },
    { "age_group": "36-45", "attrition_count": 51, "total": 513, "rate": 9.9 },
    { "age_group": "46-55", "attrition_count": 25, "total": 253, "rate": 9.9 },
    { "age_group": "56+",   "attrition_count": 11, "total": 65, "rate": 16.9 }
  ],
  "status": "success"
}
```

---

#### GET /api/charts/salary-by-department
**Auth:** Required | **Roles:** CEO, Tester

**Response:**
```json
{
  "data": [
    { "department": "Human Resources", "avg_salary": 6654, "min_salary": 1555, "max_salary": 19999 },
    { "department": "Research & Development", "avg_salary": 6281, "min_salary": 1009, "max_salary": 19999 },
    { "department": "Sales", "avg_salary": 6959, "min_salary": 1052, "max_salary": 19999 }
  ],
  "status": "success"
}
```

---

#### GET /api/charts/performance-distribution
**Auth:** Required | **Roles:** CEO, Manager, Tester

**Query Params:** `department_id` (optional)

**Response:**
```json
{
  "data": [
    { "rating": 1, "label": "Low",         "count": 56  },
    { "rating": 2, "label": "Good",        "count": 230 },
    { "rating": 3, "label": "Excellent",   "count": 828 },
    { "rating": 4, "label": "Outstanding", "count": 356 }
  ],
  "status": "success"
}
```

---

#### GET /api/charts/training-vs-performance
**Auth:** Required | **Roles:** CEO, Manager, Tester

**Response:**
```json
{
  "data": [
    { "training_times": 0, "avg_performance": 2.9 },
    { "training_times": 1, "avg_performance": 3.0 },
    { "training_times": 2, "avg_performance": 3.1 },
    { "training_times": 3, "avg_performance": 3.2 },
    { "training_times": 4, "avg_performance": 3.1 },
    { "training_times": 5, "avg_performance": 3.0 },
    { "training_times": 6, "avg_performance": 2.9 }
  ],
  "status": "success"
}
```

---

#### GET /api/charts/gender-distribution
**Auth:** Required | **Roles:** CEO, Manager, Tester

**Response:**
```json
{
  "data": [
    { "gender": "Male",   "count": 882, "percentage": 60 },
    { "gender": "Female", "count": 588, "percentage": 40 }
  ],
  "status": "success"
}
```

---

#### GET /api/charts/satisfaction-by-department
**Auth:** Required | **Roles:** CEO, Manager, Tester

**Response:**
```json
{
  "data": [
    { "department": "Sales", "avg_job_satisfaction": 2.7, "avg_env_satisfaction": 2.8, "avg_work_life_balance": 2.7 }
  ],
  "status": "success"
}
```

---

### 4.6 Export

#### GET /api/export/employees
Returns a CSV file download of the current dataset.

**Auth:** Required  
**Roles:** CEO, Manager (own dept only)

**Query Parameters:** Same as `GET /api/employees` (respects filters)

**Response:**
- Content-Type: `text/csv`
- Content-Disposition: `attachment; filename="epis_export_2025-01-15.csv"`

---

### 4.7 User Profile

#### GET /api/user/profile
Returns the current user's profile info.

**Auth:** Required | **Roles:** All

**Response:**
```json
{
  "data": {
    "id": "uuid",
    "email": "alice@company.com",
    "display_name": "Alice Smith",
    "role": "employee",
    "employee_id": "uuid"
  },
  "status": "success"
}
```

#### PATCH /api/user/profile
Updates the current user's display name.

**Auth:** Required | **Roles:** All

**Body:**
```json
{ "display_name": "Alice S." }
```

---

## 5. HTTP Status Codes Used

| Code | Meaning |
|---|---|
| 200 | Success |
| 201 | Created |
| 400 | Bad request (validation error) |
| 401 | Unauthorized (no valid session) |
| 403 | Forbidden (role does not have access) |
| 404 | Resource not found |
| 500 | Server error |

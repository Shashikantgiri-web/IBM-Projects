# 08 — Database Reference
## Employee Performance Intelligence System (EPIS)

---

## 1. Overview

The database was populated by the Data Engineering team using a Python ETL pipeline from the final validated IBM HR Analytics dataset. **This document describes the schema as it exists — no schema changes are planned for Version 1.**

Database: PostgreSQL hosted on Supabase  
Total Tables: 13

---

## 2. Table Overview

| Table | Description |
|---|---|
| employees | Core employee records |
| departments | Department definitions |
| roles | Job roles |
| education | Education level reference |
| salary | Salary records per employee |
| performance | Performance ratings |
| training | Training data per employee |
| attendance | Attendance and overtime records |
| satisfaction | Satisfaction survey scores |
| promotion | Promotion history |
| experience | Work experience details |
| users | Application users linked to employees |
| audit_logs | Login and action logs |

---

## 3. Table Schemas

### 3.1 employees
The main table. One row per employee.

| Column | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| employee_number | VARCHAR(20) | Employee ID (e.g. EMP001) |
| first_name | VARCHAR(100) | First name |
| last_name | VARCHAR(100) | Last name |
| age | INT | Age in years |
| gender | VARCHAR(10) | Male / Female |
| marital_status | VARCHAR(20) | Single / Married / Divorced |
| department_id | UUID | FK → departments.id |
| job_role_id | UUID | FK → roles.id |
| education_id | UUID | FK → education.id |
| attrition | BOOLEAN | TRUE = left the company |
| over18 | VARCHAR(5) | Y / N |
| standard_hours | INT | Standard working hours |
| over_time | BOOLEAN | Works overtime |
| distance_from_home | INT | Distance in km |
| num_companies_worked | INT | Number of previous employers |
| total_working_years | INT | Total career years |
| years_at_company | INT | Years with this company |
| years_in_current_role | INT | Years in current role |
| years_since_last_promotion | INT | Years since last promotion |
| years_with_curr_manager | INT | Years under current manager |
| created_at | TIMESTAMPTZ | Row creation timestamp |

---

### 3.2 departments

| Column | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| name | VARCHAR(100) | Department name |
| manager_user_id | UUID | FK → users.id (manager's user account) |

**Values:** Human Resources, Research & Development, Sales

---

### 3.3 roles (Job Roles)

| Column | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| name | VARCHAR(100) | Job role title |
| department_id | UUID | FK → departments.id |

**Sample values:** Healthcare Representative, Human Resources, Laboratory Technician, Manager, Manufacturing Director, Research Director, Research Scientist, Sales Executive, Sales Representative

---

### 3.4 education

| Column | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| level | INT | 1–5 |
| label | VARCHAR(50) | Below College, College, Bachelor, Master, Doctor |

---

### 3.5 salary

| Column | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| employee_id | UUID | FK → employees.id |
| monthly_income | INT | Monthly salary (in USD) |
| daily_rate | INT | Daily rate |
| hourly_rate | INT | Hourly rate |
| monthly_rate | INT | Monthly rate |
| percent_salary_hike | INT | Last salary hike percentage |
| stock_option_level | INT | 0–3 |

---

### 3.6 performance

| Column | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| employee_id | UUID | FK → employees.id |
| performance_rating | INT | 1–4 (1=Low, 4=Outstanding) |
| job_involvement | INT | 1–4 |
| business_travel | VARCHAR(50) | Non-Travel, Travel_Rarely, Travel_Frequently |
| recorded_at | TIMESTAMPTZ | Date of rating |

---

### 3.7 training

| Column | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| employee_id | UUID | FK → employees.id |
| training_times_last_year | INT | Number of trainings attended |
| training_hours | INT | Total hours (if tracked) |

---

### 3.8 attendance

| Column | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| employee_id | UUID | FK → employees.id |
| over_time | BOOLEAN | Regular overtime |
| standard_hours | INT | Standard hours per week |

---

### 3.9 satisfaction

| Column | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| employee_id | UUID | FK → employees.id |
| job_satisfaction | INT | 1–4 (1=Low, 4=Very High) |
| environment_satisfaction | INT | 1–4 |
| relationship_satisfaction | INT | 1–4 |
| work_life_balance | INT | 1–4 (1=Bad, 4=Best) |

---

### 3.10 promotion

| Column | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| employee_id | UUID | FK → employees.id |
| years_since_last_promotion | INT | Synced from employees table |
| last_promotion_date | DATE | Estimated date if available |

---

### 3.11 experience

| Column | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| employee_id | UUID | FK → employees.id |
| num_companies_worked | INT | Previous employers |
| total_working_years | INT | Total career years |
| years_at_company | INT | Years at this company |
| years_in_current_role | INT | Years in current role |
| years_with_curr_manager | INT | Years with current manager |

---

### 3.12 users
Application users — separate from the employees table.

| Column | Type | Description |
|---|---|---|
| id | UUID | Primary key = Supabase Auth user ID |
| employee_id | UUID | FK → employees.id (null for non-employee users) |
| email | VARCHAR(255) | Login email |
| role | VARCHAR(20) | ceo / manager / employee / tester |
| department_id | UUID | FK → departments.id (for managers) |
| display_name | VARCHAR(100) | Display name shown in UI |
| created_at | TIMESTAMPTZ | Account creation date |

---

### 3.13 audit_logs

| Column | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| user_id | UUID | FK → users.id |
| action | VARCHAR(100) | login, logout, export, view_employee |
| metadata | JSONB | Additional context (IP, filters used, etc.) |
| created_at | TIMESTAMPTZ | Timestamp of action |

---

## 4. Key Relationships

```
employees ─────────────────────── departments
employees ─────────────────────── roles
employees ─────────────────────── education
employees ──┬────────────────────── salary
            ├────────────────────── performance
            ├────────────────────── training
            ├────────────────────── attendance
            ├────────────────────── satisfaction
            ├────────────────────── promotion
            └────────────────────── experience
users ──────────────────────────── employees (1:1)
users ──────────────────────────── departments (managers)
users ──────────────────────────── audit_logs
```

---

## 5. Common Queries (Reference)

### Get all employees with their department and role
```sql
SELECT 
  e.employee_number,
  e.first_name,
  e.last_name,
  d.name AS department,
  r.name AS job_role,
  e.attrition
FROM employees e
JOIN departments d ON e.department_id = d.id
JOIN roles r ON e.job_role_id = r.id;
```

### Get attrition rate by department
```sql
SELECT 
  d.name AS department,
  COUNT(*) AS total,
  SUM(CASE WHEN e.attrition = TRUE THEN 1 ELSE 0 END) AS attrited,
  ROUND(
    SUM(CASE WHEN e.attrition = TRUE THEN 1 ELSE 0 END)::NUMERIC / COUNT(*) * 100, 2
  ) AS attrition_rate
FROM employees e
JOIN departments d ON e.department_id = d.id
GROUP BY d.name;
```

### Get employee performance with salary
```sql
SELECT
  e.first_name,
  e.last_name,
  p.performance_rating,
  s.monthly_income,
  t.training_times_last_year
FROM employees e
JOIN performance p ON p.employee_id = e.id
JOIN salary s ON s.employee_id = e.id
JOIN training t ON t.employee_id = e.id;
```

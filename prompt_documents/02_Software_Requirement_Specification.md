# 02 — Software Requirement Specification (SRS)
## Employee Performance Intelligence System (EPIS)

---

## 1. Purpose

This document defines all functional and non-functional requirements for EPIS Version 1.0. It serves as the contract between what is planned and what will be built.

---

## 2. Functional Requirements

### 2.1 Authentication

| ID | Requirement |
|---|---|
| FR-AUTH-01 | Users must log in with email and password |
| FR-AUTH-02 | Supabase Auth handles all authentication logic |
| FR-AUTH-03 | A JWT token is issued on successful login |
| FR-AUTH-04 | Sessions persist across page refreshes |
| FR-AUTH-05 | Users can log out, which clears the session |
| FR-AUTH-06 | Forgot password sends a reset link via email |
| FR-AUTH-07 | Reset password link expires after 1 hour |
| FR-AUTH-08 | Failed login shows an appropriate error message |
| FR-AUTH-09 | Login page is accessible to unauthenticated users only |

### 2.2 Authorization & Role-Based Access

| ID | Requirement |
|---|---|
| FR-ROLE-01 | System supports four roles: CEO, Manager, Employee, Tester |
| FR-ROLE-02 | Role is stored in the users table in Supabase |
| FR-ROLE-03 | Middleware reads role from JWT and redirects accordingly |
| FR-ROLE-04 | CEO can access all departments and employees |
| FR-ROLE-05 | Manager can only access their own department |
| FR-ROLE-06 | Employee can only access their own profile and data |
| FR-ROLE-07 | Tester has access to all views in read-only mode |
| FR-ROLE-08 | Unauthorized route access redirects to /unauthorized |

### 2.3 CEO Dashboard

| ID | Requirement |
|---|---|
| FR-CEO-01 | View total employee count |
| FR-CEO-02 | View organization-wide attrition rate |
| FR-CEO-03 | View attrition count and retained count |
| FR-CEO-04 | View average monthly salary across all departments |
| FR-CEO-05 | View average performance rating |
| FR-CEO-06 | View department-wise employee distribution (chart) |
| FR-CEO-07 | View attrition by department (chart) |
| FR-CEO-08 | View salary distribution across departments (chart) |
| FR-CEO-09 | View performance rating distribution (chart) |
| FR-CEO-10 | View attrition by age group (chart) |
| FR-CEO-11 | View training hours vs performance correlation (chart) |
| FR-CEO-12 | Filter all data by department |
| FR-CEO-13 | Filter all data by gender |
| FR-CEO-14 | Filter all data by job role |
| FR-CEO-15 | Export filtered data as CSV |

### 2.4 Manager Dashboard

| ID | Requirement |
|---|---|
| FR-MGR-01 | View employees within their department only |
| FR-MGR-02 | View department-level attrition rate |
| FR-MGR-03 | View department average salary |
| FR-MGR-04 | View department average performance rating |
| FR-MGR-05 | View employee list with search and filter |
| FR-MGR-06 | Click on an employee to view their detailed profile |
| FR-MGR-07 | View training completion rate for department |
| FR-MGR-08 | View job satisfaction scores by employee |
| FR-MGR-09 | Filter employees by job role, gender, education |

### 2.5 Employee Dashboard

| ID | Requirement |
|---|---|
| FR-EMP-01 | View own profile information |
| FR-EMP-02 | View own performance rating history |
| FR-EMP-03 | View own salary information |
| FR-EMP-04 | View own training data and completed hours |
| FR-EMP-05 | View own job satisfaction and environment scores |
| FR-EMP-06 | View own years at company, in role, since last promotion |
| FR-EMP-07 | Cannot view other employees' data |

### 2.6 Employee Search & Filtering

| ID | Requirement |
|---|---|
| FR-SRCH-01 | Search employees by name |
| FR-SRCH-02 | Filter by department |
| FR-SRCH-03 | Filter by job role |
| FR-SRCH-04 | Filter by gender |
| FR-SRCH-05 | Filter by education level |
| FR-SRCH-06 | Filter by attrition status |
| FR-SRCH-07 | Results update without full page reload |

### 2.7 Settings & Profile

| ID | Requirement |
|---|---|
| FR-SET-01 | User can update display name |
| FR-SET-02 | User can toggle dark/light mode |
| FR-SET-03 | User can change password via Supabase Auth |
| FR-SET-04 | Theme preference is saved in localStorage |

### 2.8 Export

| ID | Requirement |
|---|---|
| FR-EXP-01 | CEO and Manager can export currently filtered data as CSV |
| FR-EXP-02 | Exported CSV includes only columns relevant to the filtered view |
| FR-EXP-03 | Filename includes the current date |

### 2.9 Error Handling

| ID | Requirement |
|---|---|
| FR-ERR-01 | 404 page displays for invalid routes |
| FR-ERR-02 | Unauthorized page displays for access violations |
| FR-ERR-03 | API errors show toast notifications |
| FR-ERR-04 | Loading skeletons appear while data fetches |
| FR-ERR-05 | Empty state messages display when no data is returned |

---

## 3. Non-Functional Requirements

### 3.1 Performance

| ID | Requirement |
|---|---|
| NFR-PERF-01 | Dashboard pages must load within 3 seconds on a standard connection |
| NFR-PERF-02 | Chart renders must complete within 1 second of data load |
| NFR-PERF-03 | API responses must return within 500ms for standard queries |
| NFR-PERF-04 | Images and assets must be optimized via Next.js Image component |

### 3.2 Security

| ID | Requirement |
|---|---|
| NFR-SEC-01 | All API routes must verify JWT before returning data |
| NFR-SEC-02 | Supabase Row Level Security (RLS) must be enabled on all tables |
| NFR-SEC-03 | Environment variables must never be exposed to the client |
| NFR-SEC-04 | HTTPS must be enforced in production |
| NFR-SEC-05 | Passwords are never stored in plain text (Supabase Auth handles this) |
| NFR-SEC-06 | XSS protection via React's JSX escaping |

### 3.3 Usability

| ID | Requirement |
|---|---|
| NFR-USE-01 | Application is fully usable on screens 375px and above |
| NFR-USE-02 | Dark mode is available on all pages |
| NFR-USE-03 | Color contrast meets WCAG AA standard |
| NFR-USE-04 | All interactive elements have visible focus states |
| NFR-USE-05 | Loading states are always visible during async operations |

### 3.4 Browser Support

| Browser | Minimum Version |
|---|---|
| Chrome | 110+ |
| Firefox | 110+ |
| Safari | 16+ |
| Edge | 110+ |
| Mobile Chrome | Latest |
| Mobile Safari | Latest |

### 3.5 Availability

| ID | Requirement |
|---|---|
| NFR-AVL-01 | Application targets 99% uptime via Vercel hosting |
| NFR-AVL-02 | Supabase free tier availability is acceptable for Version 1 |

---

## 4. Constraints

- Database schema is fixed (ETL is complete — no schema changes in Version 1)
- Authentication is handled entirely by Supabase Auth (no custom auth server)
- Power BI dashboards remain separate from the web app (not embedded in Version 1)
- Application must work without requiring users to install anything

---

## 5. Success Criteria

Version 1.0 is considered complete when:

1. All four user roles can log in and reach their respective dashboards
2. Data displayed matches the validated Supabase database
3. No user can access data outside their role permissions
4. Application is live on Vercel with a public URL
5. All major browsers and mobile devices work correctly
6. All 14 testing scenarios from the Testing Strategy pass

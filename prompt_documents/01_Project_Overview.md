# 01 — Project Overview
## Employee Performance Intelligence System (EPIS)

---

## 1. Project Vision

EPIS is an enterprise web application that delivers employee performance analytics to organizational stakeholders through a secure, role-based interface. It is built on a validated IBM HR Analytics dataset processed by a dedicated Data Analysis team and loaded into a Supabase PostgreSQL database via a Python ETL pipeline.

The goal is to give CEOs, Managers, and Employees a single, secure web platform where performance data, attrition risk, training effectiveness, salary analysis, and departmental KPIs are instantly accessible — without needing to open Power BI or Excel.

---

## 2. Problem Statement

Organizations with large workforces struggle to:

- Give executives a fast, reliable view of overall workforce health
- Help managers track department-level performance and attrition risk
- Allow employees to view their own performance history and growth
- Control who can see sensitive HR data (salary, attrition probability)
- Replace static report sharing (email, PDF) with a live, secure web platform

EPIS solves all of the above in one application.

---

## 3. Project Background

This project is built on the IBM HR Employee Attrition & Performance dataset. The analysis team cleaned, validated, and enriched the data, built Power BI dashboards with DAX measures, and used Python to push the final analytical dataset into Supabase. The web development team now builds the application on top of that database.

**Note:** EPIS is inspired by IBM's HR analytics dataset and uses an IBM-style enterprise design language. The application itself is an independent product.

---

## 4. Goals

**Primary Goals:**
- Deliver a role-based web dashboard with employee performance insights
- Provide secure authentication with Supabase Auth and JWT
- Implement row-level security so users only see data they are authorized to view
- Replicate key Power BI KPIs inside the web interface using interactive charts
- Support mobile and desktop responsiveness

**Secondary Goals:**
- Provide data export functionality (CSV download)
- Implement dark mode and light mode
- Enable employee search and department filtering
- Build a profile management page for each user
- Lay the foundation for future AI-powered insights (Version 2)

---

## 5. Objectives

| Objective | Success Criteria |
|---|---|
| Secure login for all roles | Login works, JWT is issued, session persists |
| CEO can view all departments | CEO dashboard loads with full org data |
| Manager sees only their department | Data filtered correctly by department |
| Employee sees only their own profile | No cross-user data leakage |
| Charts load accurately | Data matches Power BI output |
| Mobile responsive | Works on 375px and above |
| Dark mode | Toggle saves preference |
| Export | CSV downloads correctly |

---

## 6. Scope

### In Scope (Version 1)
- Authentication (login, logout, forgot password)
- Role-based dashboards (CEO, Manager, Employee)
- Performance charts and KPI cards
- Department and employee filtering
- Profile and settings pages
- CSV data export
- Dark / light mode
- Responsive design
- Error pages (404, unauthorized)
- Deployment to Vercel

### Out of Scope (Version 1)
- AI chat / natural language queries
- Real-time data updates (live sync)
- Email notifications
- Admin panel for user management
- Mobile native application
- Power BI embedded in the web app

---

## 7. Users & Stakeholders

### Application Users
| Role | Description |
|---|---|
| CEO | Views organization-wide performance, attrition, salary, and KPIs |
| Department Manager | Views department-specific performance and employee data |
| Employee | Views personal performance history, training, and profile |
| QA Tester | Tests all features across all roles |

### Project Stakeholders
| Role | Responsibility |
|---|---|
| Lead Developer | Application architecture, coding, deployment |
| Data Analysis Team | Dataset, Power BI, KPIs, DAX measures (completed) |
| Data Engineer | Python ETL pipeline and Supabase setup (completed) |
| Project Manager | Timeline, documentation, QA coordination |

---

## 8. Technologies

| Category | Technology | Reason |
|---|---|---|
| Framework | Next.js 14 | App Router, SSR, API routes |
| Styling | Tailwind CSS | Rapid UI development |
| UI Library | Shadcn/UI | Accessible, customizable components |
| Charts | Recharts | React-native charting |
| Auth | Supabase Auth | JWT, session management, RLS |
| Database | PostgreSQL via Supabase | Already populated via ETL |
| Deployment | Vercel | Native Next.js hosting |
| Version Control | Git + GitHub | Branch-based workflow |

---

## 9. Project Timeline (Estimated)

| Phase | Duration | Deliverable |
|---|---|---|
| Documentation | Week 1 | All 19 documents complete |
| Setup | Week 2 | Next.js + Supabase + Auth working |
| CEO Dashboard | Week 3 | CEO view with charts |
| Manager & Employee | Week 4 | All dashboards complete |
| Polish & Testing | Week 5 | Responsive, dark mode, export |
| Deployment | Week 6 | Live on Vercel |

---

## 10. Version Roadmap Summary

| Version | Focus |
|---|---|
| 1.0 | Core dashboards, auth, roles, charts |
| 1.1 | Export, notifications, advanced filters |
| 2.0 | AI insights, admin panel, real-time data |

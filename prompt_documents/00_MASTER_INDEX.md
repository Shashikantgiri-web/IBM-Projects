# 00 — Master Index
## Employee Performance Intelligence System (EPIS)
**Version:** 1.0  
**Last Updated:** 2025  
**Status:** Pre-Development — Documentation Phase Complete

---

## Project Summary

EPIS is an enterprise-grade web application built on top of an existing IBM HR Analytics dataset. The Data Analysis team has completed all analytical work including Power BI dashboards, DAX measures, and KPI definitions. The Python ETL pipeline has loaded the final validated dataset into Supabase (PostgreSQL). The web development phase now begins.

The application provides role-based access to employee performance insights for CEOs, Managers, and Employees within an organization.

---

## Technology Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js 14 (App Router) |
| Styling | Tailwind CSS |
| UI Components | Shadcn/UI |
| Charts | Recharts |
| Backend / Auth | Supabase |
| Database | PostgreSQL (via Supabase) |
| Deployment | Vercel |
| Data Pipeline | Python ETL (completed) |
| Analytics | Power BI (completed) |

---

## Current Project Status

| Area | Status |
|---|---|
| Raw Dataset | ✅ Complete |
| Data Cleaning (Excel) | ✅ Complete |
| Power BI Dashboards | ✅ Complete |
| DAX Measures & KPIs | ✅ Complete |
| Python ETL Pipeline | ✅ Complete |
| Supabase Database | ✅ Complete |
| Documentation | ✅ Complete |
| Web Application | 🔲 Not Started |

---

## Documentation Map

### Phase 1 — Foundation
| # | Document | Purpose |
|---|---|---|
| 01 | Project Overview | Vision, goals, scope, stakeholders |
| 02 | Software Requirement Specification | All functional and non-functional requirements |
| 03 | System Architecture | How all components connect |

### Phase 2 — Application Design
| # | Document | Purpose |
|---|---|---|
| 04 | User Roles & Permissions | Roles, access rules, route protection |
| 05 | Features Requirement Document | Every feature with acceptance criteria |
| 06 | UI/UX Design System | Colors, typography, components |
| 07 | Navigation & Routing | All routes and navigation guards |

### Phase 3 — Backend
| # | Document | Purpose |
|---|---|---|
| 08 | Database Reference | Existing Supabase schema documented |
| 09 | API Architecture | Every endpoint, request, response |
| 10 | State Management | Auth, theme, filters, loading states |

### Phase 4 — Frontend
| # | Document | Purpose |
|---|---|---|
| 11 | Component Architecture | Every React component and hierarchy |
| 12 | Page Wireframes | Layout structure for every page |
| 13 | Chart Specification | Every chart defined precisely |

### Phase 5 — Quality
| # | Document | Purpose |
|---|---|---|
| 14 | Testing Strategy | All test types and test cases |
| 15 | Security Document | Auth, RLS, XSS, CSRF, rate limiting |

### Phase 6 — Deployment & Development
| # | Document | Purpose |
|---|---|---|
| 16 | Deployment Guide | Local, staging, production setup |
| 17 | Development Roadmap | Version milestones and build order |
| 18 | Coding Guidelines | Folder structure, naming, Git workflow |

---

## Folder Structure

```
epis/
├── app/
│   ├── (auth)/
│   │   ├── login/
│   │   ├── signup/
│   │   └── forgot-password/
│   ├── (dashboard)/
│   │   ├── ceo/
│   │   ├── manager/
│   │   └── employee/
│   ├── about/
│   ├── settings/
│   ├── profile/
│   └── not-found/
├── components/
│   ├── ui/
│   ├── charts/
│   ├── layout/
│   └── shared/
├── features/
│   ├── authentication/
│   ├── dashboard/
│   ├── employees/
│   ├── departments/
│   └── analytics/
├── lib/
├── hooks/
├── services/
├── types/
├── utils/
├── database/
├── docs/
└── public/
```

---

## Next Tasks (Development Order)

1. Set up Next.js project with Tailwind and Shadcn
2. Configure Supabase client and environment variables
3. Implement authentication flow (login, logout, session)
4. Build middleware for route protection
5. Build CEO Dashboard layout and charts
6. Build Manager Dashboard
7. Build Employee Dashboard
8. Add filters, search, export
9. Testing and QA
10. Deployment to Vercel

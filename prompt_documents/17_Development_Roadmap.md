# 17 — Development Roadmap
## Employee Performance Intelligence System (EPIS)

---

## 1. Overview

This roadmap defines the exact build order for Version 1.0, plus what comes after. Following this order avoids rework — each step builds on what the previous step established.

---

## 2. Version 1.0 — Core Application

**Goal:** A fully working, deployed application with authentication, role-based dashboards, charts, and data export.

**Estimated Duration:** 6 weeks

---

### Week 1 — Project Setup & Foundation

**Milestone:** Running Next.js app connected to Supabase with working authentication.

| Task | Priority | Notes |
|---|---|---|
| Initialize Next.js 14 project with App Router | 🔴 Critical | `npx create-next-app@latest epis` |
| Install and configure Tailwind CSS | 🔴 Critical | `npx shadcn-ui@latest init` |
| Install Shadcn/UI components | 🔴 Critical | Button, Card, Input, Dropdown, Table, Toast |
| Install Recharts | 🔴 Critical | `npm install recharts` |
| Install SWR | 🔴 Critical | `npm install swr` |
| Install Lucide React icons | 🔴 Critical | `npm install lucide-react` |
| Set up Supabase client | 🔴 Critical | `/lib/supabase/client.ts` and `server.ts` |
| Configure environment variables | 🔴 Critical | `.env.local` with Supabase keys |
| Build login page | 🔴 Critical | Email + password form |
| Build forgot password page | 🔴 Critical | Email submission + confirmation |
| Implement Supabase Auth (login / logout) | 🔴 Critical | `supabase.auth.signInWithPassword` |
| Build AuthContext | 🔴 Critical | Session + role available app-wide |
| Build ThemeContext | 🔴 Critical | Dark/light mode with localStorage |
| Build Next.js middleware | 🔴 Critical | Route protection + role redirects |
| Build root layout with providers | 🔴 Critical | AuthProvider + ThemeProvider + Toaster |
| Set up Git repository and branching strategy | 🔴 Critical | main + dev + feature/* branches |

**Definition of Done — Week 1:**
- CEO can log in and is redirected to /dashboard/ceo (even if the page is blank)
- Manager is redirected to /dashboard/manager
- Employee is redirected to /dashboard/employee
- Logging out clears session and redirects to /login
- Visiting a protected route without auth redirects to /login

---

### Week 2 — Layout & Shared Components

**Milestone:** Dashboard shell with navbar, sidebar, and all reusable components built.

| Task | Priority | Notes |
|---|---|---|
| Build DashboardLayout | 🔴 Critical | Navbar + Sidebar + Main content area |
| Build Navbar | 🔴 Critical | Logo, nav links, theme toggle, user menu |
| Build Sidebar | 🔴 Critical | Role-specific links, collapsible |
| Build PageHeader | 🔴 Critical | Title + breadcrumbs |
| Build KPICard | 🔴 Critical | Value, icon, trend, loading skeleton |
| Build KPIGrid | 🔴 Critical | Responsive 4-column grid |
| Build ChartCard wrapper | 🔴 Critical | Card shell for all charts |
| Build FilterBar + FilterDropdown | 🔴 Critical | Reusable dropdown filters |
| Build DataTable | 🔴 Critical | Columns, rows, sorting, empty state |
| Build TableSearch | 🔴 Critical | Debounced search input |
| Build TablePagination | 🔴 Critical | Page navigation |
| Build StatusBadge | 🟡 Medium | Active / Attrited colored pill |
| Build RatingStars | 🟡 Medium | Visual 1–4 star rating |
| Build EmptyState | 🟡 Medium | Illustration + message |
| Build ErrorState | 🟡 Medium | Error message + retry button |
| Build SkeletonCard + SkeletonTable | 🟡 Medium | Loading placeholders |
| Build ExportButton | 🟡 Medium | CSV download trigger |
| Build Avatar | 🟢 Low | Initials fallback |
| Build Breadcrumbs | 🟢 Low | Navigation trail |
| Mobile hamburger menu + drawer | 🔴 Critical | Sidebar as mobile drawer |

**Definition of Done — Week 2:**
- Dashboard shell renders correctly for all roles
- All shared components render correctly in isolation
- Mobile layout works at 375px with hamburger menu

---

### Week 3 — CEO Dashboard

**Milestone:** Fully working CEO dashboard with all KPIs, charts, and employee table.

| Task | Priority | Notes |
|---|---|---|
| Build `/api/dashboard/kpis` endpoint | 🔴 Critical | With department filter support |
| Build `/api/departments` endpoint | 🔴 Critical | With summary stats |
| Build `/api/employees` endpoint | 🔴 Critical | Pagination, search, all filters |
| Build `/api/employees/[id]` endpoint | 🔴 Critical | Full employee profile |
| Build `/api/charts/attrition-by-department` | 🔴 Critical | |
| Build `/api/charts/attrition-by-age` | 🔴 Critical | |
| Build `/api/charts/gender-distribution` | 🔴 Critical | |
| Build `/api/charts/salary-by-department` | 🔴 Critical | |
| Build `/api/charts/performance-distribution` | 🔴 Critical | |
| Build `/api/charts/training-vs-performance` | 🟡 Medium | |
| Build `/api/charts/satisfaction-by-department` | 🟡 Medium | |
| Build CEO Overview page | 🔴 Critical | KPI cards + all charts + filter bar |
| Build AttritionByDepartmentChart | 🔴 Critical | Recharts BarChart horizontal |
| Build AttritionByAgeChart | 🔴 Critical | Recharts BarChart vertical |
| Build GenderDistributionChart | 🔴 Critical | Recharts PieChart |
| Build SalaryByDepartmentChart | 🔴 Critical | Recharts BarChart grouped |
| Build PerformanceDistributionChart | 🔴 Critical | Recharts BarChart |
| Build TrainingVsPerformanceChart | 🟡 Medium | Recharts LineChart |
| Build SatisfactionByDepartmentChart | 🟡 Medium | Recharts BarChart grouped |
| Build CEO Employees page | 🔴 Critical | Full table + search + filters + export |
| Build Employee detail view (modal) | 🔴 Critical | Full profile on row click |
| Build CEO Departments page | 🟡 Medium | Side-by-side department comparison |
| Connect filters to all charts and KPIs | 🔴 Critical | SWR re-fetches on filter change |
| Build `/api/export/employees` endpoint | 🔴 Critical | CSV generation with filters |

**Definition of Done — Week 3:**
- CEO can see all KPIs with correct values
- All charts render with real data from Supabase
- Filters update all charts and KPIs
- Employee table loads, searches, filters, and paginates
- Clicking an employee opens full profile
- Export downloads a real CSV file

---

### Week 4 — Manager & Employee Dashboards

**Milestone:** All three role dashboards fully working with correct data isolation.

| Task | Priority | Notes |
|---|---|---|
| Build Manager Overview page | 🔴 Critical | Department KPIs + charts |
| Build Manager employee list | 🔴 Critical | Own department only |
| Build Manager performance page | 🟡 Medium | Performance + training charts |
| Build `/api/charts/attrition-risk` | 🟡 Medium | Calculated risk score per employee |
| Build Employee Overview page | 🔴 Critical | Personal KPIs + satisfaction bars |
| Build Employee Profile page | 🔴 Critical | All personal data fields |
| Enforce department isolation in Manager API | 🔴 Critical | RLS + server-side check |
| Enforce employee self-only access | 🔴 Critical | RLS + server-side check |
| Build Unauthorized page | 🔴 Critical | Access denied screen |
| Build 404 Not Found page | 🔴 Critical | Custom Next.js not-found.tsx |
| Build About page | 🟢 Low | Project info + team + tech stack |

**Definition of Done — Week 4:**
- Manager can only see own department data
- Employee can only see own profile data
- Manually navigating to a forbidden route shows the Unauthorized page
- All three role dashboards are functional

---

### Week 5 — Polish, Settings & Testing

**Milestone:** All features complete, fully tested, ready for deployment.

| Task | Priority | Notes |
|---|---|---|
| Build Settings page | 🟡 Medium | Display name + theme + password reset |
| Build Profile page | 🟡 Medium | Account info |
| Build `/api/user/profile` GET + PATCH | 🟡 Medium | |
| Connect theme toggle to Tailwind dark mode | 🔴 Critical | `dark:` class prefix everywhere |
| Add loading skeletons to all data sections | 🔴 Critical | KPI cards + charts + tables |
| Add toast notifications for all API errors | 🔴 Critical | sonner toast.error() |
| Add empty states to all tables and charts | 🟡 Medium | |
| Write unit tests for utility functions | 🟡 Medium | Jest |
| Write unit tests for key components | 🟡 Medium | React Testing Library |
| Write integration tests for API routes | 🟡 Medium | |
| Run role-based access test scenarios | 🔴 Critical | Manual test from document 14 |
| Run authentication test scenarios | 🔴 Critical | Manual test from document 14 |
| Run Lighthouse performance audit | 🟡 Medium | Target ≥ 85 |
| Run axe accessibility audit | 🟡 Medium | |
| Cross-browser testing | 🟡 Medium | Chrome, Firefox, Safari, Edge |
| Mobile responsive testing | 🔴 Critical | 375px, 768px, 1024px |
| Fix all identified bugs | 🔴 Critical | |
| Code review + cleanup | 🟡 Medium | Remove console.logs, dead code |

---

### Week 6 — Deployment & Go Live

**Milestone:** Application is live on Vercel with all users created and verified.

| Task | Priority | Notes |
|---|---|---|
| Configure Vercel project | 🔴 Critical | Connect GitHub repo |
| Add all environment variables to Vercel | 🔴 Critical | Supabase keys + App URL |
| Configure Supabase auth redirect URLs | 🔴 Critical | Add production URL |
| Configure Supabase RLS policies | 🔴 Critical | All tables protected |
| Create production users in Supabase | 🔴 Critical | CEO, Manager, Employee, Tester accounts |
| First production deployment | 🔴 Critical | `git push origin main` |
| Verify all features on production URL | 🔴 Critical | Full UAT checklist from doc 14 |
| Share production URL with team | 🟡 Medium | |
| Document any known issues | 🟢 Low | Create GitHub issues for Version 1.1 |

---

## 3. Version 1.1 — Enhancements

**Estimated:** 2–3 weeks after Version 1.0

| Feature | Description |
|---|---|
| Advanced export options | Export charts as PNG, export specific date ranges |
| Email notifications | Alert managers when an employee's performance drops |
| Improved filters | Date range filter, multi-select department filter |
| Dashboard personalization | Users can rearrange which charts appear first |
| Print view | Printer-friendly dashboard layout |
| Data refresh indicator | Show when data was last updated from the ETL pipeline |
| Attrition prediction score | Visible risk indicator on employee cards |

---

## 4. Version 2.0 — Advanced Features

**Estimated:** 2–3 months after Version 1.1

| Feature | Description |
|---|---|
| Admin panel | Create/edit/delete users and roles without Supabase dashboard |
| AI-powered insights | Natural language queries ("Which department has highest attrition risk?") |
| Real-time data sync | Supabase Realtime subscriptions for live dashboard updates |
| Comparative period analysis | Compare Q1 vs Q2 performance |
| Custom report builder | CEO can build and save custom chart combinations |
| Mobile application | React Native or PWA version |
| Multi-tenant support | Multiple companies / organisations |
| Power BI embedded | Embed Power BI dashboards directly in the web app |

---

## 5. Build Order Summary (Quick Reference)

```
Week 1: Auth → Middleware → Layout providers
Week 2: Shared components → Dashboard shell → Mobile nav
Week 3: CEO APIs → CEO charts → CEO tables → Export
Week 4: Manager dashboard → Employee dashboard → Role isolation
Week 5: Settings → Polish → Testing → Bug fixes
Week 6: Supabase config → Vercel deploy → UAT → Go live
```

---

## 6. Git Branch Strategy

| Branch | Purpose |
|---|---|
| `main` | Production-ready code only — triggers Vercel deploy |
| `dev` | Integration branch — all features merge here first |
| `feature/auth` | Authentication implementation |
| `feature/ceo-dashboard` | CEO dashboard features |
| `feature/manager-dashboard` | Manager dashboard features |
| `feature/employee-dashboard` | Employee dashboard features |
| `feature/charts` | Chart components |
| `feature/export` | CSV export feature |
| `hotfix/*` | Emergency production fixes |

**Workflow:**
1. Create feature branch from `dev`
2. Build and test locally
3. Open pull request to `dev`
4. Review + merge to `dev`
5. Test on Vercel preview URL
6. Merge `dev` to `main` for production release

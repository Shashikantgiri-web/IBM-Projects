# 14 — Testing Strategy
## Employee Performance Intelligence System (EPIS)

---

## 1. Overview

EPIS requires multiple layers of testing to ensure correctness, security, and usability across all roles. Testing happens before deployment and after each significant change.

---

## 2. Testing Types

| Type | Tool | When |
|---|---|---|
| Unit Testing | Jest + React Testing Library | During development |
| Integration Testing | Jest + Supertest | During development |
| API Testing | Thunder Client / Postman | During API development |
| Authentication Testing | Manual + Cypress | Before deployment |
| Role Testing | Manual + Cypress | Before deployment |
| Security Testing | Manual | Before deployment |
| Responsive Testing | Chrome DevTools + BrowserStack | Before deployment |
| Performance Testing | Lighthouse | Before deployment |
| Accessibility Testing | axe DevTools | Before deployment |
| User Acceptance Testing | Manual | Final QA phase |

---

## 3. Unit Tests

Unit tests cover individual components and utility functions in isolation.

### 3.1 Component Tests

| Component | Test Cases |
|---|---|
| `KPICard` | Renders title, value, icon correctly; shows skeleton when loading; shows trend in correct color |
| `StatusBadge` | Renders "Active" in green; renders "Attrited" in red |
| `FilterDropdown` | Renders options; calls onChange on select; renders placeholder when no value |
| `TablePagination` | Shows correct page number; disables prev on page 1; disables next on last page |
| `PrimaryButton` | Renders children; shows spinner when isLoading=true; is disabled when disabled=true |
| `EmptyState` | Renders message; renders action button if provided |
| `RatingStars` | Renders correct number of filled stars for rating 1–4 |
| `ThemeToggle` | Calls toggleTheme when clicked |

### 3.2 Utility Function Tests

| Function | Test Cases |
|---|---|
| `formatCurrency(6503)` | Returns "$6,503" |
| `formatPercentage(16.12)` | Returns "16.1%" |
| `calculateAttritionRate(237, 1470)` | Returns 16.12 |
| `getAgeGroup(27)` | Returns "26-35" |
| `buildQueryString(filters)` | Returns correct URL params |
| `formatDate(date)` | Returns "2025-01-15" format |

---

## 4. Integration Tests

Integration tests verify that API routes return correct data and handle errors properly.

### 4.1 API Route Tests

| Endpoint | Test Cases |
|---|---|
| `GET /api/dashboard/kpis` | Returns correct KPI values; returns 401 without auth; filters work correctly with department_id param |
| `GET /api/employees` | Returns paginated list; pagination works; search works; filters work; returns 401 without auth |
| `GET /api/employees/[id]` | Returns correct employee; returns 404 for non-existent ID; employee cannot access another employee's record; manager cannot access other dept employee |
| `GET /api/departments` | Returns all departments; returns 401 without auth |
| `GET /api/charts/attrition-by-department` | Returns correct shape; values sum to total attrition count |
| `GET /api/export/employees` | Returns CSV content-type; file includes correct headers; respects active filters |
| `PATCH /api/user/profile` | Updates display name; rejects empty name; rejects unauthorized request |

---

## 5. Authentication Tests

| Test Case | Expected Result |
|---|---|
| Login with valid CEO credentials | Redirect to /dashboard/ceo |
| Login with valid Manager credentials | Redirect to /dashboard/manager |
| Login with valid Employee credentials | Redirect to /dashboard/employee |
| Login with invalid password | Show error "Invalid email or password" |
| Login with non-existent email | Show error "Invalid email or password" |
| Submit empty login form | Show field validation errors |
| Submit invalid email format | Show "Enter a valid email address" |
| Visit /login while logged in | Redirect to correct dashboard |
| Visit /dashboard/ceo while logged out | Redirect to /login |
| Forgot password with valid email | Show "Check your email" confirmation |
| Forgot password with unknown email | Show confirmation (security — no user enumeration) |
| Logout | Clear session, redirect to /login |
| Use browser back after logout | Cannot access protected pages |

---

## 6. Role-Based Access Tests

| Test Case | Expected Result |
|---|---|
| CEO visits /dashboard/ceo | ✅ Access granted |
| CEO visits /dashboard/manager | ✅ Access granted |
| CEO visits /dashboard/employee | ✅ Access granted |
| Manager visits /dashboard/ceo | ❌ Redirect to /unauthorized |
| Manager visits /dashboard/manager | ✅ Access granted |
| Manager visits /dashboard/employee | ✅ Access granted |
| Employee visits /dashboard/ceo | ❌ Redirect to /unauthorized |
| Employee visits /dashboard/manager | ❌ Redirect to /unauthorized |
| Employee visits /dashboard/employee | ✅ Access granted |
| Manager accesses API for other dept employee | ❌ Returns 403 |
| Employee accesses another employee via API | ❌ Returns 403 |
| Tester visits any dashboard | ✅ Access granted (read-only) |

---

## 7. Security Tests

| Test Case | Expected Result |
|---|---|
| API call without JWT | Returns 401 |
| API call with expired JWT | Returns 401 |
| API call with manipulated JWT | Returns 401 |
| Direct DB query bypassing RLS | RLS policy blocks unauthorized rows |
| XSS via search input | React escaping prevents script execution |
| Accessing another user's profile via direct URL | Returns 403 |
| SUPABASE_SERVICE_ROLE_KEY exposed to browser | Must not appear in client bundle |
| Environment variables in Git | .env.local must not be committed |
| Force-navigate to /dashboard/ceo as Employee | Middleware redirects to /unauthorized |
| Export another dept's data as Manager | API returns only own department |

---

## 8. Responsive Testing

Test on the following viewports:

| Device | Width | Expected |
|---|---|---|
| iPhone SE | 375px | Single column, hamburger nav, scrollable charts |
| iPhone 14 | 390px | Single column, hamburger nav |
| iPad Mini | 768px | Two columns, collapsible sidebar |
| iPad Pro | 1024px | Full layout, sidebar visible |
| MacBook 13" | 1280px | Full layout |
| Desktop 1440p | 1440px | Full layout, wider charts |

**Check on each viewport:**
- Navbar collapses on mobile
- Sidebar shows as drawer on mobile
- KPI cards stack vertically on mobile
- Charts resize without overflow
- Tables scroll horizontally on mobile
- Buttons are minimum 44px tap target on mobile
- Fonts remain readable at all sizes

---

## 9. Performance Tests (Lighthouse)

Run Lighthouse audit on the CEO dashboard page before deployment.

| Metric | Target |
|---|---|
| Performance Score | ≥ 85 |
| First Contentful Paint | ≤ 2.0s |
| Time to Interactive | ≤ 3.5s |
| Largest Contentful Paint | ≤ 3.0s |
| Cumulative Layout Shift | ≤ 0.1 |
| Accessibility Score | ≥ 90 |
| Best Practices Score | ≥ 90 |
| SEO Score | ≥ 80 |

---

## 10. Accessibility Tests

| Check | Tool | Expected |
|---|---|---|
| All images have alt text | axe DevTools | No violations |
| Color contrast ≥ 4.5:1 | axe DevTools | No violations |
| All form inputs have labels | axe DevTools | No violations |
| Keyboard navigation works | Manual | Tab order logical |
| Focus indicators visible | Manual | Blue ring visible |
| Modals trap focus | Manual | Tab stays within modal |
| Screen reader announces charts | Manual with VoiceOver | Chart data readable |

---

## 11. User Acceptance Testing (UAT)

UAT is done manually by a real person acting in each role.

### UAT Scenarios

**CEO User:**
1. Log in, land on CEO dashboard
2. View all 4 KPI cards — verify numbers make sense
3. Apply department filter "Sales" — verify all charts update
4. Navigate to Employees page — search for an employee
5. Click an employee row — view full profile
6. Export CSV — open file and verify data
7. Toggle dark mode — verify all components update
8. Log out

**Manager User:**
1. Log in, land on Manager dashboard
2. Verify only own department data is shown
3. Navigate to My Team — search for an employee
4. Click employee — verify profile loads
5. Try to manually navigate to /dashboard/ceo — verify access denied
6. Export department CSV

**Employee User:**
1. Log in, land on Employee dashboard
2. Verify only own data is shown (name, salary, performance)
3. Navigate to My Profile
4. Try to manually navigate to /dashboard/manager — verify access denied
5. Check Settings — toggle theme
6. Log out

---

## 12. Test Coverage Goals

| Area | Goal |
|---|---|
| Utility functions | 100% |
| API routes | 80%+ |
| React components | 70%+ |
| Authentication flows | 100% (manual) |
| Role access scenarios | 100% (manual) |

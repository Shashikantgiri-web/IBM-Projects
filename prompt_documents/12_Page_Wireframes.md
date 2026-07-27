# 12 — Page Wireframes
## Employee Performance Intelligence System (EPIS)

---

## 1. Login Page

```
┌───────────────────────────────────────────────────────────────┐
│                      [EPIS Logo]                              │
│              Employee Performance Intelligence System         │
│                                                               │
│   ┌───────────────────────────────────────────────────────┐   │
│   │                    Sign In                            │   │
│   │                                                       │   │
│   │   Email Address                                       │   │
│   │   ┌─────────────────────────────────────────────┐    │   │
│   │   │ alice@company.com                           │    │   │
│   │   └─────────────────────────────────────────────┘    │   │
│   │                                                       │   │
│   │   Password                                            │   │
│   │   ┌─────────────────────────────────────────────┐    │   │
│   │   │ ••••••••••••                           [👁] │    │   │
│   │   └─────────────────────────────────────────────┘    │   │
│   │                                                       │   │
│   │              [ Sign In → ]                            │   │
│   │                                                       │   │
│   │           Forgot your password?                       │   │
│   └───────────────────────────────────────────────────────┘   │
│                                                               │
│                    IBM HR Analytics Dataset                    │
└───────────────────────────────────────────────────────────────┘
```

**Notes:**
- Centered card, max-width 420px
- Dark background with subtle pattern or gradient
- Logo at top center
- Error message appears below the Sign In button in red

---

## 2. CEO Dashboard — Overview

```
┌──────────────────────────────────────────────────────────────────────┐
│ EPIS  Overview  Employees  Departments  Reports     🌙  [Avatar ▼]   │  ← Navbar
├────────────┬─────────────────────────────────────────────────────────┤
│            │  Dashboard Overview                                      │
│ Overview   │  ─────────────────────────────────────────              │
│ Employees  │                                                          │
│ Departments│  [ Dept ▼ ] [ Gender ▼ ] [ Job Role ▼ ] [✕ Clear]      │  ← Filters
│ Reports    │                                                          │
│            │  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐   │
│ ─────────  │  │  👥     │  │  📉     │  │  💰     │  │  ⭐     │   │
│ Settings   │  │  1,470  │  │  16.1%  │  │  $6,503 │  │   3.15  │   │  ← KPIs
│ Profile    │  │Total Emp│  │Attrition│  │Avg Salary│  │Avg Perf │   │
│            │  └─────────┘  └─────────┘  └─────────┘  └─────────┘   │
│            │                                                          │
│            │  ┌───────────────────────┐  ┌───────────────────────┐  │
│            │  │ Attrition by Dept     │  │ Employees by Dept     │  │
│            │  │   [Bar Chart]         │  │   [Bar Chart]         │  │  ← Charts row 1
│            │  └───────────────────────┘  └───────────────────────┘  │
│            │                                                          │
│            │  ┌───────────────────────┐  ┌───────────────────────┐  │
│            │  │ Salary Distribution   │  │ Gender Distribution   │  │
│            │  │   [Bar Chart]         │  │   [Pie Chart]         │  │  ← Charts row 2
│            │  └───────────────────────┘  └───────────────────────┘  │
│            │                                                          │
│            │  ┌───────────────────────┐  ┌───────────────────────┐  │
│            │  │ Performance Rating    │  │ Training vs Perf      │  │
│            │  │   [Bar Chart]         │  │   [Line Chart]        │  │  ← Charts row 3
│            │  └───────────────────────┘  └───────────────────────┘  │
└────────────┴─────────────────────────────────────────────────────────┘
```

---

## 3. CEO Dashboard — Employees Page

```
┌──────────────────────────────────────────────────────────────────────┐
│ EPIS  Overview  Employees  Departments  Reports     🌙  [Avatar ▼]   │
├────────────┬─────────────────────────────────────────────────────────┤
│            │  All Employees                          [⬇ Export CSV]  │
│ Overview   │  ─────────────────────────────────────────              │
│ Employees  │                                                          │
│ Departments│  [🔍 Search by name...] [ Dept ▼ ] [ Role ▼ ] [Attr ▼]│
│ Reports    │                                                          │
│            │  ┌────┬──────────────┬─────────┬──────────┬────────┐   │
│            │  │ID  │ Name         │ Dept    │ Perf ★   │ Status │   │
│            │  ├────┼──────────────┼─────────┼──────────┼────────┤   │
│            │  │001 │ Alice Smith  │ Sales   │ ●●●○     │ Active │   │
│            │  │002 │ Bob Johnson  │ R&D     │ ●●●●     │ Active │   │
│            │  │003 │ Carol White  │ HR      │ ●●○○     │ Left   │   │
│            │  │004 │ David Lee    │ Sales   │ ●●●○     │ Active │   │
│            │  │... │ ...          │ ...     │ ...      │ ...    │   │
│            │  └────┴──────────────┴─────────┴──────────┴────────┘   │
│            │                                                          │
│            │  Showing 1–25 of 1,470    [< Prev]  Page 1/59  [Next >]│
└────────────┴─────────────────────────────────────────────────────────┘
```

---

## 4. Employee Detail Page (Modal or Slide-Over)

```
┌──────────────────────────────────────────────────────────────┐
│  Employee Profile                                      [✕]   │
│  ──────────────────────────────────────────────────────      │
│                                                              │
│  [Avatar]  Alice Smith                                       │
│            Sales Executive · Sales Department                │
│            Employee #001                                     │
│                                                              │
│  ── Personal ─────────────────────────────────────────────   │
│  Age: 35          Gender: Female         Married             │
│  Distance from Home: 12 km                                   │
│                                                              │
│  ── Employment ────────────────────────────────────────────  │
│  Years at Company: 7       Years in Role: 3                  │
│  Years Since Promotion: 1  Years w/ Manager: 4               │
│  Over Time: Yes            Business Travel: Rarely           │
│                                                              │
│  ── Performance ───────────────────────────────────────────  │
│  Rating: ●●●○ (3/4 Excellent)   Job Involvement: ●●●○       │
│                                                              │
│  ── Salary ────────────────────────────────────────────────  │
│  Monthly Income: $5,993    Salary Hike: 14%                  │
│  Stock Option Level: 1                                       │
│                                                              │
│  ── Satisfaction ──────────────────────────────────────────  │
│  Job Satisfaction:    ●●●● (4/4 Very High)                   │
│  Environment:         ●●●○ (3/4 High)                        │
│  Relationships:       ●●●○ (3/4 High)                        │
│  Work-Life Balance:   ●●●○ (3/4 Good)                        │
│                                                              │
│  ── Training ──────────────────────────────────────────────  │
│  Training Sessions Last Year: 3                              │
│                                                              │
│  Status:  ● Active (Not Attrited)                            │
└──────────────────────────────────────────────────────────────┘
```

---

## 5. Manager Dashboard — Overview

```
┌──────────────────────────────────────────────────────────────────────┐
│ EPIS  Overview  My Team  Performance              🌙  [Avatar ▼]     │
├────────────┬─────────────────────────────────────────────────────────┤
│            │  Sales Department Overview                               │
│ Overview   │  ─────────────────────────────────────────              │
│ My Team    │                                                          │
│ Performance│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐   │
│            │  │  446    │  │  20.6%  │  │  $6,959 │  │  3.2    │   │
│ ─────────  │  │  Staff  │  │ Attrtn  │  │Avg Sal  │  │ Avg Prf │   │
│ Settings   │  └─────────┘  └─────────┘  └─────────┘  └─────────┘   │
│ Profile    │                                                          │
│            │  ┌───────────────────────┐  ┌───────────────────────┐  │
│            │  │ Performance Ratings   │  │ Job Satisfaction       │  │
│            │  │   [Bar Chart]         │  │   [Bar Chart]         │  │
│            │  └───────────────────────┘  └───────────────────────┘  │
│            │                                                          │
│            │  ┌───────────────────────┐  ┌───────────────────────┐  │
│            │  │ Training Hours        │  │ Attrition Risk        │  │
│            │  │   [Bar Chart]         │  │   [Bar Chart]         │  │
│            │  └───────────────────────┘  └───────────────────────┘  │
└────────────┴─────────────────────────────────────────────────────────┘
```

---

## 6. Employee Dashboard — My Profile

```
┌──────────────────────────────────────────────────────────────────────┐
│ EPIS  My Dashboard  My Profile                    🌙  [Avatar ▼]     │
├────────────┬─────────────────────────────────────────────────────────┤
│            │  My Performance Profile                                  │
│ My         │  ─────────────────────────────────────────              │
│ Dashboard  │                                                          │
│            │  [Avatar]  Alice Smith                                   │
│ My Profile │            Sales Executive · Sales Department            │
│            │                                                          │
│ ─────────  │  ┌─────────────────┐   ┌─────────────────┐             │
│ Settings   │  │ Performance     │   │ Salary          │             │
│            │  │   ●●●○ 3/4      │   │   $5,993/month  │             │
│            │  └─────────────────┘   └─────────────────┘             │
│            │                                                          │
│            │  ── My Journey ──────────────────────────────────────   │
│            │  Years at Company:    7                                  │
│            │  Years in Role:       3                                  │
│            │  Since Last Promotion: 1 year                            │
│            │                                                          │
│            │  ── My Satisfaction Scores ─────────────────────────    │
│            │  Job Satisfaction     ████████████░░░  4/4              │
│            │  Work-Life Balance    █████████░░░░░░  3/4              │
│            │  Environment          █████████░░░░░░  3/4              │
│            │  Relationships        █████████░░░░░░  3/4              │
│            │                                                          │
│            │  ── Training ──────────────────────────────────────     │
│            │  Training Sessions Last Year: 3                          │
└────────────┴─────────────────────────────────────────────────────────┘
```

---

## 7. Settings Page

```
┌──────────────────────────────────────────────────────────────────────┐
│ EPIS                                              🌙  [Avatar ▼]     │
├────────────┬─────────────────────────────────────────────────────────┤
│            │  Settings                                                │
│            │  ─────────────────────────────────────────              │
│            │                                                          │
│            │  ── Account ────────────────────────────────────────    │
│            │  Display Name                                            │
│            │  ┌───────────────────────────────────────┐  [Save]      │
│            │  │ Alice Smith                           │              │
│            │  └───────────────────────────────────────┘              │
│            │                                                          │
│            │  Email: alice@company.com  (read-only)                   │
│            │                                                          │
│            │  [Send Password Reset Email]                             │
│            │                                                          │
│            │  ── Appearance ─────────────────────────────────────    │
│            │  Theme                                                   │
│            │  ○ Light Mode     ● Dark Mode                            │
│            │                                                          │
│            │  ── Danger Zone ────────────────────────────────────    │
│            │  [Sign Out]                                              │
└────────────┴─────────────────────────────────────────────────────────┘
```

---

## 8. Error Pages

### 404 Not Found
```
┌──────────────────────────────────────────────────────────────┐
│                                                              │
│                        404                                   │
│                  Page Not Found                              │
│                                                              │
│    The page you are looking for doesn't exist or            │
│    has been moved.                                           │
│                                                              │
│              [← Back to Dashboard]                          │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

### Unauthorized
```
┌──────────────────────────────────────────────────────────────┐
│                                                              │
│                     🔒 403                                   │
│                  Access Denied                               │
│                                                              │
│    You don't have permission to view this page.             │
│                                                              │
│              [← Back to My Dashboard]                       │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

---

## 9. Mobile Layout (375px)

```
┌──────────────────────┐
│ ☰  EPIS        🌙 👤 │   ← Mobile Navbar
├──────────────────────┤
│  Dashboard           │
│                      │
│  ┌──────────────┐    │
│  │  1,470       │    │   ← Full-width KPI card
│  │  Total Emp   │    │
│  └──────────────┘    │
│                      │
│  ┌──────────────┐    │
│  │  16.1%       │    │
│  │  Attrition   │    │
│  └──────────────┘    │
│                      │
│  ┌──────────────┐    │
│  │ [Bar Chart]  │    │   ← Full-width chart
│  │ Attr by Dept │    │
│  └──────────────┘    │
│                      │
│  ┌──────────────┐    │
│  │ [Pie Chart]  │    │
│  │ Gender Dist  │    │
│  └──────────────┘    │
└──────────────────────┘
```

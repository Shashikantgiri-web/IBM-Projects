# 11 — Component Architecture
## Employee Performance Intelligence System (EPIS)

---

## 1. Component Hierarchy

```
RootLayout
├── AuthProvider
├── ThemeProvider
├── Toaster
│
├── (auth) Group
│   ├── LoginPage
│   │   ├── AuthCard
│   │   ├── EmailInput
│   │   ├── PasswordInput
│   │   ├── PrimaryButton
│   │   └── ForgotPasswordLink
│   │
│   └── ForgotPasswordPage
│       ├── AuthCard
│       ├── EmailInput
│       └── PrimaryButton
│
└── (dashboard) Group
    ├── DashboardLayout
    │   ├── Navbar
    │   │   ├── Logo
    │   │   ├── NavLinks
    │   │   ├── ThemeToggle
    │   │   └── UserMenu
    │   │       ├── Avatar
    │   │       ├── UserName
    │   │       ├── ProfileLink
    │   │       └── LogoutButton
    │   │
    │   ├── Sidebar
    │   │   ├── SidebarHeader
    │   │   ├── SidebarNav
    │   │   │   └── SidebarNavItem (×n)
    │   │   └── SidebarFooter
    │   │
    │   └── MainContent
    │       ├── PageHeader
    │       │   ├── PageTitle
    │       │   ├── PageSubtitle
    │       │   └── Breadcrumbs
    │       │
    │       ├── FilterBar
    │       │   ├── DepartmentFilter
    │       │   ├── GenderFilter
    │       │   ├── JobRoleFilter
    │       │   └── ClearFiltersButton
    │       │
    │       ├── KPIGrid
    │       │   └── KPICard (×4)
    │       │
    │       ├── ChartGrid
    │       │   ├── BarChartCard
    │       │   ├── LineChartCard
    │       │   ├── PieChartCard
    │       │   └── AreaChartCard
    │       │
    │       ├── EmployeeTable
    │       │   ├── TableSearch
    │       │   ├── TableFilters
    │       │   ├── DataTable
    │       │   │   ├── TableHeader
    │       │   │   ├── TableRow (×n)
    │       │   │   │   ├── StatusBadge
    │       │   │   │   └── RatingStars
    │       │   │   └── TableEmpty
    │       │   ├── TablePagination
    │       │   └── ExportButton
    │       │
    │       └── Footer
```

---

## 2. Component Catalogue

### 2.1 Layout Components

#### `DashboardLayout`
- **Purpose:** Wraps all dashboard pages with Navbar + Sidebar + Main area
- **Props:** `children: ReactNode`
- **State:** `sidebarOpen: boolean`

#### `Navbar`
- **Purpose:** Top bar with logo, nav links, theme toggle, user menu
- **Props:** `role: string`
- **Features:** Responsive hamburger on mobile

#### `Sidebar`
- **Purpose:** Left navigation for dashboard pages
- **Props:** `role: string`, `isOpen: boolean`, `onClose: () => void`
- **Features:** Collapsible, active link highlighting

#### `PageHeader`
- **Purpose:** Page title, subtitle, and breadcrumb
- **Props:** `title: string`, `subtitle?: string`, `breadcrumbs: Breadcrumb[]`

---

### 2.2 KPI Components

#### `KPICard`
- **Purpose:** Single metric display (number + label + trend)
- **Props:**
  ```typescript
  {
    title: string
    value: string | number
    icon: LucideIcon
    trend?: number       // positive = green, negative = red
    trendLabel?: string
    isLoading?: boolean
  }
  ```
- **Features:** Skeleton loading state, trend indicator

#### `KPIGrid`
- **Purpose:** Responsive 4-column grid of KPI cards
- **Props:** `children: ReactNode`

---

### 2.3 Chart Components

All chart components follow the same wrapper pattern:

#### `ChartCard` (wrapper)
- **Purpose:** Provides the card shell for any chart
- **Props:** `title: string`, `subtitle?: string`, `children: ReactNode`, `isLoading?: boolean`

#### `AttritionByDepartmentChart`
- **Type:** Horizontal Bar Chart (Recharts)
- **Data hook:** `useAttritionByDepartment(filters)`

#### `AttritionByAgeChart`
- **Type:** Bar Chart
- **Data hook:** `useAttritionByAge(filters)`

#### `SalaryByDepartmentChart`
- **Type:** Bar Chart
- **Data hook:** `useSalaryByDepartment(filters)`

#### `PerformanceDistributionChart`
- **Type:** Bar Chart
- **Data hook:** `usePerformanceDistribution(filters)`

#### `GenderDistributionChart`
- **Type:** Pie Chart
- **Data hook:** `useGenderDistribution(filters)`

#### `TrainingVsPerformanceChart`
- **Type:** Line Chart
- **Data hook:** `useTrainingVsPerformance(filters)`

#### `SatisfactionByDepartmentChart`
- **Type:** Grouped Bar Chart
- **Data hook:** `useSatisfactionByDepartment(filters)`

---

### 2.4 Filter Components

#### `FilterBar`
- **Purpose:** Container for all filter dropdowns
- **Props:** `filters: Filters`, `onChange: (key, value) => void`, `onClear: () => void`

#### `FilterDropdown`
- **Purpose:** Single reusable dropdown filter
- **Props:**
  ```typescript
  {
    label: string
    options: { value: string; label: string }[]
    value: string | null
    onChange: (value: string | null) => void
    placeholder?: string
  }
  ```

---

### 2.5 Table Components

#### `EmployeeTable`
- **Purpose:** Full employee list with search, filters, pagination, export
- **Props:** `role: string`, `departmentId?: string`
- **State:** `search`, `page`, `filters`

#### `DataTable`
- **Purpose:** Generic table component
- **Props:**
  ```typescript
  {
    columns: Column[]
    data: Record<string, any>[]
    onRowClick?: (row: any) => void
    isLoading?: boolean
    emptyMessage?: string
  }
  ```

#### `TableSearch`
- **Purpose:** Debounced search input
- **Props:** `value: string`, `onChange: (v: string) => void`, `placeholder?: string`

#### `TablePagination`
- **Purpose:** Page navigation
- **Props:** `page: number`, `totalPages: number`, `onPageChange: (p: number) => void`

#### `StatusBadge`
- **Purpose:** Colored pill for attrition/active status
- **Props:** `status: 'active' | 'attrited'`

#### `RatingStars`
- **Purpose:** Visual 1–4 star rating
- **Props:** `rating: number`, `max?: number`

---

### 2.6 Form Components

#### `EmailInput`
- **Purpose:** Email field with validation
- **Props:** Standard HTML input props + `error?: string`

#### `PasswordInput`
- **Purpose:** Password field with show/hide toggle
- **Props:** Standard HTML input props + `error?: string`

#### `PrimaryButton`
- **Purpose:** Main CTA button with loading state
- **Props:** `children`, `isLoading?: boolean`, `disabled?: boolean`, `onClick?: () => void`

---

### 2.7 Feedback Components

#### `SkeletonCard`
- **Purpose:** Animated placeholder for KPI cards while loading
- **Props:** `count?: number`

#### `SkeletonTable`
- **Purpose:** Animated placeholder for tables while loading
- **Props:** `rows?: number`

#### `EmptyState`
- **Purpose:** Illustration + message for zero results
- **Props:** `message: string`, `action?: ReactNode`

#### `ErrorState`
- **Purpose:** Error display with retry button
- **Props:** `message: string`, `onRetry?: () => void`

#### `Toast` (via Sonner)
- **Purpose:** System notifications
- **Usage:** `toast.success()`, `toast.error()`, `toast.loading()`

#### `LoadingSpinner`
- **Purpose:** Circular loading indicator
- **Props:** `size?: 'sm' | 'md' | 'lg'`

---

### 2.8 Export Component

#### `ExportButton`
- **Purpose:** Triggers CSV download of current filtered data
- **Props:** `filters: Filters`, `role: string`
- **State:** `isExporting: boolean`

---

### 2.9 Utility Components

#### `ThemeToggle`
- **Purpose:** Dark/light mode switch button
- **Uses:** `useTheme()` context

#### `Avatar`
- **Purpose:** User avatar with fallback initials
- **Props:** `name: string`, `size?: 'sm' | 'md' | 'lg'`

#### `Breadcrumbs`
- **Purpose:** Navigation breadcrumb trail
- **Props:** `items: { label: string; href?: string }[]`

---

## 3. File Organization

```
components/
├── layout/
│   ├── DashboardLayout.tsx
│   ├── Navbar.tsx
│   ├── Sidebar.tsx
│   ├── PageHeader.tsx
│   └── Footer.tsx
│
├── kpi/
│   ├── KPICard.tsx
│   └── KPIGrid.tsx
│
├── charts/
│   ├── ChartCard.tsx
│   ├── AttritionByDepartmentChart.tsx
│   ├── AttritionByAgeChart.tsx
│   ├── SalaryByDepartmentChart.tsx
│   ├── PerformanceDistributionChart.tsx
│   ├── GenderDistributionChart.tsx
│   ├── TrainingVsPerformanceChart.tsx
│   └── SatisfactionByDepartmentChart.tsx
│
├── filters/
│   ├── FilterBar.tsx
│   └── FilterDropdown.tsx
│
├── table/
│   ├── EmployeeTable.tsx
│   ├── DataTable.tsx
│   ├── TableSearch.tsx
│   ├── TablePagination.tsx
│   ├── StatusBadge.tsx
│   └── RatingStars.tsx
│
├── forms/
│   ├── EmailInput.tsx
│   ├── PasswordInput.tsx
│   └── PrimaryButton.tsx
│
├── feedback/
│   ├── SkeletonCard.tsx
│   ├── SkeletonTable.tsx
│   ├── EmptyState.tsx
│   ├── ErrorState.tsx
│   └── LoadingSpinner.tsx
│
└── shared/
    ├── ThemeToggle.tsx
    ├── Avatar.tsx
    ├── Breadcrumbs.tsx
    └── ExportButton.tsx
```

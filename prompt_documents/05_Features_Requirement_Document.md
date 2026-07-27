# 05 — Features Requirement Document (FRD)
## Employee Performance Intelligence System (EPIS)

---

## Feature 1: Authentication

### 1.1 Login
**Description:** Users log in with email and password via Supabase Auth.

**User Story:** As a user, I want to log in with my email and password so that I can access my role-specific dashboard.

**Acceptance Criteria:**
- Login form has email and password fields
- Validation runs before submission (empty fields, invalid email format)
- On success, user is redirected to their role dashboard
- On failure, error message displays ("Invalid email or password")
- Loading spinner shows during authentication
- "Forgot Password" link is visible on the login page

**UI Elements:** Email input, Password input (with show/hide toggle), Login button, Forgot Password link, Error message area

---

### 1.2 Logout
**Description:** Users can log out from any page via the navbar user menu.

**Acceptance Criteria:**
- Logout option is in the user dropdown in the navbar
- On logout, Supabase session is cleared
- User is redirected to /login
- Back button after logout does not allow re-entry to protected pages

---

### 1.3 Forgot Password
**Description:** Users can request a password reset email.

**Acceptance Criteria:**
- User enters email on /forgot-password
- Supabase sends a reset email
- Confirmation message shows ("Check your email for a reset link")
- Reset link expires after 1 hour
- Invalid email shows appropriate error

---

## Feature 2: CEO Dashboard

### 2.1 Overview Page
**Description:** The main CEO page with KPI cards and summary charts.

**KPI Cards (top row):**
- Total Employees
- Attrition Rate (%)
- Average Monthly Salary
- Average Performance Rating

**Charts Section:**
- Department-wise employee distribution (bar chart)
- Attrition by department (bar chart)
- Attrition by age group (bar chart)
- Gender distribution (pie chart)
- Salary distribution by department (bar chart)
- Performance rating distribution (bar chart)
- Training hours vs performance (scatter or line chart)
- Job satisfaction by department (bar chart)

**Filters:**
- Department dropdown
- Gender dropdown
- Job Role dropdown
- Education Level dropdown

**Acceptance Criteria:**
- All KPIs display correct values from Supabase
- Charts render without errors
- Filters dynamically update charts and KPIs
- Export button downloads filtered data as CSV

---

### 2.2 Employee List Page (CEO View)
**Description:** Full employee table with search and filtering.

**Columns:** Employee ID, Name, Department, Job Role, Gender, Age, Years at Company, Performance Rating, Attrition

**Features:**
- Search by name
- Filter by department, job role, gender, attrition
- Pagination (25 rows per page)
- Click row to view employee detail

---

### 2.3 Department Overview Page
**Description:** Side-by-side department comparison.

**Displays:** Employee count per department, attrition per department, average salary per department, average performance per department

---

## Feature 3: Manager Dashboard

### 3.1 Department Overview
**Description:** Manager's home page showing their department summary.

**KPI Cards:**
- Total Employees in Department
- Department Attrition Rate
- Average Salary (Department)
- Average Performance Rating (Department)
- Training Completion Rate

**Charts:**
- Employee performance distribution (bar chart)
- Job satisfaction scores (bar chart)
- Attrition risk indicators (bar chart)
- Training hours per employee (bar chart)

---

### 3.2 Department Employee List
**Description:** List of employees in the manager's department only.

**Columns:** Name, Job Role, Age, Performance Rating, Job Satisfaction, Years at Company, Attrition

**Features:**
- Search by name
- Filter by job role, gender
- Click to view individual employee profile

---

### 3.3 Individual Employee Profile (Manager View)
**Description:** Full view of a single employee's data — accessible to managers for their department employees.

**Displays:**
- Personal info (name, age, gender, education)
- Employment info (department, job role, years at company)
- Performance rating
- Salary
- Training details
- Satisfaction scores (job, environment, relationships)
- Attrition status

---

## Feature 4: Employee Dashboard

### 4.1 My Profile Page
**Description:** Employee's personal view of their own data.

**Displays:**
- Name, Department, Job Role, Education
- Performance Rating
- Monthly Income
- Years at Company / Years in Current Role / Years Since Last Promotion
- Training Times Last Year
- Job Satisfaction Score
- Work-Life Balance Score
- Environment Satisfaction Score
- Relationship Satisfaction Score
- Overtime status

**Acceptance Criteria:**
- Only shows data for the logged-in employee
- Cannot navigate to other employees' data
- Display is read-only (no editing)

---

## Feature 5: Global Filters

### 5.1 Department Filter
**Description:** Dropdown to filter dashboard data by department.

**Options:** All Departments, Human Resources, Research & Development, Sales

**Behavior:** Selecting a department re-fetches data and updates all KPI cards and charts on the page.

---

### 5.2 Gender Filter
**Description:** Dropdown to filter by gender.

**Options:** All, Male, Female

---

### 5.3 Job Role Filter
**Description:** Dropdown to filter by job role.

**Options:** All job roles derived from database (dynamic list)

---

### 5.4 Education Level Filter
**Description:** Dropdown to filter by education level.

**Options:** Below College, College, Bachelor, Master, Doctor

---

## Feature 6: Search

### 6.1 Employee Search
**Description:** Text input that searches employees by name in real time.

**Behavior:**
- Debounced (waits 300ms after user stops typing before querying)
- Case-insensitive
- Clears with an X button
- Shows "No results found" if no match

---

## Feature 7: Data Export

### 7.1 CSV Export
**Description:** Download the currently filtered dataset as a CSV file.

**Available to:** CEO, Manager

**Behavior:**
- Exports the data currently displayed in the table (respects active filters)
- Filename format: `epis_export_YYYY-MM-DD.csv`
- Columns match the visible table columns
- Triggers a browser download

---

## Feature 8: Dark Mode

### 8.1 Theme Toggle
**Description:** Switch between dark and light mode.

**Location:** Navbar (top right)

**Behavior:**
- Toggles instantly without page reload
- Saves preference in localStorage
- Persists across sessions
- All components (charts, cards, tables, modals) respect the theme

---

## Feature 9: Settings Page

### 9.1 Account Settings
**Description:** User-specific settings.

**Available Options:**
- Display name (editable)
- Theme preference (dark/light toggle)
- Change password (triggers Supabase password reset email)

---

## Feature 10: About Page

### 10.1 Project Information
**Description:** Static page describing the project.

**Displays:**
- Project name and description
- Technologies used
- Dataset credit (IBM HR Analytics)
- Team members
- Version information

---

## Feature 11: Error States

### 11.1 404 Page
**Description:** Shown when a route does not exist.

**Displays:** Friendly error message with a link back to the dashboard.

### 11.2 Unauthorized Page
**Description:** Shown when a user accesses a route outside their role.

**Displays:** Access denied message with a link back to their correct dashboard.

### 11.3 Empty State
**Description:** Shown when data exists but no results match the filters.

**Displays:** Illustration and message: "No employees match your current filters."

### 11.4 Loading State
**Description:** Skeleton loaders appear while data is being fetched.

**Behavior:** KPI cards and chart areas show skeleton animations during API calls.

### 11.5 API Error Toast
**Description:** Toast notification when an API call fails.

**Displays:** "Failed to load data. Please refresh." with a dismiss button.

---

## Feature 12: Navigation

### 12.1 Navbar
**Description:** Top navigation bar present on all authenticated pages.

**Contains:** Logo, role-specific nav links, theme toggle, user avatar/name, logout menu

### 12.2 Sidebar
**Description:** Left sidebar present on all dashboard pages.

**Contains:** Section links specific to the user's role (Overview, Employees, Departments, Reports for CEO; Overview, My Team, Performance for Manager; My Profile for Employee)

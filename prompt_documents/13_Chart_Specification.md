# 13 — Chart Specification
## Employee Performance Intelligence System (EPIS)

---

## Overview

Every chart in EPIS is defined here. For each chart:
- The business question it answers
- The data source (API endpoint + columns)
- Chart type and library
- Axis labels
- Tooltip content
- Color mapping
- Empty state behavior
- Loading state behavior
- Which roles can see it

---

## Chart 1: Attrition by Department

| Property | Value |
|---|---|
| Title | Attrition Rate by Department |
| Business Question | Which department has the highest attrition? |
| API Endpoint | GET /api/charts/attrition-by-department |
| Chart Type | Horizontal Bar Chart |
| Library | Recharts — `BarChart` (layout="vertical") |
| X Axis | Attrition Rate (%) |
| Y Axis | Department Name |
| Bar Color | `#DA1E28` (Red) for attrition |
| Tooltip | "Sales: 20.6% (92 employees)" |
| Empty State | "No attrition data available" |
| Loading | Skeleton bar |
| Visible To | CEO, Manager (own dept only), Tester |
| Filters | Department (CEO only), Gender |

**Data shape:**
```json
[
  { "department": "Sales", "attrition_rate": 20.63, "count": 92 },
  { "department": "Human Resources", "attrition_rate": 19.05, "count": 12 },
  { "department": "Research & Development", "attrition_rate": 13.84, "count": 133 }
]
```

---

## Chart 2: Attrition by Age Group

| Property | Value |
|---|---|
| Title | Attrition by Age Group |
| Business Question | Which age group has the highest risk of leaving? |
| API Endpoint | GET /api/charts/attrition-by-age |
| Chart Type | Vertical Bar Chart |
| Library | Recharts — `BarChart` |
| X Axis | Age Group (18-25, 26-35, etc.) |
| Y Axis | Attrition Count |
| Bar Color | `#FF832B` (Orange) |
| Tooltip | "26-35: 112 employees left (20.2%)" |
| Empty State | "No age data available" |
| Loading | Skeleton bar |
| Visible To | CEO, Tester |
| Filters | Department, Gender |

**Data shape:**
```json
[
  { "age_group": "18-25", "attrition_count": 38, "total": 85, "rate": 44.7 },
  { "age_group": "26-35", "attrition_count": 112, "total": 554, "rate": 20.2 },
  { "age_group": "36-45", "attrition_count": 51, "total": 513, "rate": 9.9 },
  { "age_group": "46-55", "attrition_count": 25, "total": 253, "rate": 9.9 },
  { "age_group": "56+", "attrition_count": 11, "total": 65, "rate": 16.9 }
]
```

---

## Chart 3: Employee Count by Department

| Property | Value |
|---|---|
| Title | Employees by Department |
| Business Question | How is the workforce distributed across departments? |
| API Endpoint | GET /api/departments |
| Chart Type | Vertical Bar Chart |
| Library | Recharts — `BarChart` |
| X Axis | Department |
| Y Axis | Employee Count |
| Bar Color | `#0F62FE` (Primary Blue) |
| Tooltip | "Research & Development: 961 employees" |
| Empty State | "No department data available" |
| Visible To | CEO, Tester |
| Filters | None |

**Data shape:**
```json
[
  { "department": "Human Resources", "employee_count": 63 },
  { "department": "Research & Development", "employee_count": 961 },
  { "department": "Sales", "employee_count": 446 }
]
```

---

## Chart 4: Gender Distribution

| Property | Value |
|---|---|
| Title | Gender Distribution |
| Business Question | What is the male/female ratio in the organization? |
| API Endpoint | GET /api/charts/gender-distribution |
| Chart Type | Donut Pie Chart |
| Library | Recharts — `PieChart` |
| Segment Colors | Male: `#0F62FE`, Female: `#EE538B` |
| Center Label | Total count |
| Tooltip | "Male: 882 (60%)" |
| Legend | Below chart |
| Empty State | "No gender data available" |
| Visible To | CEO, Manager, Tester |
| Filters | Department |

**Data shape:**
```json
[
  { "gender": "Male", "count": 882, "percentage": 60 },
  { "gender": "Female", "count": 588, "percentage": 40 }
]
```

---

## Chart 5: Salary Distribution by Department

| Property | Value |
|---|---|
| Title | Average Monthly Salary by Department |
| Business Question | How does salary compare across departments? |
| API Endpoint | GET /api/charts/salary-by-department |
| Chart Type | Grouped Bar Chart |
| Library | Recharts — `BarChart` |
| X Axis | Department |
| Y Axis | Monthly Salary (USD) |
| Bars | Avg, Min, Max salary per department |
| Colors | Avg: `#0F62FE`, Min: `#42BE65`, Max: `#DA1E28` |
| Tooltip | "Sales — Avg: $6,959 | Min: $1,052 | Max: $19,999" |
| Y Axis Format | `$6,959` (with dollar sign) |
| Visible To | CEO, Tester |
| Filters | None (salary is CEO-only sensitive data) |

---

## Chart 6: Performance Rating Distribution

| Property | Value |
|---|---|
| Title | Performance Rating Distribution |
| Business Question | How are employees distributed across performance levels? |
| API Endpoint | GET /api/charts/performance-distribution |
| Chart Type | Vertical Bar Chart |
| Library | Recharts — `BarChart` |
| X Axis | Rating Level (1=Low, 2=Good, 3=Excellent, 4=Outstanding) |
| Y Axis | Employee Count |
| Bar Colors | 1: `#DA1E28`, 2: `#F1C21B`, 3: `#42BE65`, 4: `#0F62FE` |
| Tooltip | "Excellent (3): 828 employees" |
| Empty State | "No performance data available" |
| Visible To | CEO, Manager, Tester |
| Filters | Department, Gender |

**Data shape:**
```json
[
  { "rating": 1, "label": "Low",         "count": 56  },
  { "rating": 2, "label": "Good",        "count": 230 },
  { "rating": 3, "label": "Excellent",   "count": 828 },
  { "rating": 4, "label": "Outstanding", "count": 356 }
]
```

---

## Chart 7: Training Times vs Performance Rating

| Property | Value |
|---|---|
| Title | Training Frequency vs Performance |
| Business Question | Does more training lead to better performance? |
| API Endpoint | GET /api/charts/training-vs-performance |
| Chart Type | Line Chart |
| Library | Recharts — `LineChart` |
| X Axis | Number of Training Sessions (0–6) |
| Y Axis | Average Performance Rating |
| Line Color | `#08BDBA` (Teal) |
| Dot | Visible on each data point |
| Tooltip | "3 sessions → avg. 3.2 rating" |
| Y Axis Domain | [1, 4] |
| Reference Line | Average performance (dashed) |
| Visible To | CEO, Manager, Tester |
| Filters | Department |

---

## Chart 8: Job Satisfaction by Department

| Property | Value |
|---|---|
| Title | Job Satisfaction by Department |
| Business Question | Which department has the best employee satisfaction? |
| API Endpoint | GET /api/charts/satisfaction-by-department |
| Chart Type | Grouped Bar Chart |
| Library | Recharts — `BarChart` |
| X Axis | Department |
| Y Axis | Average Score (1–4) |
| Groups | Job Satisfaction, Environment Satisfaction, Work-Life Balance |
| Colors | `#0F62FE`, `#42BE65`, `#BE95FF` |
| Y Axis Domain | [0, 4] |
| Tooltip | "Sales — Job: 2.7 | Env: 2.8 | WLB: 2.7" |
| Visible To | CEO, Manager, Tester |
| Filters | Department |

---

## Chart 9: Attrition Risk Indicators (Manager View Only)

| Property | Value |
|---|---|
| Title | Attrition Risk Factors |
| Business Question | Which employees in my department are most at risk? |
| API Endpoint | GET /api/charts/attrition-risk |
| Chart Type | Horizontal Bar Chart |
| Library | Recharts — `BarChart` |
| Y Axis | Employee Name |
| X Axis | Risk Score (0–100) |
| Color | Green (low) → Yellow (medium) → Red (high) |
| Tooltip | "Alice Smith: 72% risk — Overtime + Low Satisfaction" |
| Visible To | Manager, CEO, Tester |
| Filters | None |

**Note:** Risk score is calculated server-side from: overtime status, job satisfaction, years since last promotion, distance from home, and work-life balance score.

---

## Chart 10: Training Hours per Employee (Manager View)

| Property | Value |
|---|---|
| Title | Training Sessions per Employee |
| Business Question | Which employees received the most/least training? |
| API Endpoint | GET /api/employees with training join |
| Chart Type | Horizontal Bar Chart |
| Library | Recharts — `BarChart` |
| Y Axis | Employee Name |
| X Axis | Training Sessions Last Year |
| Bar Color | `#42BE65` (Green) |
| Reference Line | Department average (dashed) |
| Tooltip | "Alice Smith: 3 sessions" |
| Visible To | Manager, CEO, Tester |
| Filters | None |

---

## General Chart Rules

1. **All charts use the ChartCard wrapper** — consistent title, subtitle, and border
2. **Loading state:** Skeleton animation in the chart area (200px height placeholder)
3. **Empty state:** Icon + message centered in the chart area
4. **Tooltips:** Custom styled tooltip using Recharts `<Tooltip content={<CustomTooltip />}>`
5. **Responsive:** All charts use `<ResponsiveContainer width="100%" height={280}>`
6. **Axes:** X and Y axis labels use 12px IBM Plex Sans, gray color
7. **Animation:** Recharts default animation on mount (400ms ease)
8. **Dark mode:** Chart background transparent; text and grid lines adapt to dark/light via CSS variables
9. **Grid lines:** Light horizontal grid lines only (`strokeDasharray="3 3"`, opacity 0.3)
10. **No legends inside chart area** — legends go below the chart

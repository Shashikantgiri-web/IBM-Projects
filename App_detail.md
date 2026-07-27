This is a web analysis IBM app of IBM company.
<img width="168" height="148" alt="image" src="https://github.com/user-attachments/assets/b0328fcc-21a9-44d7-8e96-146eb8285f58" />


The idea is inspired by a college website where students login and the website tries to show student details after proper analysis, but it doesn’t show it because the college doesn’t have an analysis team that performs analysis of every single student on a monthly basis.

I decided to make the IBM app for a IBM company because making it for the college doesn’t make sense.

Companies use their own development IBM app to perform tasks, and every company wants to track the performance of its employees. The company also has an analysis team which uses company employee data to create analysis dashboards that use that analysis data. Then the dashboard developers make an IBM app that shows:
- performance of every employee,
- performance of every department, and
- the overall company performance.

This is the basic idea and inspiration for the IBM app. Now the real work starts. This is not a simple comment IBM app that is made only by writing code; it needs analysis.

Break down these two parts: the developer and the analysis.

The analysis part will be handled by the analysis team. The team uses Excel to collect row data, clean it, remove duplication, and understand the data.

The team imports those Excel files into Power BI to create new columns and measures, using DAX queries, to make three dashboards.

In Power BI, the team makes three dashboards.

First is for CEO or admin dashboards. In these dashboards, the CEO can see the whole IBM company’s detailed performance analysis, including which department category is performing good or bad, along with employee performance analysis.

Second is for manage or department-wise dashboards. In this, the team plans to use user slicers to select one single department to see only that department’s data, which makes the dashboards.

Third (last three dashboards) is for employee or single-employee dashboards. In this, the team uses the user slicer again to select one employee to see only that employee’s data, which makes the dashboards.

After making all three dashboards, the team copies all table data and pastes it into Excel to store all new changes.

The team creates an online PostgreSQL server on Supabase, which becomes the database of the IBM app.

The team creates a new database name (access). In the IBM app, access control is saved in that table. The team stores the CEO, managers, and test people’s user IDs, email, and passwords.

The team plans to use Python to convert all Excel table data into PostgreSQL tables on the online server:

```
Excel File
      │
      ▼
Read Excel
(pandas)

      │
      ▼
Validate Columns

      │
      ▼
Remove Duplicates

      │
      ▼
Handle Missing Values

      │
      ▼
Standardize Values
(Male/Female
Yes/No)

      │
      ▼
Generate Department IDs

      │
      ▼
Generate Job Role IDs

      │
      ▼
Split Data into
Multiple DataFrames

employees

salary

performance

training

experience

satisfaction

      │
      ▼
Bulk Insert
(psycopg2)

      │
      ▼
Commit

      │
      ▼
Write Import Log

      │
      ▼
Refresh Dashboard Summary Tables
Recommended Project Structure
etl/

│
├── config.py

├── db.py

├── validator.py

├── cleaner.py

├── transformer.py

├── mapper.py

├── loader.py

├── logger.py

├── main.py

│
├── excel/
│      employees.xlsx
│
├── logs/
│
└── archive/
Each module has a single responsibility:
validator.py checks required columns and data types.
cleaner.py removes duplicates and fixes missing values.
transformer.py derives new fields (e.g., Experience Level, Promotion Status).
mapper.py converts names to foreign keys (department, role, education field).
loader.py performs transactional inserts into PostgreSQL.
logger.py records every import into import_logs.
Recommendation
Your document currently assumes copying Power BI output back into Excel and then uploading it to PostgreSQL. From a database engineering perspective, I recommend making PostgreSQL the single source of truth:
Import and clean raw Excel data with Python.
Store normalized data in PostgreSQL.
Let Power BI connect directly to PostgreSQL for dashboards.
Let the Next.js application also read from PostgreSQL.
This removes duplicate data movement, reduces errors, and ensures both the dashboards and the web app always use the same, up-to-date data.
```

Now the analysis team provides the online server and the three dashboards to the developer to make the IBM app.

Your job, like a senior developer (not AI), is to make a web IBM app using this analysis that is SEO friendly, properly tested for every feature, responsive, and secured with security algorithms.

The term of IBM app prompt:

```
Below is a **professional AI prompt** you can give to Claude, GPT, Gemini, Lovable, v0, Bolt, Replit AI, or any UI generation model to recreate the **design philosophy** of IBM's website—not to copy it exactly, but to capture its visual language, UX principles, typography, spacing, color system, and component behavior.

---

# IBM Website UI/UX Design Analysis Prompt

## Role

Act as a **Senior UI/UX Designer**, **Creative Director**, and **Frontend Design System Architect** with over 20 years of experience designing enterprise websites for Fortune 500 companies.

Analyze the IBM homepage and recreate its **design language**, **visual hierarchy**, and **professional enterprise aesthetic**, without copying the original website.

Design a modern enterprise website inspired by IBM's design principles.

---

# Overall Design Philosophy

The design should feel:

* Enterprise-grade
* Professional
* Premium
* Clean
* Minimal
* Highly structured
* Technology-focused
* Future-ready
* Corporate but approachable
* Elegant rather than flashy
* Large-scale global company

The interface should communicate trust, intelligence, innovation, and stability.

Avoid startup-style colorful gradients or excessive animations.

---

# Layout Structure

Use a strict grid layout.

Characteristics:

* Large white spaces
* Plenty of breathing room
* Modular card-based sections
* Horizontal alignment
* Consistent spacing system
* Clear visual hierarchy
* Symmetrical content blocks

Page Width

* Max Width:
  1440–1600px

Content Width

* 1200–1320px

Padding

Desktop:

80px–120px

Tablet:

48px

Mobile:

20px

Spacing System

8px scale

Example:

8px

16px

24px

32px

48px

64px

96px

128px

Every section should have generous vertical spacing.

---

# Color Palette

Primary Colors

IBM Blue

```
#0F62FE
```

Dark Text

```
#161616
```

Secondary Text

```
#525252
```

Light Gray Background

```
#F4F4F4
```

Border Gray

```
#E0E0E0
```

Card Background

```
#FFFFFF
```

Footer Background

```
#161616
```

Footer Text

```
#FFFFFF
```

Accent Colors

Very limited.

Only use:

Blue

Black

White

Light Gray

Never use rainbow gradients.

---

# Typography

Use IBM Plex Sans.

Fallback:

Inter

Helvetica

Arial

Hierarchy

Hero Title

48–64px

Bold

Section Title

36px

Medium

Card Title

20px

Medium

Paragraph

16px

Regular

Small Text

14px

Navigation

15px

Weight

Regular

Medium

SemiBold

Bold

Avoid extra-bold fonts.

Letter spacing should be slightly open.

Line height around 150%.

---

# Navigation Bar

Sticky navigation.

Height:

72px

Background:

White

Minimal shadow on scroll.

Left

Logo

Center

Dropdown menus

Products

Solutions

Industries

Resources

Support

Company

Right

Search

Language

Account

CTA button

Hover Effects

Simple underline

Text color transition

No dramatic animation.

---

# Hero Section

Large whitespace.

Left Side

Large headline

Supporting description

Primary CTA

Secondary CTA

Right Side

Large clean illustration

Technology visual

AI graphic

Enterprise dashboard

Abstract 3D shapes

Avoid clutter.

Buttons

Primary

Solid blue

White text

Secondary

White

Blue border

Blue text

Radius

4px

---

# Cards

Cards should be minimal.

Structure

Image

Category

Title

Description

CTA

Border

1px solid light gray

Radius

4px

Hover

Small elevation

Border turns blue

Arrow icon animates slightly

Shadow should remain subtle.

---

# Buttons

Primary

Blue background

White text

Small radius

Secondary

Outlined

Blue border

Blue text

Text Button

Only arrow icon

Simple hover

---

# Icons

Thin line icons

Simple

Monochrome

IBM Carbon style

Avoid colorful icons.

---

# Images

Use

Technology

Cloud

AI

Business

Enterprise teams

Data visualization

Servers

Cybersecurity

Digital transformation

Photography style

Professional

Minimal

Soft lighting

Corporate

High resolution

Never use stock images that feel generic.

---

# Sections

Hero

Featured News

Recommended Solutions

Technology Products

Case Studies

Training

Innovation

Newsletter

Footer

Every section should have different layout rhythm while maintaining consistency.

---

# White Space

Very important.

Leave generous spacing between:

Sections

Cards

Headings

Paragraphs

Buttons

Whitespace should be a design element.

---

# Grid System

Desktop

12-column grid

Tablet

8-column

Mobile

4-column

Card spacing

24px

Section spacing

96px

---

# Footer

Dark background

Simple multi-column layout

Company

Products

Resources

Support

Legal

Social

Minimal typography

No excessive decoration.

---

# Interaction Design

Hover

Border color change

Text color transition

Arrow movement

Card elevation

Buttons

Small scale

Smooth transition

Duration

200ms

Ease-in-out

Avoid flashy animations.

---

# Responsive Design

Desktop First

Then tablet

Then mobile

Navigation becomes hamburger.

Cards become stacked.

Images resize proportionally.

Maintain generous spacing.

---

# Accessibility

WCAG AA

High contrast

Keyboard navigation

Focus indicators

Readable typography

Accessible buttons

Proper heading hierarchy

ARIA labels

---

# Visual Hierarchy

Priority

1 Hero

2 Featured Technology

3 Solutions

4 Products

5 Case Studies

6 Learning

7 Newsletter

8 Footer

Users should understand the page in less than 5 seconds.

---

# Design Language

Keywords

Enterprise

Minimal

Professional

Elegant

Trustworthy

Intelligent

Modern

Technical

Corporate

Scalable

Clean

Sophisticated

Future-focused

High-end

Premium

---

# Component Design Rules

Buttons

Minimal

Cards

Bordered

Navigation

Simple

Dropdown

Clean

Forms

Large inputs

Simple validation

Tables

Minimal borders

Charts

Blue palette

Icons

Line style

Images

Professional

Spacing

Generous

Typography

Readable

Animations

Subtle

---

# Positive Aspects of IBM's Design

* Excellent use of whitespace that makes complex content easy to scan.
* Strong visual hierarchy with clear section separation.
* Consistent design system across cards, buttons, typography, and icons.
* Restrained color palette that reinforces trust and professionalism.
* Enterprise-focused aesthetic suitable for B2B audiences.
* Modular card layouts that are reusable and scalable.
* High accessibility through strong contrast and readable typography.
* Clear calls-to-action without overwhelming the user.
* Structured navigation that supports large information architectures.
* Balanced mix of imagery, illustrations, and text.

---

# Design Improvements (Optional Enhancements)

If modernizing this style while preserving its enterprise feel, consider:

* Adding subtle glassmorphism only where appropriate (avoid overuse).
* Introducing soft micro-interactions (fade, slide, scale) for cards and buttons.
* Using slight background color variations to improve section separation.
* Enhancing CTA buttons with gentle hover states.
* Incorporating tasteful gradients only as small accent elements, not full-page backgrounds.
* Supporting dark mode with a complete design token system.
* Adding tasteful scroll-triggered reveal animations while maintaining performance.
* Including a more prominent search experience for content-heavy sites.

---

## Final Design Objective

> Create a premium enterprise website inspired by IBM's visual language. The interface should feel trustworthy, clean, intelligent, and scalable. Use a restrained blue, white, gray, and black palette, generous whitespace, IBM Plex Sans-style typography, modular card layouts, subtle interactions, and a robust design system suitable for AI, cloud, analytics, cybersecurity, consulting, and other enterprise technology products. The result should look like a modern Fortune 500 technology company website rather than a startup landing page.
```

The IBM app should open the first login or sign-up page. When the user enters user ID, email, password, and clicks the submit button, the IBM app checks in the database whether the user ID, email, and password are present in the access database.

Then it checks which access role it is: CEO, manager, or test. If it doesn’t match, it checks the main database for access employees. If that still doesn’t match any user, it should return “User not found/Unauthorized.”

IBM App logic:

If the role is CEO: the person has full access and can see all employee and department performance information, including both department-wise and every single employee performance dashboards with charts.

If the role is manager: the person is the manager of a single department, so it should show only that department’s employees and the department’s performance dashboards (including both department-wise and every single employee performance dashboards with charts).

If the role is employee: the person can see only their own data and their employee performance dashboards with charts.

If the role is test: the test person can access all three roles, or can choose a role to test the IBM app functions.

This all will hIBM appen in `page.js` and redirects based on the role in:
- `/ceo`
- `/manage/[slug]`
- `/employee/[slug]`
- `/test/[slug]`

Routes:
- `/ceo` is the CEO role page.
- `/manage/[slug]` is the manage role page, and `[slug]` is the department; only that department data will show in that page.
- `/employee/[slug]` is the employee role page, and `[slug]` is the employee; only that employee data will show in that page.
- `/test/[slug]` is the test role page, and `[slug]` is the role; only that role data will show in that page.
- Include the IBM app route `/about` as well.

```
@pages
   # if access role is CEO
   - /ceo
   # if access role is manage
   - /manage
     # which department manage
     - /[slug]
   # if access role is employee
   - /employee
     # which employee
     - /[slug]
   # if access role is test
   - /test
     # which role
     - /[slug]
   # About this Web IBM App and analysis process
   - /about
```

The technology developer will use to make this IBM app: Next.js with JavaScript, Tailwind, React components, Supabase (PostgreSQL online server), Recharts, and ApexCharts (JavaScript chart library).

```
1. Frontend: Next.js with JavaScript
2. CSS: Tailwind (CSS framework)
3. Backend: Next.js
4. Database: Supabase (PostgreSQL online server)
5. Chart: Recharts and ApexCharts (JavaScript chart library)
```

While using any technology, framework, or library, use only the official full version.

The database contains the following column in the main table:

```
Age,Attrition,BusinessTravel,DailyRate,Department,DistanceFromHome,Education,EducationField,EmployeeCount,EmployeeNumber,EnvironmentSatisfaction,Gender,HourlyRate,JobInvolvement,JobLevel,JobRole,JobSatisfaction,MaritalStatus,MonthlyIncome,MonthlyRate,NumCompaniesWorked,Over18,OverTime,PercentSalaryHike,PerformanceRating,RelationshipSatisfaction,StandardHours,StockOptionLevel,TotalWorkingYears,TrainingTimesLastYear,WorkLifeBalance,YearsAtCompany,YearsInCurrentRole,YearsSinceLastPromotion,YearsWithCurrManager,EMP_ID,Experience Level,Promotion Status
```

In `/employee/[slug]` page, this dashboard chart will display in `page.js` file.

```
    Employee sirf apni details dekh sake. 
    
    Cards 
    • Name (EmployeeNumber)  
    • Department  
    • Job Role  
    • Salary  
    • Experience  
    • Years at Company  
    
    Charts 
    1. Salary Growth 
    Question 
    How much salary hike has the employee received? 
    Chart 
    Gauge Chart 
    Column 
    PercentSalaryHike 
    
    2. Experience Timeline 
    Question 
    How long has the employee worked? 
    Chart 
    Line Chart 
    Columns 
    YearsAtCompany 
    YearsInCurrentRole 
    YearsWithCurrManager 
    
    3. Satisfaction Scores 
    Question 
    How satisfied is the employee? 
    Chart 
    Radar Chart (Power BI) 
    or 
    Bar Chart 
    Columns 
    JobSatisfaction 
    EnvironmentSatisfaction 
    RelationshipSatisfaction 
    
    4. Promotion Status 
    Question 
    How many years since last promotion? 
    Chart 
    Card 
    Column 
    YearsSinceLastPromotion 
    
    5. Stock Option Level 
    Question 
    What stock option level does the employee have? 
    Chart 
    Card 
    Column 
    StockOptionLevel
```

In `/manage/[slug]` page, this dashboard chart will display in `page.js` file.

```
    Manager sirf apni team dekh sake. 
    
    KPI 
    • Team Size  
    • Team Attrition  
    • Average Salary  
    • Average Job Satisfaction  
    • Average Work Life Balance  
    
    Charts 
    1. Team Members by Job Role 
    Question 
    How many employees are in each role? 
    Chart 
    Bar Chart 
    
    2. Job Satisfaction 
    Question 
    Are employees satisfied with their jobs? 
    Chart 
    Pie Chart 
    Column 
    JobSatisfaction 
    
    3. Environment Satisfaction 
    Question 
    Is work environment affecting employees? 
    Chart 
    Bar Chart 
    Column 
    EnvironmentSatisfaction 
    
    4. Performance Rating 
    Question 
    How are employees performing? 
    Chart 
    Column Chart 
    Column 
    PerformanceRating 
    
    5. Training Analysis 
    Question 
    Who received more training? 
    Chart 
    Bar Chart 
    Column 
    TrainingTimesLastYear 
    
    6. Work Life Balance 
    Question 
    Do employees have a healthy work-life balance? 
    Chart 
    Donut Chart 
    Column 
    WorkLifeBalance 
    
    7. Promotion Analysis 
    Question 
    Which employees haven't received promotions? 
    Chart 
    Bar Chart 
    Column 
    YearsSinceLastPromotion 
    
    8. Experience Distribution 
    Question 
    How experienced are employees? 
    Chart 
    Histogram 
    Column 
    TotalWorkingYears
```
In `/ceo` page, this dashboard chart will display in `page.js` file.
```
    Purpose 
    Company ka overall performance aur employee attrition monitor karna. 
    KPI Cards 
    • Total Employees  
    • Active Employees  
    • Employees Left (Attrition)  
    • Attrition Rate %  
    • Average Salary  
    • Average Age  
    • Average Experience  
    • Average Job Satisfaction  
    
    Charts + Questions 
    1. Attrition Distribution 
    Question 
    How many employees left the company? 
    Chart 
    Pie Chart 
    Columns 
    • Attrition  
    
    2. Attrition by Department 
    Question 
    Which department has the highest employee attrition? 
    Chart 
    Bar Chart 
    Columns 
    • Department  
    • Attrition  
    
    3. Attrition by Gender 
    Question 
    Do male or female employees leave more frequently? 
    Chart 
    Stacked Bar Chart 
    Columns 
    • Gender  
    • Attrition  
    
    4. Attrition by Job Role 
    Question 
    Which job role experiences the highest attrition? 
    Chart 
    Horizontal Bar Chart 
    Columns 
    • JobRole  
    • Attrition  
    
    5. Salary Distribution 
    Question 
    How are employee salaries distributed? 
    Chart 
    Histogram 
    Column 
    • MonthlyIncome  
    
    6. Attrition by Overtime 
    Question 
    Does overtime increase attrition? 
    Chart 
    Clustered Bar Chart 
    Columns 
    • OverTime  
    • Attrition  
    
    7. Education Field Analysis 
    Question 
    Which education field has the highest attrition? 
    Chart 
    Bar Chart 
    Columns 
    • EducationField  
    • Attrition  
    
    8. Age Group Analysis 
    Question 
    Which age group leaves the company the most? 
    Chart 
    Column Chart 
    Create Age Groups 
    18-25 
    26-35 
    36-45 
    46-55 
    55+ 
    
    9. Monthly Income vs Attrition 
    Question 
    Does lower salary affect employee attrition? 
    Chart 
    Box Plot (Power BI/Python) 
    or 
    Scatter Chart 
    Columns 
    MonthlyIncome 
    Attrition 
    
    10. Years at Company 
    Question 
    Which employees leave early? 
    Chart 
    Histogram 
    Column 
    YearsAtCompany
```
At last test the website 

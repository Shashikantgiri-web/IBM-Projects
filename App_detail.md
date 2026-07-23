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

```

Now the analysis team provides the online server and the three dashboards to the developer to make the IBM app.

Your job, like a senior developer (not AI), is to make a web IBM app using this analysis that is SEO friendly, properly tested for every feature, responsive, and secured with security algorithms.

The term of IBM app prompt:

```
# IBM app design or theme as a prompt
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

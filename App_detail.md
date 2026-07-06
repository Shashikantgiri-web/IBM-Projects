This is a web analysis app.
The idea is inspired by a college website where students login and the website tries to show student details after proper analysis, but it doesn’t show it because the college doesn’t have an analysis team that performs analysis of every single student on a monthly basis.

I decided to make the app for a company because making it for the college doesn’t make sense.

Companies use their own development app to perform tasks, and every company wants to track the performance of its employees. The company also has an analysis team which uses company employee data to create analysis dashboards that use that analysis data. Then the dashboard developers make an app that shows:
- performance of every employee,
- performance of every department, and
- the overall company performance.

This is the basic idea and inspiration for the app. Now the real work starts. This is not a simple comment app that is made only by writing code; it needs analysis.

Break down these two parts: the developer and the analysis.

The analysis part will be handled by the analysis team. The team uses Excel to collect row data, clean it, remove duplication, and understand the data.

The team imports those Excel files into Power BI to create new columns and measures, using DAX queries, to make three dashboards.

In Power BI, the team makes three dashboards.

First is for CEO or admin dashboards. In these dashboards, the CEO can see the whole company’s detailed performance analysis, including which department category is performing good or bad, along with employee performance analysis.

Second is for manage or department-wise dashboards. In this, the team plans to use user slicers to select one single department to see only that department’s data, which makes the dashboards.

Third (last three dashboards) is for employee or single-employee dashboards. In this, the team uses the user slicer again to select one employee to see only that employee’s data, which makes the dashboards.

After making all three dashboards, the team copies all table data and pastes it into Excel to store all new changes.

The team creates an online PostgreSQL server on Supabase, which becomes the database of the app.

The team creates a new database name (access). In the app, access control is saved in that table. The team stores the CEO, managers, and test people’s user IDs, email, and passwords.

The team plans to use Python to convert all Excel table data into PostgreSQL tables on the online server:

```
import os
import pandas as pd
from sqlalchemy import create_engine
from sqlalchemy.engine import URL

url = URL.create(
    drivername="",
    username="",
    password="",
    host="",
    port="",
    database="postgres"
)

engine = create_engine(url)

excel_file = pd.read_excel(
    "Dataset_1.xlsx",
    sheet_name=None
)

for sheet_name, df in excel_file.items():

    # Clean column names
    df.columns = (
        df.columns
        .str.strip()
        .str.lower()
        .str.replace(" ", "_", regex=False)
        .str.replace("-", "_", regex=False)
    )

    # Clean table name
    table_name = (
        sheet_name.strip()
        .lower()
        .replace(" ", "_")
        .replace("-", "_")
    )

    print(f"\nImporting sheet: {sheet_name}")
    print(f"Creating table: {table_name}")
    print("Columns:", df.columns.tolist())

    df.to_sql(
        name=table_name,
        con=engine,
        if_exists='replace',
        index=False
    )

    print(f"✅ {table_name} imported successfully")
print("\n🎉 All sheets imported successfully!")
```

Now the analysis team provides the online server and the three dashboards to the developer to make the app.

Your job, like a senior developer (not AI), is to make a web app using this analysis that is SEO friendly, properly tested for every feature, responsive, and secured with security algorithms.

The term of app prompt:

```
# app design or theme as a prompt
```

The app should open the first login or sign-up page. When the user enters user ID, email, password, and clicks the submit button, the app checks in the database whether the user ID, email, and password are present in the access database.

Then it checks which access role it is: CEO, manager, or test. If it doesn’t match, it checks the main database for access employees. If that still doesn’t match any user, it should return “User not found/Unauthorized.”

App logic:

If the role is CEO: the person has full access and can see all employee and department performance information, including both department-wise and every single employee performance dashboards with charts.

If the role is manager: the person is the manager of a single department, so it should show only that department’s employees and the department’s performance dashboards (including both department-wise and every single employee performance dashboards with charts).

If the role is employee: the person can see only their own data and their employee performance dashboards with charts.

If the role is test: the test person can access all three roles, or can choose a role to test the app functions.

This all will happen in `page.js` and redirects based on the role in:
- `/ceo`
- `/manage/[slug]`
- `/employee/[slug]`
- `/test/[slug]`

Routes:
- `/ceo` is the CEO role page.
- `/manage/[slug]` is the manage role page, and `[slug]` is the department; only that department data will show in that page.
- `/employee/[slug]` is the employee role page, and `[slug]` is the employee; only that employee data will show in that page.
- `/test/[slug]` is the test role page, and `[slug]` is the role; only that role data will show in that page.
- Include the app route `/about` as well.

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
   # About this Web App and analysis process
   - /about
```

The technology developer will use to make this app: Next.js with JavaScript, Tailwind, React components, Supabase (PostgreSQL online server), Recharts, and ApexCharts (JavaScript chart library).

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
#table column name will go to come here
```

In `/employee/[slug]` page, this dashboard chart will display in `page.js` file.

```
1. Employee Information (Cards)
   Visual	Field
   Card	Employee_ID
   Card	Department
   Card	Gender
   Card	Job_Title
   Card	Current_Age
   Card	Years_At_Company
   Card	Experience_Level
   Card	Monthly_Salary
   Card	Salary_Level
   Card	Employee_Status
   Card	Promotion_Status
   Card	Retirement_Status

2. Performance Gauge
   Visual: Gauge
   Setting	Value
   Value	Percentage_PS
   Minimum	0
   Maximum	100
   Target	90

3. Satisfaction Gauge
   Setting	Value
   Value	Percentage_Satisfaction_score
   Minimum	0
   Maximum	100
   Target	85

4. Overall Performance Gauge
   Setting	Value
   Value	Overall_Performance_Index
   Minimum	0
   Maximum	100
   Target	90

5. Clustered Column Chart (Working Hours)
   Visual Clustered Column Chart
   X-Axis:
       Normal_Work_Hours
       Total_Work_Hours
       Overtime_Hours

6. Horizontal Bar Chart
   Categories
       Projects
       Training
       Sick Days
       Remote Work

   Values
       Projects_Handled
       Training_Hours
       Sick_Days
       Remote_Work_Frequency

7. Donut Chart (Project Capacity)
   Values
       Projects_Handled
       Remaining_Projects

   Legend
       Completed
       Remaining

8. Donut Chart (Career Progress)
   Values
       Career_Completed
       Career_Remaining

9. Bar Chart (Employee Scores)
   Axis
       Performance
       Satisfaction
       Overall Score

   Values
       Percentage_PS
       Percentage_Satisfaction_score
       Overall_Performance_Index
```

In `/manage/[slug]` page, this dashboard chart will display in `page.js` file.

```
1. Top KPI Cards
   Use these as cards:
   Total Employees = Count of EmpID
   Average Performance Score
   Average Monthly Salary
   Average Years at Company
   Average Training Hours
   Average Work-Life Balance
   Total Projects Handled
   Average Work Hours per Week

2. Performance Analysis
   Column Chart
       X-axis: Performance Rating
       Y-axis: Count of EmpID

   Bar Chart
       X-axis: Overall Performance Index
       Y-axis: Count of Employees

3. Salary Analysis
   Column Chart
       X-axis: Salary Level
       Y-axis: Average Monthly Salary

4. Workload Analysis
   Stacked Column Chart
       X-axis: Workload
       Legend: Overtime Category
       Values: Count of Employees

   Donut Chart
       Legend: Work Life Balance
       Values: Count of Employees

5. Training Analysis
   Line Chart
       X-axis: Training Level
       Y-axis: Average Performance Score

6. Promotion Analysis
   Column Chart
       Legend: Promotion Status
       Values: Count of Employees

   Bar Chart
       X-axis: Experience Level
       Y-axis: Promotion Count
```


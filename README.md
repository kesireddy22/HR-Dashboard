# HR Dashboard (Power BI)

A Power BI dashboard to analyze HR attrition, employee demographics, tenure, performance, and other people analytics metrics. This project provides interactive visuals and insights to help HR teams make data-driven decisions.

---

## Table of contents

- [🚀 Features](#-features)  
- [📂 Repository structure](#-repository-structure)  
- [🔧 Prerequisites](#-prerequisites)  
- [🗃 Data schema & preparation](#-data-schema--preparation)  
- [📊 Sample DAX measures & Power Query steps](#-sample-dax-measures--power-query-steps)  
- [📈 Visuals included](#-visuals-included)  
- [⚙ Customization guidance](#-customization-guidance)  
- [🤝 Contribution](#-contribution)  
- [📄 License](#-license)

---

## 🚀 Features

- Employee count, active employees, attrition counts and rates  
- Tenure calculations (months / years)  
- Attrition by department, job role, age group, gender etc.  
- Trend analysis over time (monthly / quarterly)  
- Interactive filtering / slicers: date, department, job role, demographic segments  
- Dashboard pages for summary + drill‐down into employee details

---

## 🔧 Prerequisites

- Power BI Desktop (latest version recommended)  
- (Optional) Power BI Pro / Service for publishing & sharing  
- Basic familiarity with Power Query (M language) and DAX formulas  
- CSV or Excel file of HR data with at least: employee identifiers, hiring & termination dates, demographic & job info

---

## 🗃 Data schema & preparation

Here are typical columns you'd have in your dataset:

| Column name        | Description                                                |
|--------------------|------------------------------------------------------------|
| EmployeeID         | Unique identifier for each employee                       |
| Name / FullName    | Employee name                                             |
| Gender             | Male / Female / Other                                      |
| Age                | Current age or birthdate                                   |
| Department         | Department / Business unit                                 |
| JobRole            | Specific job designation                                    |
| HireDate           | Date employee joined                                        |
| TerminationDate    | Date employee left (if applicable)                         |
| Attrition          | Yes / No or 1 / 0                                           |
| PerformanceRating  | Numeric or categorical rating                               |
| Education          | Level or field of education                                 |
| MaritalStatus      | Marital status                                             |
| MonthlyIncome      | Numeric salary / wage                                       |
| Other optional fields | Location, Manager, YearsAtCompany, etc.                |

### Sample Power Query (M) transformations

```m
let
    Source = Csv.Document(File.Contents("data/employees.csv"),[Delimiter=",", Columns=12, Encoding=1252, QuoteStyle=QuoteStyle.Csv]),
    PromoteHeaders = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),
    ChangedTypes = Table.TransformColumnTypes(PromoteHeaders,{
        {"EmployeeID", type text},
        {"HireDate", type date},
        {"TerminationDate", type date},
        {"MonthlyIncome", type number},
        {"PerformanceRating", Int64.Type}
    }),
    // Create Tenure in years (if still active, use today’s date)
    AddEndDate = Table.AddColumn(ChangedTypes, "EndDate", each if [Attrition] = "Yes" then [TerminationDate] else DateTime.Date(DateTime.LocalNow())),
    AddTenureDays = Table.AddColumn(AddEndDate, "TenureDays", each Duration.Days([EndDate] - [HireDate])),
    AddTenureYears = Table.AddColumn(AddTenureDays, "TenureYears", each Number.Round([TenureDays] / 365, 2)),
    // Age Groups
    AddAgeGroup = Table.AddColumn(AddTenureYears, "AgeGroup", each 
        if [Age] <= 25 then "18-25" 
        else if [Age] <= 35 then "26-35" 
        else if [Age] <= 45 then "36-45" 
        else "46+")
in
    AddAgeGroup
---

### Sample Dax Measures

-- Total Employees
TotalEmployees = COUNTROWS('Employees')

-- Attrition Count
AttritionCount = CALCULATE(
    COUNTROWS('Employees'),
    'Employees'[Attrition] = "Yes"
)

-- Active Employees
ActiveEmployees = TotalEmployees - AttritionCount

-- Attrition Rate (%)
AttritionRatePct = 
DIVIDE(
    [AttritionCount],
    [TotalEmployees],
    0
) * 100

-- Average Tenure (Years)
AverageTenureYears = AVERAGE('Employees'[TenureYears])

-- Attrition Rate by Department (%)
AttritionRateByDept = 
VAR DeptTotal = CALCULATE(COUNTROWS('Employees'), ALLEXCEPT('Employees', 'Employees'[Department]))
VAR DeptAttr = CALCULATE(COUNTROWS('Employees'), 'Employees'[Attrition] = "Yes", ALLEXCEPT('Employees', 'Employees'[Department]))
RETURN
    DIVIDE(DeptAttr, DeptTotal, 0) * 100

-- Trend of Attrition Over Time (Monthly)
AttritionByMonth = 
SUMMARIZE(
    'Employees',
    'Employees'[YearMonth],   -- assuming you have a YearMonth column (like YYYY-MM)
    "AttritionCount", CALCULATE(COUNTROWS('Employees'), 'Employees'[Attrition] = "Yes"),
    "TotalCount", COUNTROWS('Employees')
)

📈 Visuals included

KPI cards: Total Employees, Attrition Rate, Avg. Tenure

Line / Area chart: Attrition trend over time (monthly / quarterly)

Bar chart: Attrition count & rate by Department, JobRole

Pie / Donut charts: Attrition by AgeGroup, Gender

Matrix / Table: list of employees, maybe filterable, showing key metrics

Slicers / filters: Department, JobRole, Gender, Date range, AgeGroup

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



 ---
### 📈 Visuals included

- **KPI Cards**: Display key HR metrics such as  
  - Total Employees  
  - Attrition Rate  
  - Average Tenure  

- **Line / Area Chart**: Shows attrition trend over time (monthly / quarterly).  

- **Bar Chart**: Highlights attrition count and attrition rate by Department and Job Role.  

- **Pie / Donut Charts**: Provides demographic breakdown of attrition by Age Group and Gender.  

- **Matrix / Table**: Lists employees with key metrics, allowing filtering and drill-down.  

- **Slicers / Filters**: Interactive controls for Department, Job Role, Gender, Date Range, and Age Group.

  # Dashboard designing
Based on the received mock-ups, the team will start designing the visuals and creating measures as needed.
![HR Dashboard Demo](HR_Dashboard.gif)

## ✅ Project Outcome

- Built a **comprehensive HR Analytics Dashboard** to track employee demographics, attrition, and satisfaction.  
- Identified **high-attrition departments, job roles, and age groups**, enabling data-driven retention strategies.  
- Improved **decision-making** by highlighting at-risk employee segments and workforce patterns.  
- Monitored **average tenure, attrition rate trends, and department-wise breakdowns** for better planning.  
- Reduced manual reporting efforts through **automated, interactive Power BI visuals**.  
- Provided actionable insights for **HR strategy, employee engagement, and workforce forecasting**.  
- Created a foundation for future **predictive analytics** (e.g., attrition forecasting, risk factor analysis).  


 

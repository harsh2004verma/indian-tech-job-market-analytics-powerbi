# 📊 Indian Tech Job Market Analytics — Project Methodology

## 1. Project Overview

The **Indian Tech Job Market Analytics** project is an end-to-end Business Intelligence project developed using **Microsoft Power BI**.

The objective is to transform raw technology job-posting data into an interactive analytical dashboard that provides insights into:

* Job demand
* Hiring locations
* Job roles
* Salary patterns
* Experience requirements
* In-demand technical skills
* Company hiring activity
* Work modes
* Fresher opportunities

The project follows a complete data analytics workflow:

```text
Raw Data
   ↓
Data Understanding
   ↓
Data Cleaning
   ↓
Data Transformation
   ↓
Data Modeling
   ↓
DAX Measures
   ↓
Dashboard Development
   ↓
Analysis & Insights
   ↓
Documentation
```

---

# 2. Project Objectives

The main objectives of this project are:

1. Analyze the distribution of technology jobs across Indian cities.
2. Identify job roles with high hiring activity.
3. Analyze salary ranges across roles and locations.
4. Understand experience requirements.
5. Identify frequently requested technical skills.
6. Analyze hiring activity by company.
7. Compare Remote, Hybrid, and On-site opportunities.
8. Identify fresher-friendly opportunities.
9. Provide an interactive interface for exploring individual job postings.
10. Demonstrate practical Power BI, Power Query, and DAX skills.

---

# 3. Dataset

The project uses a dataset containing **23,000+ technology job postings**.

The dataset contains information about:

* Job identifiers
* Job titles
* Companies
* Company ratings
* Locations
* Cities
* Role categories
* Experience requirements
* Salary information
* Required skills
* Work modes
* Company sizes
* Posting information
* Skill domains
* Fresher suitability

### Important Dataset Fields

| Field                 | Purpose                         |
| --------------------- | ------------------------------- |
| `job_id`              | Unique job identifier           |
| `job_title`           | Job position                    |
| `company_name`        | Hiring organization             |
| `company_rating`      | Company rating                  |
| `location`            | Job location                    |
| `primary_city`        | Standardized city               |
| `role_category`       | Standardized role               |
| `experience_min_yrs`  | Minimum experience              |
| `experience_max_yrs`  | Maximum experience              |
| `salary_min_lpa`      | Minimum salary                  |
| `salary_max_lpa`      | Maximum salary                  |
| `salary_midpoint_lpa` | Salary midpoint                 |
| `salary_disclosed`    | Salary availability flag        |
| `skills_required`     | Skills required by the employer |
| `skills_count`        | Number of skills                |
| `skill_domain`        | Skill category                  |
| `work_mode`           | Remote/Hybrid/On-site           |
| `company_size_bucket` | Company size                    |
| `experience_tier`     | Experience classification       |
| `salary_tier`         | Salary classification           |
| `is_fresher_friendly` | Fresher suitability             |
| `is_senior`           | Senior-position indicator       |
| `salary_negotiable`   | Salary negotiability            |
| `days_since_posted`   | Age of job posting              |

---

# 4. Data Understanding

Before performing transformations, the dataset was reviewed to understand:

* Number of records
* Number of columns
* Data types
* Missing values
* Duplicate records
* Salary fields
* Experience fields
* Multi-value skill fields
* Categorical fields
* Boolean fields

The dataset was found to contain approximately **23,000+ job records**, making it suitable for meaningful job-market analysis.

---

# 5. Data Cleaning

Data cleaning was performed using **Power Query in Power BI**.

## 5.1 Data Type Validation

Columns were reviewed and assigned appropriate data types.

### Text fields

Examples:

```text
job_title
company_name
location
role_category
skills_required
work_mode
primary_city
skill_domain
```

### Numeric fields

Examples:

```text
company_rating
experience_min_yrs
experience_max_yrs
salary_min_lpa
salary_max_lpa
salary_midpoint_lpa
skills_count
days_since_posted
```

### Boolean fields

Examples:

```text
salary_disclosed
is_fresher_friendly
is_senior
salary_negotiable
```

Correct data types were important for accurate aggregation, filtering, and DAX calculations.

---

# 6. Missing Value Handling

Missing values were reviewed before analysis.

A key example is salary information.

Some job postings do not disclose salary.

Instead of deleting these records, the project preserves them because salary disclosure itself is useful information.

The field:

```text
salary_disclosed
```

is used to distinguish between:

```text
Salary Disclosed
```

and

```text
Salary Not Disclosed
```

This prevents undisclosed salary records from incorrectly affecting salary averages.

---

# 7. Salary Data Handling

Salary fields include:

```text
salary_min_lpa
salary_max_lpa
salary_midpoint_lpa
salary_disclosed
```

For salary analysis, only jobs with disclosed salary information are included in salary calculations.

For example:

```DAX
Average Salary =
CALCULATE(
    AVERAGE(Jobs[salary_midpoint_lpa]),
    Jobs[salary_disclosed] = TRUE()
)
```

This avoids treating undisclosed salary values as actual salaries.

---

# 8. Experience Data

Experience information is available through:

```text
experience_min_yrs
experience_max_yrs
experience_tier
```

These fields allow the dashboard to analyze:

* Entry-level opportunities
* Junior roles
* Mid-level roles
* Senior roles
* Experience requirements by job role
* Experience vs salary

An average experience value can also be calculated using the midpoint of the minimum and maximum experience requirements.

Example:

```DAX
Average Experience =
AVERAGEX(
    Jobs,
    DIVIDE(
        Jobs[experience_min_yrs] +
        Jobs[experience_max_yrs],
        2
    )
)
```

---

# 9. Skills Data Transformation

One of the most important transformations in this project is the treatment of the `skills_required` column.

A job posting can contain multiple skills in one field.

Example:

```text
Python, SQL, Power BI, Excel, Pandas
```

If this remains in one column, it becomes difficult to accurately count individual skills.

Therefore, a separate table called:

```text
JobSkills
```

was created.

---

# 10. JobSkills Table

The `JobSkills` table contains:

```text
job_id
skill
```

Example:

### Original Jobs Table

| job_id | job_title    | skills_required       |
| ------ | ------------ | --------------------- |
| 101    | Data Analyst | Python, SQL, Power BI |
| 102    | BI Analyst   | SQL, Excel, Tableau   |

### Transformed JobSkills Table

| job_id | skill    |
| ------ | -------- |
| 101    | Python   |
| 101    | SQL      |
| 101    | Power BI |
| 102    | SQL      |
| 102    | Excel    |
| 102    | Tableau  |

This transformation allows the project to calculate:

* Skill frequency
* Top skills
* Skill mentions
* Skill-domain demand
* Skill × Role relationships

---

# 11. Data Model

The primary table is:

```text
Jobs
```

The skill-level table is:

```text
JobSkills
```

The relationship is based on:

```text
Jobs[job_id]
        ↓
JobSkills[job_id]
```

Conceptually:

```text
                Jobs
                  │
                  │ job_id
                  │
                  ▼
              JobSkills
```

The `Jobs` table represents job postings, while `JobSkills` represents individual skills associated with those postings.

---

# 12. Analytical Measures

DAX measures were created to support dashboard analysis.

## Total Jobs

```DAX
Total Jobs =
COUNTROWS(Jobs)
```

## Total Companies

```DAX
Total Companies =
DISTINCTCOUNT(Jobs[company_name])
```

## Total Cities

```DAX
Total Cities =
DISTINCTCOUNT(Jobs[primary_city])
```

## Fresher Jobs

```DAX
Fresher Jobs =
CALCULATE(
    [Total Jobs],
    Jobs[is_fresher_friendly] = TRUE()
)
```

## Fresher Job %

```DAX
Fresher Job % =
DIVIDE(
    [Fresher Jobs],
    [Total Jobs],
    0
)
```

## Salary Disclosure %

```DAX
Salary Disclosure % =
DIVIDE(
    CALCULATE(
        [Total Jobs],
        Jobs[salary_disclosed] = TRUE()
    ),
    [Total Jobs],
    0
)
```

---

# 13. Dashboard Design

The dashboard was divided into multiple analytical pages instead of placing all visuals on one page.

The final structure consists of:

```text
Page 1 → Market Overview
Page 2 → Salary Intelligence
Page 3 → Skills Intelligence
Page 4 → Company Analysis
Page 5 → Fresher Opportunities
Page 6 → Job Explorer
```

---

# 14. Page 1 — Market Overview

The Market Overview page provides a high-level summary of the job market.

### KPIs

* Total Jobs
* Total Companies
* Total Cities
* Fresher Jobs

### Visualizations

* Jobs by City
* Jobs by Role
* Jobs by Work Mode
* Jobs by Experience Tier

### Purpose

This page helps users quickly understand the overall distribution of job opportunities.

---

# 15. Page 2 — Salary Intelligence

The Salary Intelligence page focuses on compensation patterns.

### KPIs

* Average Salary
* Maximum Salary
* Minimum Salary
* Salary Disclosure %

### Visualizations

* Average Salary by Role
* Average Salary by City
* Salary vs Experience
* Salary Tier Distribution

### Purpose

This page helps analyze how advertised salary varies across different roles, locations, and experience levels.

---

# 16. Page 3 — Skills Intelligence

The Skills Intelligence page analyzes employer skill requirements.

### KPIs

* Unique Skills
* Skill Mentions
* Jobs With Skills
* Average Skills per Job

### Visualizations

* Top 10 Skills
* Skill Domain Distribution
* Jobs by Skill Domain
* Skill × Role Matrix
* Skill Detail Table

### Purpose

This page identifies the technical skills most frequently associated with technology job postings.

---

# 17. Page 4 — Company Analysis

The Company Analysis page focuses on employer activity.

### Visualizations

* Top Hiring Companies
* Company Rating vs Jobs
* Jobs by Company Size
* Hiring Activity by Company

### Purpose

This page provides an employer-level view of job-posting activity.

---

# 18. Page 5 — Fresher Opportunities

The Fresher Opportunities page focuses on entry-level positions.

### KPIs

* Fresher Jobs
* Fresher Job %
* Fresher Jobs With Salary
* Fresher Salary Disclosure %

### Visualizations

* Fresher Jobs by City
* Fresher Jobs by Role
* Fresher Jobs by Work Mode
* Fresher Jobs by Experience Tier

### Purpose

This page provides a focused analysis of jobs classified as suitable for freshers.

---

# 19. Page 6 — Job Explorer

The Job Explorer provides detailed job-level information.

Users can filter the data using:

* City
* Job Role
* Experience
* Work Mode
* Salary Tier
* Skill Domain
* Company Size

The detailed table can contain:

```text
Job Title
Company
City
Role
Experience
Salary
Work Mode
Skills
Job URL
```

### Purpose

The Job Explorer allows users to move from high-level market analysis to individual job records.

---

# 20. Interactivity

Interactive features were incorporated into the dashboard to improve exploration.

These include:

* Slicers
* Cross-filtering
* Visual interactions
* Drill-through
* Tooltips
* Conditional formatting
* Dynamic DAX measures

Example:

A user can select:

```text
City = Bangalore
Role = Data Analyst
Work Mode = Hybrid
```

and the dashboard updates the relevant metrics and charts.

---

# 21. Dashboard Design Principles

The dashboard follows basic BI visualization principles:

### Consistency

The same fonts, spacing, and visual hierarchy are used throughout the pages.

### Simplicity

Only relevant visualizations are included.

### Readability

Charts and KPIs have descriptive titles.

### Interactivity

Users can filter the dashboard according to their analytical requirements.

### Business Focus

Each page is designed around a specific analytical question.

---

# 22. Validation

Before finalizing the dashboard, the following checks should be performed:

### Record Count

Confirm that the number of records in Power BI matches the source dataset.

### Duplicate Check

Check whether `job_id` contains unexpected duplicates.

### Salary Validation

Confirm that salary calculations exclude undisclosed salary records.

### Experience Validation

Check that minimum experience is not greater than maximum experience.

### Skill Validation

Verify that the JobSkills table correctly splits multiple skills into individual rows.

### Filter Validation

Test slicers and cross-filtering across all dashboard pages.

### KPI Validation

Manually verify selected KPI calculations against the underlying data.

---

# 23. Analytical Approach

The project follows a layered analytical approach.

## Layer 1 — Descriptive Analysis

Answers:

> What is happening?

Examples:

* Number of jobs
* Number of companies
* Jobs by city
* Jobs by role

## Layer 2 — Comparative Analysis

Answers:

> How do categories differ?

Examples:

* Salary by role
* Salary by city
* Jobs by work mode
* Fresher opportunities by city

## Layer 3 — Relationship Analysis

Answers:

> How are variables related?

Examples:

* Salary vs experience
* Company rating vs hiring activity
* Skills vs job roles

## Layer 4 — Detailed Exploration

Answers:

> What individual records make up the summary?

This is handled through the Job Explorer page.

---

# 24. Key Business Questions

The completed dashboard is designed to answer:

### Demand

* Which cities have the highest number of job postings?
* Which roles have the highest demand?
* Which companies have the most postings?

### Salary

* What is the average advertised salary?
* Which roles have higher salary ranges?
* How does salary vary by city?
* How does salary relate to experience?

### Skills

* Which skills are most frequently requested?
* Which skill domains are most common?
* Which skills appear across different roles?

### Experience

* What proportion of jobs are entry-level?
* Which roles require more experience?
* How does experience vary by role?

### Fresher Opportunities

* How many jobs are fresher-friendly?
* Which cities contain the most fresher-friendly jobs?
* Which roles have more entry-level opportunities?

### Work Mode

* What proportion of jobs are Remote, Hybrid, or On-site?
* How does work mode vary by role and city?

---

# 25. Important Analytical Considerations

The dashboard represents the job postings available in the dataset and should not automatically be interpreted as a complete representation of the entire Indian technology job market.

Important considerations include:

* The dataset may represent only selected job sources.
* Job postings can change over time.
* A single company may post multiple jobs.
* Salary information may not be available for every posting.
* Job classifications may depend on the dataset's categorization.
* Skill names may have variations in spelling or terminology.
* Job-posting counts represent postings, not necessarily unique vacancies or hires.

Therefore, dashboard results should be interpreted within the context of the dataset.

---

# 26. Project Outcome

The project transforms raw job-posting data into a structured Business Intelligence solution.

The final dashboard enables users to move from:

```text
Raw Job Data
      ↓
Clean Data
      ↓
Structured Data Model
      ↓
DAX Analysis
      ↓
Interactive Dashboard
      ↓
Business Insights
```

The project demonstrates practical capabilities in:

* Power BI
* Power Query
* DAX
* Data Modeling
* Data Cleaning
* Data Visualization
* Exploratory Data Analysis
* Business Intelligence
* Dashboard Design
* Analytical Storytelling

---

# 27. Tools Used

```text
Power BI
Power Query
DAX
Excel
CSV
GitHub
```

---

# 28. Final Deliverables

The completed project should contain:

```text
✓ Power BI Dashboard
✓ Cleaned Dataset
✓ Jobs Data Model
✓ JobSkills Data Model
✓ DAX Measures
✓ Dashboard Screenshots
✓ Data Dictionary
✓ Project Methodology
✓ GitHub README
✓ Analytical Insights
```

---

# 29. Project Workflow Summary

```text
                    RAW CSV
                       │
                       ▼
              DATA UNDERSTANDING
                       │
                       ▼
                POWER QUERY
                       │
            ┌──────────┴──────────┐
            │                     │
       DATA CLEANING        DATA TRANSFORMATION
            │                     │
            └──────────┬──────────┘
                       ▼
                  DATA MODEL
                       │
                ┌──────┴──────┐
                │             │
               Jobs        JobSkills
                │             │
                └──────┬──────┘
                       ▼
                     DAX
                       │
                       ▼
                POWER BI PAGES
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
      Salary         Skills        Companies
        │              │              │
        └──────────────┼──────────────┘
                       ▼
                 FINAL INSIGHTS
                       │
                       ▼
                 GITHUB PORTFOLIO
```

---

# 30. Conclusion

The **Indian Tech Job Market Analytics** project demonstrates an end-to-end approach to converting raw job-market data into actionable business intelligence.

By combining Power Query for data preparation, a structured data model, DAX for analytical calculations, and Power BI for interactive visualization, the project provides a comprehensive framework for exploring technology hiring trends in India.

The project also demonstrates how data can be transformed from a raw collection of job postings into a structured analytical product suitable for portfolio and professional use.

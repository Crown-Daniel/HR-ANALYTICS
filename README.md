# HR-ANALYTICS
## HR data analysis that tries to understand the reason staffs are leaving the company.
## TABLE OF CONTENTS

- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Analysis](#analysis)
- [Tools](#tools)
- [Data Cleaning/Preparation](#data-cleaning/preparation)
- [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-(EDA))
- [Results](#results)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
- [References](#references)
  
### Project Overview
This project attempt to look into the issue of staff churn using data such as Employee Count, Attrition, Business Travel, CF_age band, CF_attrition label, Department, Education Field, emplo no, Employee Number, Gender, Job Role, Marital Status, Over Time, Over18, Training Times Last Year, Age, CF_current Employee, Daily Rate, Distance From Home, Education, Employee Count, Environment Satisfaction, Hourly Rate, Job Involvement, Job Level, Job Satisfaction, Monthly Income, Monthly Rate, Num Companies Worked, Percent Salary Hike, Performance Rating, Relationship Satisfaction, Standard Hours, Stock Option Level, Total Working Years, Work Life Balance, Years At Company, Years In Current Role, Years Since Last Promotion, Years With Curr Manager. Stakeholders want to know why staffs are leaving the company, wants to know the department with the highest number of staffs that left and there gender also there average age, taking note of employee count, attrition, attrition rate, active employees and average age of employees filtered by education.

![Screenshot (1)](https://github.com/user-attachments/assets/75a11e13-5697-459e-816a-0267b8edf135)


### Data Sources
The primary dataset used for this analysis is A fiction India company. [link](https://docs.google.com/spreadsheets/...)

### Analysis
Analysis conducted using Bigquery SQL codes.

 Data Cleaning and Formatting (including Null Handling)

 ```SQL
CREATE OR REPLACE TABLE `my-project-0003-439503.hr.hr_data_cleaned` AS
SELECT
  IFNULL(Attrition, 'Unknown') AS Attrition,
  IFNULL(`Business Travel`, 'Unknown') AS `Business Travel`,
  IFNULL(`CF_age band`, 'Unknown') AS `CF_age band`,
  IFNULL(`CF_attrition label`, 'Unknown') AS `CF_attrition label`,
  IFNULL(Department, 'Unknown') AS Department,
  IFNULL(`Education Field`, 'Unknown') AS `Education Field`,
  `emp no`,
  `Employee Number`,
  IFNULL(Gender, 'Unknown') AS Gender,
  IFNULL(`Job Role`, 'Unknown') AS `Job Role`,
  IFNULL(`Marital Status`, 'Unknown') AS `Marital Status`,
  IFNULL(`Over Time`, 'Unknown') AS `Over Time`,
  Over18,
  IFNULL(`Training Times Last Year`, 0) AS `Training Times Last Year`,
  IFNULL(Age, 0) AS Age,
  IFNULL(`CF_current Employee`, 'Unknown') AS `CF_current Employee`,
  IFNULL(`Daily Rate`, 0) AS `Daily Rate`,
  IFNULL(`Distance From Home`, 0) AS `Distance From Home`,
  IFNULL(Education, 0) AS Education,
  IFNULL(`Employee Count`, 0) AS `Employee Count`,
  IFNULL(`Environment Satisfaction`, 0) AS `Environment Satisfaction`,
  IFNULL(`Hourly Rate`, 0) AS `Hourly Rate`,
  IFNULL(`Job Involvement`, 0) AS `Job Involvement`,
  IFNULL(`Job Level`, 0) AS `Job Level`,
  IFNULL(`Job Satisfaction`, 0) AS `Job Satisfaction`,
  IFNULL(`Monthly Income`, 0) AS `Monthly Income`,
  IFNULL(`Monthly Rate`, 0) AS `Monthly Rate`,
  IFNULL(`Num Companies Worked`, 0) AS `Num Companies Worked`,
  IFNULL(`Percent Salary Hike`, 0) AS `Percent Salary Hike`,
  IFNULL(`Performance Rating`, 0) AS `Performance Rating`,
  IFNULL(`Relationship Satisfaction`, 0) AS `Relationship Satisfaction`,
  IFNULL(`Standard Hours`, 0) AS `Standard Hours`,
  IFNULL(`Stock Option Level`, 0) AS `Stock Option Level`,
  IFNULL(`Total Working Years`, 0) AS `Total Working Years`,
  IFNULL(`Work Life Balance`, 0) AS `Work Life Balance`,
  IFNULL(`Years At Company`, 0) AS `Years At Company`,
  IFNULL(`Years In Current Role`, 0) AS `Years In Current Role`,
  IFNULL(`Years Since Last Promotion`, 0) AS `Years Since Last Promotion`,
  IFNULL(`Years With Curr Manager`, 0) AS `Years With Curr Manager`
FROM
  `my-project-0003-439503.hr.hr_data`;
```

-- Optional: Data Type Conversion (if needed, adjust data types)

```SQL
CREATE OR REPLACE TABLE `my-project-0003-439503.hr.hr_data_cleaned` AS
SELECT
  Attrition,
  `Business Travel`,
  `CF_age band`,
  `CF_attrition label`,
  Department,
  `Education Field`,
  `emp no`,
  CAST(`Employee Number` AS INT64) AS `Employee Number`,
  Gender,
  `Job Role`,
  `Marital Status`,
  `Over Time`,
  Over18,
  CAST(`Training Times Last Year` AS INT64) AS `Training Times Last Year`,
  CAST(Age AS INT64) AS Age,
  `CF_current Employee`,
  CAST(`Daily Rate` AS FLOAT64) AS `Daily Rate`,
  CAST(`Distance From Home` AS INT64) AS `Distance From Home`,
  CAST(Education AS INT64) AS Education,
  CAST(`Employee Count` AS INT64) AS `Employee Count`,
  CAST(`Environment Satisfaction` AS INT64) AS `Environment Satisfaction`,
  CAST(`Hourly Rate` AS FLOAT64) AS `Hourly Rate`,
  CAST(`Job Involvement` AS INT64) AS `Job Involvement`,
  CAST(`Job Level` AS INT64) AS `Job Level`,
  CAST(`Job Satisfaction` AS INT64) AS `Job Satisfaction`,
  CAST(`Monthly Income` AS FLOAT64) AS `Monthly Income`,
  CAST(`Monthly Rate` AS FLOAT64) AS `Monthly Rate`,
  CAST(`Num Companies Worked` AS INT64) AS `Num Companies Worked`,
  CAST(`Percent Salary Hike` AS FLOAT64) AS `Percent Salary Hike`,
  CAST(`Performance Rating` AS INT64) AS `Performance Rating`,
  CAST(`Relationship Satisfaction` AS INT64) AS `Relationship Satisfaction`,
  CAST(`Standard Hours` AS INT64) AS `Standard Hours`,
  CAST(`Stock Option Level` AS INT64) AS `Stock Option Level`,
  CAST(`Total Working Years` AS INT64) AS `Total Working Years`,
  CAST(`Work Life Balance` AS INT64) AS `Work Life Balance`,
  CAST(`Years At Company` AS INT64) AS `Years At Company`,
  CAST(`Years In Current Role` AS INT64) AS `Years In Current Role`,
  CAST(`Years Since Last Promotion` AS INT64) AS `Years Since Last Promotion`,
  CAST(`Years With Curr Manager` AS INT64) AS `Years With Curr Manager`
FROM
  `my-project-0003-439503.hr.hr_data_cleaned`;
```
Analysis Queries

-- 1. Department with Highest Attrition and Gender Breakdown

```SQL
SELECT
  Department,
  Gender,
  COUNT(*) AS AttritionCount
FROM
  `my-project-0003-439503.hr.hr_data_cleaned`
WHERE
  Attrition = 'Yes'
GROUP BY
  Department,
  Gender
ORDER BY
  AttritionCount DESC;
```
-- 2. Average Age of Those Who Left

```SQL
SELECT
  AVG(Age) AS AverageAgeOfAttrition
FROM
  `my-project-0003-439503.hr.hr_data_cleaned`
WHERE
  Attrition = 'Yes';
```
-- 3. Employee Count, Attrition, Attrition Rate, Active Employees, and Average Age by Education

```SQL
SELECT
  `Education Field`,
  COUNT(*) AS TotalEmployees,
  SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) AS AttritionCount,
  SUM(CASE WHEN Attrition = 'No' THEN 1 ELSE 0 END) AS ActiveEmployees,
  (SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) / COUNT(*)) AS AttritionRate,
  AVG(CASE WHEN Attrition = 'No' THEN Age ELSE NULL END) AS AverageActiveEmployeeAge,
  AVG(CASE WHEN Attrition = 'Yes' THEN Age ELSE NULL END) AS AverageAttritionEmployeeAge
FROM
  `my-project-0003-439503.hr.hr_data_cleaned`
GROUP BY
  `Education Field`;
```
-- 4. potential correlation to why staffs are leaving.
-- Example 1: Attrition by Job role.

```SQL
SELECT `Job Role`, count(Attrition) as Attrition_Count from `my-project-0003-439503.hr.hr_data_cleaned` where Attrition = "Yes" group by `Job Role` order by count(Attrition) desc;

-- example 2. Attrition by Years at company.
SELECT `Years At Company`, count(Attrition) from `my-project-0003-439503.hr.hr_data_cleaned` where Attrition = "Yes" group by `Years At Company` order by `Years At Company`;

-- example 3. Attrition by over time.
SELECT `Over Time`, count(Attrition) from `my-project-0003-439503.hr.hr_data_cleaned` where Attrition = "Yes" group by `Over Time` order by count(Attrition) desc;
```

### Tools
Big query SQL Server and codes - Analysis.

Excel

Google spreadsheet - Data used for analysis. [link]( https://docs.google.com/spreadsheets/...)

Tableau public dashboard - Creating report and analysis. [link](https://public.tableau.com/app/profile/daniel.iwunze/viz/IwunzeDaniel/HRANALYTICSDASHBOARD)

### Data Cleaning/Preparation

In the initial data preparation phase, we performed the following tasks:

Data loading and inspection.

Handling missing values.

Data cleaning and formatting.

### Exploratory Data Analysis (EDA)

EDA involved exploring the employee count, attrition, attrition rate, active employees and average age of employees filtered by education data to answer key questions, such as:

what is the overall employee churn trend?

which products are top sellers?

What department have the highest churn?


### Results

The analysis results are summarized as follows:

The company's staff have been steadily decreasing over the past period.

R and D department have the highest attrition of 56.12%.

Sales department have the second highest attrition of 38.82%.

HR department have an attrition of 5.06%.

### Recommendations

Based on the analysis, we recommend the following actions:

Manegement should focus more attention on R and D as well as on sales department.

Focus on expanding and promoting packages that will increase job satisfaction.

### Limitations
Analysis conducted is strictly based on the data provided.

### References
Gemini

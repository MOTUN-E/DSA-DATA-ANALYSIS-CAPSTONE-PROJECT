# DSA-Project-on-Palmora-Group-HR-Analysis-CS.3
Exploratory Analysis of Gender-Related Issues Using Power BI

## Project Overview
This project analyzes HR data from the Palmora Group to identify gender-related issues in the organization. The analysis was performed in Power BI using two datasets:
- `Palmoria Group emp-data.csv` – the main employee dataset
- `Palmoria Group Bonus Rules.xlsx` – bonus rate mapping based on department and rating


## Data Preparation & Cleaning

### 1. Imported and Transformed Employee Data
- Loaded the Palmoria Group emp-data.csv into Power BI via Transform Data.
- Applied the following cleaning steps:
  * Removed rows with missing values in Salary and Department columns.
  * Replaced missing or unspecified Gender with `"N/A"`.
  * Renamed Location column to Region, as clarified by the project brief.
  * Standardized Rating to a numeric column by creating a custom column `Numeric Rating` with the following mappings:
      - Very Poor → 1
      - Average → 2
      - Good → 3
      - Very Good → 4
      - Excellent → 5
      - Not Related → 0 

### 2. Imported Bonus Rules and Unpivoted
- Imported the Palmoria Group Bonus Rules.xlsx file into Power BI.
- Selected only the Bonus Mapping sheet and unpivoted all columns except `Department`.
- Created a new column named `Dept_Rating` by inputing [Department] & "/" & [Attribute]
  
### 3. Merging Datasets
- In the employee table, created a matching `Dept_Rating` column using: [Department] & "/" & [Rating]
-  Merged both datasets using the `Dept_Rating` column via Merge Queries as New.
- Renamed `Value` column to `Bonus Rate` and `Name` column to `Employee Name`, replaced `null` with `0`.

### 4. Calculated Additional Columns
- Annual Bonus: Calculated using: [Salary] * [Bonus Rate]
- Total Pay (Salary + Bonus): [Salary] + [Annual Bonus]
- - Salary Band: Created a conditional column grouping salaries into $10,000 bands:
- `$10,000 - $20,000`, `$20,000 - $30,000`, ..., `Above $90,000`

### 5. Final Applied Changes
- Converted Salary, Annual Bonus, and Total Pay to Currency format (USD).
- Ensured `Numeric Rating` is in decimal number format.
- Verified all calculated columns for accuracy.
- Closed and applied all transformations.
- Then remaned the new merged table as `Employee Table`


## Pointers 1
### Gender Distribution Analysis

QS: What is the gender distribution in the organization? Distil to regions and departments.

### Visuals

Bar Chart: Gender Distribution by Region/Department.
- Visualization Type: Stacked Bar Chart; X-Axis: Gender Count, Y-Axis: Region, Legend: Gender
Slice: To filter by Region/Department
Pie Chart: Overall Gender Distribution.
 - Visualization Type: Pie Chart; Legend: Gender, Values: Gender Percentage

### DAX Measures Used

- Gender Count = COUNT('Employee Table'[Gender])
- Gender Percentage =
DIVIDE(
    CALCULATE(COUNT('Employee Table'[Gender])),
    CALCULATE(COUNT('Employee Table'[Gender]), ALL('Employee Table'[Gender]))
)

### Images

Gender Distribution by Region
![Gender by Region](./Gender Distribution by Region.png)

#### Gender Distribution by Department
![Gender by Department](./gender_distribution_department.png)

#### Overall Gender Distribution
![Gender Pie](./gender_distribution_pie.png)


Insight and Interpretation

The company has a higher percentage of Male employees(49.15%) compared to Female(46.62%), with a small portion marked as N/A94.23%) where gender wasn’t specified.

Across regions, this disparity is most pronounced in [insert region based on chart], where male employees dominate.
Similarly, within departments, [insert department] shows the widest gender gap.

The presence of N/A values indicates missing or undisclosed gender data, which should be investigated to ensure inclusivity in record keeping.

 






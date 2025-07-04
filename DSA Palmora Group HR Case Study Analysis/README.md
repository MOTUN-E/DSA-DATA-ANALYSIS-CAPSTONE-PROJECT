# DSA-Project-on-Palmora-Group-HR-Analysis-CS.3
Exploratory Analysis of Gender-Related Issues Using Power BI

## Project Overview
This project analyzes HR data from the Palmora Group to identify gender-related issues in the organization. The analysis was performed in Power BI using two datasets:
- `Palmoria Group emp-data.csv` – the main employee dataset
- `Palmoria Group Bonus Rules.xlsx` – bonus rate mapping based on department and rating

- Insights include:
- Gender distribution across departments and salary bands
- Bonus and performance rating differences by gender
- Visualization of pay gaps, department representation, and policy thresholds

* Tool: Power BI with DAX  
* Goal: Support HR decision-making on gender equity and compensation fairness


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

Slicer: To filter by Region/Department

Pie Chart: Overall Gender Distribution.
 - Visualization Type: Pie Chart; Legend: Gender, Values: Gender Percentage


### Images

Gender Distribution by Region  
![Gender Distribution by Region](Gender%20Distribution%20by%20Region%281%29.png)

Slicer by Dept.  
![Slicer by Department](Slicer%20by%20Department%20.png)

Overall Gender Distribution
![Overall Gender Distribution](OverallGenderDistribution.png)


### DAX Measures Used

- Gender Count = COUNT('Employee Table'[Gender])
- Gender Percentage =
DIVIDE(
    CALCULATE(COUNT('Employee Table'[Gender])),
    CALCULATE(COUNT('Employee Table'[Gender]), ALL('Employee Table'[Gender]))
)

### Insight and Interpretation

The company has a higher percentage of Male employees(49.15%) compared to Female(46.62%), with a small portion marked as N/A(4.23%) where gender wasn’t specified.

Across regions, this disparity is most pronounced in Kaduna, where male employees dominate with 182 employee and females 165.
Similarly, within departments, Legal dept.shows the widest gender gap males recording 49 while females 34.

The presence of N/A values indicates missing or undisclosed gender data, which should be investigated to ensure inclusivity in record keeping.



## Pointer 2
### Rating Distribution and Comparison by Gender

QA: What are the employee ratings by gender? Show insights on ratings based on gender.

### Visuals

Histogram: Ratings Distribution by Gender
- Visualization Type: Clustered Column Chart; X-Axis: Rating, Y-Axis: Employee Name, Legend: Gender

Slicer: To filter by Gender
- Visualization Type: Slicer; Field: Gender
 
Card Visual: Rating Comparison by Gender(Male, Female, N/A)
- Visualization Type: Values; Average Rating (calculated using a measure)

#### Images

Rating Distribution by Gender
![Rating Distribution by Gender](RatingDistributionbyGender.png)

 Rating Comparison by Gender  
![Rating Comparison](Rating%20Comparison%20by%20Gender%282%29.png)

Slicer by Gender
![Slicer by Gender](SlicerbyGender.png)

### DAX Measures Used

- Average Rating by Gender =
AVERAGE('Employee Table'[Rating]),
- Rating Count by Gender =
COUNT('Employee Table'[Numeric Rating])

Note: "Numeric Rating" is a column created during transformation that maps textual ratings (e.g., "Very Good", "Poor") into a numerical scale (e.g., 5 to 1), to enable calculation of averages and comparisons.

### Insight and Interpretation
The clustered column chart shows that the majority of employees across genders received "Average" and "Good" ratings.
Average Rating from the card visuals indicates that females slightly outperform males, scoring 2.96 on average vs 2.83. Gender N/A group also performed more(3.10), suggesting no clear bias in rating distribution.

The slicer enhances interactivity, allowing management to isolate and review trends specific to each gender.

These insights help highlight how performance evaluation is distributed across gender groups and where improvements in transparency or consistency may be needed.



## Pointer 3 
### Salary Structure and Gender Pay Gap Analysis

QA: Analyse the company’s salary structure. Identify if there is a gender pay gap. If there is, identify the department and regions that should be the focus of management.

### Visuals

Scatter Plot: "Salary vs. Gender"
- Visualization Type: Scatter Chart; X-Axis: Salary, Y-Axis: Gender, Values: Emplyee Name

Bar Chart: "Average Salary by Department and Region"
- Visualization Type: Clustered Bar Chart; X-Axis: Average Salary by Gender, Y-Axis: Region, Legend: Gender

Slicers: Filter by Dept.

Card Visual: "Average Salary by Gender"
- Visualization Type: Card; Values: Average Salary by Gender(Male, Female, N/A)

### DAX Measures Used

- Average Salary by Gender =
AVERAGE('Employee Table'[Salary])

- Salary Count by Department and Region =
COUNT('Employee Table'[Salary])

### Images

Salary vs Gender  
![Salary vs Gender](Salary%20VS%20Gender%283%29.png)

Average Salary by Department and Region  
![Average Salary by Department and Region](Average%20Salary%20by%20Department%20and%20Region%283%29.png)

Slicer by Dept.  
![Slicer by Department](Slicer%20by%20Department%20.png)

Average Salary by Gender  
![Average Salary by Gender](Average%20Salary%20by%20Gender%283%29.png)

### Insight and Interpretation

The scatter plot reveals a clustering pattern: male employees are more heavily concentrated in the upper salary band compared to females and N/A genders.

The bar chart provides a clear view of average salary differences across departments and regions. Certain regions like Lagos show a notable pay disparity.

Card visuals show that the average salary for male employees is slightly higher than their female counterparts, supporting concerns about a potential gender pay gap.

The salary count measure helps quantify the concentration of salaries within each department/region to identify specific areas of concern.

### Recommendation: The HR team should conduct an equity review in departments/regions with the highest disparities, and implement corrective policies where applicable.


## Pointer 4 
### Minimum Salary Requirement Analysis

QA: A recent regulation was adopted which requires manufacturing companies to pay employees a minimum of $90,000
- Does Palmoria meet this requirement?
- Show the pay distribution of employees grouped by a band of $10,000.
- How many employees fall into a band of $10,000 – $20,000, $20,000 – $30,000, etc.?
- Also visualize this by regions.

### Visuals

Histogram: "Salary Distribution by $10,000 Band"
- Visualization Type: Clustered Column Chart; X-Axis: Salary Band, Y-Axis: Employees Name, Legend: Gender
slicer: Fliter Histogram by Region
Bar Chart: "Employees Below Minimum Salary Threshold by Region"
- Visualization Type: Clustered Column Chart; X-Axis: Region, Y-Axis: Employees Below Minimum Salary, legend: Gender 
Card Visual:  "Employees Below Minimum Salary"
- Visualization Type: Card; Values: Employees Below Minimum Salary

### Images

Salary Distribution by $10,000 Band
![Salary Distribution by $10,000 Band](SalaryDistributionby$10,000Band.png)

Employees Below Minimum Salary  
![Below Minimum Salary](Employee%20Below%20Minimum%20Salary%284%29.png)

Employees Below Minimum Salary Threshold by Region  
![Below Minimum Salary by Region](Employee%20Below%20Minimum%20Salary%20Threhold%20by%20Region%284%29.png)

Slicer by Region  
![Slicer by Region](Slicer%20by%20Region%284%29.png)


### Insight and Interpretation
The histogram clearly shows that although a large portion of employees fall into the above $90,000 the addition of those below are more, indicating many employees are close to but below the required threshold.

The bar chart by region reveals that Kaduna has the highest number of employees earning below $90,000. This helps management target specific regions for salary adjustments.

The card visual displays the total number of employees who fall below the required minimum salary(654) which is more then 70% of the entire employee.

Salary Banding was done using a conditional column in Power Query, grouping salaries in $10,000 increments.

 ### Recommendation
 Immediate salary reviews should be conducted in regions and departments with a high number of employees below the $90,000 threshold to ensure regulatory compliance.



## Pointer 5 
### Bonus Payment Calculation 

QA: Help allocate the annual bonus pay to employees based on the performance rating, using the bonus rules provided.
- Calculate the amount to be paid as a bonus to individual employees.
- Calculate the total amount to be paid to individual employees (salary + bonus).
- Calculate the total amount to be paid out per region and company-wide.

### Visuals

Bar Chart: "Total Bonus Payout by Region"
- Visualization Type: Clustered Column Chart; X-Axis: Region, Y-Axis: Total Pay
Table: "Individual Employee Bonus Payments"
- Visualization Type: Table; Columns: Name, Department, Region, Rating, Salary, Bonus Rate, Annual Bonus, Total Pay

### Images

Individual Employee Bonus Payment  
![Employee Bonus Payment](Individual%20Employee%20Bonus%20Payment%285%29.png)

Total Bonus Payout Per Region  
![Bonus Payout Per Region](Total%20Bonus%20Payout%20Per%20Region%285%29.png)

### Insight and Interpretation
Bonus amounts were calculated based on the Department and Performance Rating using a mapping table that was merged using a custom Dept_Rating key.

The bar chart displays how much each region is paying out in total bonuses, helping Palmora evaluate regional performance-based costs.

The table visual breaks down bonus information for each employee, fully transparent and helpful for audits or internal communication.

Using conditional logic, employees with better ratings received higher bonuses, encouraging a performance driven culture.

As stated earlier annual bonus was calculated using "[Salary] * [Bonus Rate]" while total pay is (Salary + Bonus): [Salary] + [Annual Bonus]. No measure was used

### Recommendation
Palmora should track bonus fairness by periodically reviewing the rating distribution to avoid bias and ensure equity in performance-based rewards.

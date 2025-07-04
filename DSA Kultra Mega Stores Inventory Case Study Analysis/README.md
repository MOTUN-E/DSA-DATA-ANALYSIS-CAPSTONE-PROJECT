# DSA SQL Project

## Project Overview
**Case Study:** Kultra Mega Stores Inventory (KMS)    

**Time Frame:** 2009–2012  

**Scope:** A SQL-based project analyzing to provide business intelligence on:
- Total sales by product category
- Profitability across segments
- Shipping cost patterns
- Customer and regional trends

**Tool:** SQL (SSMS)  

**Goal:** Help Kultra optimize inventory, profit, and shipping strategies

---

## Case Scenario I


### Q1. Which product category had the highest sales?
 Answer: Technology — ₦5,984,248.18  

 ### Insight & Interpretation: Technology performed best across all categories. This proves the fast rising and value of technology, so promoting this catergory may lead to more sales in the future.

### SQL Query used

 ```sql
SELECT 
    [Product_Category],
    FORMAT(SUM(Sales), 'C', 'en-NG') AS Total_Sales_Naira
FROM 
    [dbo].[ DSA_KMS_Sql_Case_Study_Project]
GROUP BY 
    [Product_Category]
ORDER BY 
    SUM(Sales) DESC;
```


### Q2. What are the Top 3 and Bottom 3 regions in terms of sales?

Answer: Top 3 Regions:
1. West — ₦3,597,549.27  
2. Ontario — ₦3,063,212.48  
3. Prarie — ₦2,837,304.61  

 Bottom 3 Regions:
1. Quebec — ₦1,510,195.09  
2. Northwest Territories — ₦800,847.33  
3. Nunavut — ₦116,376.48

###  Insight & Interpretation:

The West, Ontario, and Prarie regions are leading in total sales, contributing the highest revenue between 2009–2012. These regions likely have higher market penetration and consistent customer demand.

On the other hand, the bottom 3 regions may require market research to understand demand issues, delivery challenges, or pricing barriers.

### SQL Query used
```sql
SELECT 
    [Region],
    FORMAT(SUM(Sales), 'C', 'en-NG') AS Total_Sales_Naira
FROM 
    [dbo].[ DSA_KMS_Sql_Case_Study_Project]
GROUP BY 
    [Region]
ORDER BY 
    SUM(Sales) DESC;
```


### Q3. What were the total sales of all  appliances in Ontario?

Answer:  
Ontario recorded a total sales value of ₦3,063,212.48 between 2009 and 2012.

###  Insight & Interpretation:

Ontario is one of the highest-performing regions in terms of total sales, indicating a strong customer base and consistent demand. KMS should consider reinforcing its presence here by building long-term relationships with high-volume customers

This region may also serve as a benchmark for replicating sales strategies in other regions with lower performance.


### SQL Query used:

```sql
SELECT 
    [Region],
    FORMAT(SUM(Sales), 'C', 'en-NG') AS Total_Sales_Naira
FROM 
    [dbo].[ DSA_KMS_Sql_Case_Study_Project]
WHERE 
    [Region] = 'Ontario'
GROUP BY 
    [Region];
```


 ### Q4. Advise the management of KMS on what to do to increase the revenue from the bottom 10 customers

Answer: Bottom 10 Customers by Sales (2009–2012)

1. Jeremy Farry — ₦85.72  
2. Natalie DeCherney — ₦125.90  
3. Nicole Fjeld — ₦153.03  
4. Katrina Edelman — ₦180.76  
5. Dorothy Dickinson — ₦198.08  
6. Christine Kargatis — ₦293.22  
7. Eric Murdock — ₦343.33  
8. Chris McAfee — ₦350.18  
9. Rick Huthwaite — ₦415.82
10. 10. Mark Hamilton — ₦450.99  

###  Insight & Interpretation:

These customers contributed the least to overall revenue during the 4-year period, which suggests limited engagement or one-time purchases. KMS should offer offer incentives, such as free shipping on their next purchase or bundle complementary products to encourage slightly higher spending and also conduct a quick survey to understand why.

###  SQL Query Used:

```sql
SELECT 
    [Customer_Name],
    FORMAT(Total_Sales, 'C', 'en-NG') AS Total_Sales_Naira
FROM (
    SELECT 
        [Customer_Name],
        SUM(Sales) AS Total_Sales
    FROM 
        [dbo].[ DSA_KMS_Sql_Case_Study_Project]
    GROUP BY 
        [Customer_Name]
    ORDER BY 
        SUM(Sales) ASC
    OFFSET 0 ROWS
    FETCH NEXT 10 ROWS ONLY
) AS Bottom_Customers
ORDER BY 
    Total_Sales ASC;
```


### Q5. KMS incurred the most shipping cost using which shipping method?

 Answer:  
KMS incurred the highest shipping cost using Delivery Truck, with a total cost of ₦51,971.94 between 2009 and 2012.


###  Insight & Interpretation:

Delivery Truck is the most used and most economical shipping method per trip, it has accumulated the highest overall cost  possibly due to high volume usage or frequent rural deliveries.


### SQL Query Used:

```sql
SELECT 
    [Ship_Mode],
    FORMAT(SUM(Shipping_Cost), 'C', 'en-NG') AS Total_Shipping_Cost_Naira
FROM 
    [dbo].[ DSA_KMS_Sql_Case_Study_Project]
GROUP BY 
    [Ship_Mode]
ORDER BY 
    SUM(Shipping_Cost) DESC;
```


## Case Scenario II



### Q6. Who are the most valuable customers, and what products or services do they typically purchase?

Answer: 
Here are the top 10 highest-spending customers at KMS between 2009 and 2012, along with the product categories they purchased most:

| Customer Name       | Top Product Category | Category Sales (₦) |
|---------------------|----------------------|---------------------|
| Emily Phan          | Technology           | ₦110,481.97         |
| Deborah Brumfield   | Technology           | ₦76,795.79          |
| Roy Skaria          | Furniture            | ₦50,177.24          |
| Sylvia Foulston     | Furniture            | ₦48,173.38          |
| Grant Carroll       | Office Supplies      | ₦50,837.27          |
| Alejandro Grove     | Office Supplies      | ₦51,696.02          |
| Darren Budd         | Furniture            | ₦43,367.21          |
| Julia Barnett       | Furniture            | ₦46,359.63          |
| John Lucas          | Furniture            | ₦44,090.34          |
| Liz MacKendrick     | Technology           | ₦33,457.21          |


###  Insight & Interpretation:

The most valuable customers overwhelmingly favor Technology and Furniture, with a few leaning toward Office Supplies. This suggests corporate buyers purchasing tech & furniture for workspace setup and loyal customers buying across multiple product lines


### SQL Queries Used:

```sql
-- Query 1: Top 10 Most Valuable Customers
SELECT 
    [Customer_Name],
    FORMAT(SUM(Sales), 'C', 'en-NG') AS Total_Sales_Naira
FROM 
    [dbo].[ DSA_KMS_Sql_Case_Study_Project]
GROUP BY 
    [Customer_Name]
ORDER BY 
    SUM(Sales) DESC
OFFSET 0 ROWS
FETCH NEXT 10 ROWS ONLY;
```
```sql
-- Query 2: What Product Categories They Buy
SELECT 
    [Customer_Name],
    [Product_Category],
    FORMAT(SUM(Sales), 'C', 'en-NG') AS Category_Sales_Naira
FROM 
    [dbo].[ DSA_KMS_Sql_Case_Study_Project]
WHERE 
    [Customer_Name] IN (
        SELECT TOP 10 [Customer_Name]
        FROM [dbo].[ DSA_KMS_Sql_Case_Study_Project]
        GROUP BY [Customer_Name]
        ORDER BY SUM(Sales) DESC
    )
GROUP BY 
    [Customer_Name], [Product_Category]
ORDER BY 
    [Customer_Name], SUM(Sales) DESC;
```


### Q7. Which small business customer had the highest sales?

 Answer:  
The small business customer with the highest total sales between 2009 and 2012 was Emily Phan, with purchases totaling ₦117,124.44

###  Insight & Interpretation:

Emily Phan stands out as the top-performing customer in the Small Business segment, contributing significantly to revenue. This suggests a highly engaged and possibly repeat-buying customer with strong product demand, likely in high-value categories such as Technology or Furniture.

Her purchasing behavior may represent a valuable customer profile for KMS’s B2B small business strategy. Monitoring and replicating this engagement pattern with similar clients could help boost revenue across the segment.


###  SQL Query Used:

```sql
SELECT TOP 1
    [Customer_Name],
    FORMAT(SUM(Sales), 'C', 'en-NG') AS Total_Sales_Naira
FROM 
    [dbo].[ DSA_KMS_Sql_Case_Study_Project]
WHERE 
    [Customer_Segment] = 'Small Business'
GROUP BY 
    [Customer_Name]
ORDER BY 
    SUM(Sales) DESC;
```


### Q8. Which corporate customer placed the most number of orders in 2009–2012?

Answer:  
The corporate customer who placed the most orders between 2009 and 2012 was Adam Hart, with a total of 27 orders.

###  Insight & Interpretation:

Adam Hart stands out as the most consistently engaged customer in the Corporate segment by order frequency — even if his total spending might not be the highest.

### SQL Query Used:

```sql
SELECT TOP 1
    [Customer_Name],
    COUNT([Order_ID]) AS Number_of_Orders
FROM 
    [dbo].[ DSA_KMS_Sql_Case_Study_Project]
WHERE 
    [Customer_Segment] = 'Corporate'
GROUP BY 
    [Customer_Name]
ORDER BY 
    COUNT([Order_ID]) DESC;
```


### Q9. Which consumer customer was the most profitable?

Answer: 
The consumer customer who generated the highest total profit between 2009 and 2012 was Emily Phan, with a total profit of ₦34,005.44.

###  Insight & Interpretation:

Emily Phan not only ranked among the top customers by sales volume, but also delivered the highest profit within the Consumer segment.

This suggests:
- She consistently purchased high-margin products
- Possibly a high-frequency buyer of premium items (like Technology)
- Strong repeat purchasing behavior

Her profile indicates an ideal consumer buyer persona — loyal, profitable, and product-focused.

###  SQL Query Used:

```sql
SELECT TOP 1
    [Customer_Name],
    FORMAT(SUM(Profit), 'C', 'en-NG') AS Total_Profit_Naira
FROM 
    [dbo].[ DSA_KMS_Sql_Case_Study_Project]
WHERE 
    [Customer_Segment] = 'Consumer'
GROUP BY 
    [Customer_Name]
ORDER BY 
    SUM(Profit) DESC;
```


### Q10. Which customer returned items, and what segment do they belong to?

Answer:  
A total of returned orders were identified using the second table file sent, which contains return status by `Order_ID`.

Here are examples of customers who returned items and their segments:
- Aaron Bergman — Corporate
- Adam Bellavance — Small Business
- Alejandro Grove — Consumer, Corporate
- Anne Pryor — consumer
- Brian Stugart — Consumer, Corporate
- Julia West — Consumer, Home Office
- Roy Phan — Consumer, Corporate
  (...and many more)

Returns occurred across all four segments:
- Consumer  
- Corporate  
- Small Business  
- Home Office  

###  Insight & Interpretation:

Return behavior is distributed across all customer segments, not isolated to a specific type.
The Corporate and Small Business segments are especially prominent among returners, which may indicate differences in volume, product expectations, or usage needs.
Some customers appear in multiple segments, suggesting either:
   - Multi-channel purchasing behavior (e.g., same person ordering as both individual and business rep), or
   - Data consistency issues needing cleanup.

###  SQL Query Used:

```sql
SELECT 
    DISTINCT main.[Customer_Name],
    main.[Customer_Segment]
FROM 
    [dbo].[ DSA_KMS_Sql_Case_Study_Project] AS main
JOIN 
    [dbo].[DSA_Project_KMS_Order_Status_Table] AS status
    ON main.[Order_ID] = status.[Order_ID]
WHERE 
    status.[Status] = 'Returned';
```


### Q11. If the delivery truck is the most economical but the slowest shipping method and
Express Air is the fastest but the most expensive one, do you think the company
appropriately spent shipping costs based on the Order Priority? Explain your answer 

Answer Breakdown:

| Order Priority | Ship Mode       | Total Orders | Total Shipping Cost (₦) |
|---------------:|-----------------|-------------:|------------------------:|
| Critical       | Delivery Truck  |          228 | ₦10,783.82             |
| Critical       | Express Air     |          200 | ₦1,742.10              |
| Critical       | Regular Air     |        1,180 | ₦8,586.76              |
| High           | Delivery Truck  |          248 | ₦11,206.88             |
| High           | Express Air     |          212 | ₦1,453.53              |
| High           | Regular Air     |        1,308 | ₦10,005.01             |
| Medium         | Delivery Truck  |          205 | ₦9,461.62              |
| Medium         | Express Air     |          201 | ₦1,633.59              |
| Medium         | Regular Air     |        1,225 | ₦9,418.72              |
| Low            | Delivery Truck  |          250 | ₦11,131.61             |
| Low            | Express Air     |          190 | ₦1,551.63              |
| Low            | Regular Air     |        1,280 | ₦10,263.62             |
| Not Specified  | Delivery Truck  |          215 | ₦9,388.01              |
| Not Specified  | Express Air     |          180 | ₦1,470.06              |
| Not Specified  | Regular Air     |        1,277 | ₦9,734.08              |

###  Insight & Interpretation:

- Express Air is under-utilized for Critical (200/1,608 ≈ 12%) and High (212/1,768 ≈ 12%) orders, even though it’s the fastest method.
- Delivery Truck, while cheapest, is used almost equally across all priorities (~14–18%), rather than being reserved for Low priority orders.
- Regular Air dominates (~70–75%) in every category, indicating it’s the default regardless of urgency.

Conclusion:
Shipping method choice is not aligned with order urgency. KMS should:
1. Prioritize Express Air for Critical/High orders  
2. Use Delivery Truck for Low-priority shipments  
3. Reserve Regular Air for Medium-priority or overflow  

This alignment will optimize cost without sacrificing service levels.
### SQL Query Used:
```sql
SELECT 
    [Order_Priority],
    [Ship_Mode],
    COUNT(*)                AS Total_Orders,
    FORMAT(SUM([Shipping_Cost]), 'C', 'en-NG') AS Total_Shipping_Cost_Naira
FROM 
    [dbo].[ DSA_KMS_Sql_Case_Study_Project]
GROUP BY 
    [Order_Priority],
    [Ship_Mode]
ORDER BY 
    [Order_Priority],
    [Ship_Mode];
```


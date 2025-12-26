# SALES-ANALYSIS-WITH-SQL-
This project analyzes sales transaction data using SQL Server to uncover key business insights around customer behaviour,revenue trends and retention. 

The analysis focuses on ; 
.Identifying first-time vs repeat customers using cohort logic
.Tracking monthly customer acquisition and retention
. What's the 30-day moving average of orders?
.What's the 7-day moving average of revenue ?
.What's the revenue for the last 30 days Vs Previous days?
.What's the running total of orders over time?
.What's the cumulative revenue over time?
.What's the order distribution by day of week?
.Which day of the week generates the most revenue?
.What's the Day-Over-Day revenue growth?
.What's the daily revenue trend?
.What's the total revenue by quarter?
.What's the Quarter-Over-Quarter revenue growth?
.What's the Week-Over-Week revenue growth?
.What is the Month-over-month revenue growth?
.Find the most expensive single order per customer?
.What's the revenue contribution of each category?
.Which customers have placed orders in multiple location?
.Which customers have placed orders in multiple location?


```SQL  
SELECT *
FROM [Walmart Sales 2025];


----SALES ANALYSIS ---

---What is the total revenue from all orders?

SELECT SUM(Total_Sales ) AS Total_Revenue
FROM [Walmart Sales 2025];


---What's the average order value?

SELECT AVG(Total_Sales) AS Average_Order_Value
FROM [Walmart Sales 2025];

---How many orders were placed in total?

SELECT COUNT(*) AS Total_Orders
FROM [Walmart Sales 2025];

---What's the total revenue per category?

SELECT Category,
SUM(Total_Sales) AS Total_Revenue
FROM [Walmart Sales 2025]
GROUP BY Category
ORDER BY Total_Revenue DESC;


---What's the total revenue by product---

SELECT Product, SUM(Total_Sales) AS Total_Revenue
FROM [Walmart Sales 2025]
GROUP BY Product
ORDER BY Total_Revenue DESC;

---CUSTOMER ANALYSIS ---

---Who are the top 5 customers by total spending---
SELECT TOP 5 Customer_Name, SUM(Total_Sales) AS Total_Spent
FROM [Walmart Sales 2025]
GROUP BY Customer_Name
ORDER BY Total_Spent DESC;

---How many orders has each customer placed?

SELECT Customer_Name,
COUNT(*) AS Orders_Placed 
FROM [Walmart Sales 2025]
GROUP BY Customer_Name
ORDER BY Orders_Placed DESC;

---What's the average order value per customer?
SELECT Customer_Name,
AVG(Total_Sales) AS Average_Order_Value 
FROM [Walmart Sales 2025]
GROUP BY Customer_Name
ORDER BY Average_Order_Value DESC;


---PRODUCT ANALYSIS 

---Which products have been ordered more than 20 times?

SELECT Product,
COUNT(*) AS Order_Count
FROM [Walmart Sales 2025]
GROUP BY Product
HAVING COUNT(*) >20
ORDER BY Order_Count DESC;

----What's the total quantity sold by product?

SELECT Product,SUM(Quantity) AS Total_Quantity
FROM [Walmart Sales 2025]
GROUP BY Product
ORDER BY Total_Quantity DESC;

---What's the average price per product?

SELECT Product,
AVG(Price) AS Average_Price
FROM [Walmart Sales 2025]
GROUP BY Product
ORDER BY Average_Price DESC;

---What's the total revenue by location?

SELECT Customer_Location,
SUM(Total_Sales) AS Total_Revenue
FROM [Walmart Sales 2025]
GROUP BY Customer_Location
ORDER BY Total_Revenue DESC;

---Which locations have the most orders?

SELECT Customer_Location, COUNT(*) AS Order_Count
FROM [Walmart Sales 2025]
GROUP BY Customer_Location
ORDER BY Order_Count DESC;

---What's the average order value by location?

SELECT Customer_Location,
AVG(Total_Sales) AS Average_Order_Value
FROM [Walmart Sales 2025]
GROUP BY Customer_Location
ORDER BY Average_Order_Value DESC;

---PAYMENT METHOD ANALYSIS---
---What's the total revenue by payment method 

SELECT Payment_Method,
SUM(Total_Sales) AS Total_Revenue
FROM [Walmart Sales 2025] 
GROUP BY Payment_Method
ORDER BY Total_Revenue DESC;


---How many orders used each payment method---
SELECT Payment_Method,
COUNT(*) AS Order_Count
FROM [Walmart Sales 2025]
GROUP BY Payment_Method
ORDER BY Order_Count DESC;

----What's the most popular payment method 
SELECT TOP 1 Payment_Method,
COUNT(*) AS Usage_Count
FROM [Walmart Sales 2025]
GROUP BY Payment_Method
ORDER BY Usage_Count DESC;


---ORDER STATUS ANALYSIS---
---How many orders are in each status--
SELECT Status, COUNT(*) Order_Count
FROM [Walmart Sales 2025]
GROUP BY Status
ORDER BY Order_Count DESC;

---What is the total revenue by order status?
SELECT Status,
SUM(Total_Sales) AS Total_Revenue
FROM [Walmart Sales 2025]
GROUP BY Status
ORDER BY Total_Revenue DESC ; 

---What percentage of orders were completed Vs canceled?

SELECT Status,
COUNT(*) AS Order_Count,
CAST(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM [Walmart Sales 2025]) AS DECIMAL(5,1)) AS Order_Percentage
FROM [Walmart Sales 2025]
GROUP BY Status 

---TIME-BASED ANALYSIS
---What is the total revenue by month?

SELECT 
YEAR(Date) AS Year,
MONTH(Date) AS Month_Number,
DATENAME(MONTH,Date) AS Month_Name,
SUM(Total_Sales) AS Total_Revenue
FROM [Walmart Sales 2025]
GROUP BY YEAR(Date),MONTH(Date),DATENAME(MONTH,Date)
ORDER BY Year, Month_Number;


--- How many orders were placed each month?
SELECT DATENAME(MONTH,Date) Month_Name,
COUNT(*) AS Order_Count
FROM [Walmart Sales 2025]
GROUP BY DATENAME(MONTH,Date)
ORDER BY Order_Count DESC; 


---What is trend of order over time?
SELECT Date,
COUNT(*) AS Orders_Per_Day
FROM [Walmart Sales 2025]
GROUP BY Date
ORDER BY Date DESC;

---CATEGORY ANALYSIS 
---Which category generates most revenue
SELECT TOP 1 Category,
SUM(Total_Sales) AS Total_Revenue
FROM [Walmart Sales 2025]
GROUP BY Category
ORDER BY Total_Revenue DESC;

---What's the average order value by category?
SELECT Category,
AVG(Total_Sales) AS Avg_Order_Value
FROM [Walmart Sales 2025]
GROUP BY Category
ORDER BY Avg_Order_Value DESC;

---How many Products were sold in each category?
SELECT Category,
SUM(Quantity) AS Total_Quantity 
FROM [Walmart Sales 2025]
GROUP BY Category
ORDER BY Total_Quantity DESC;

---Find orders with above-average total sales

SELECT *
FROM [Walmart Sales 2025]
WHERE Total_Sales > (SELECT AVG(Total_Sales) FROM [Walmart Sales 2025])
ORDER BY Total_Sales DESC;

---Which customers have placed orders in multiple location?
SELECT Customer_Name,
COUNT(*) AS Order_Count,
COUNT(DISTINCT Customer_Location) AS Location_Count
FROM [Walmart Sales 2025]
GROUP BY Customer_Name
HAVING COUNT(DISTINCT Customer_Location) > 1; 

---What's the revenue contribution of each category?
SELECT Category,
SUM(Total_Sales) AS Category_Revenue,
CAST(SUM(Total_Sales) * 100.0 / (SELECT SUM(Total_Sales) FROM [Walmart Sales 2025] ) AS DECIMAL(5,1)) AS Revenue_Percentage
FROM [Walmart Sales 2025]
GROUP BY Category
ORDER BY Revenue_Percentage DESC;

---Find the most expensive single order per customer
SELECT Customer_Name,MAX(Total_Sales) AS Most_Expensive_Single_Order
FROM [Walmart Sales 2025]
GROUP BY Customer_Name
ORDER BY Most_Expensive_Single_Order DESC;

---MONTH-OVER-MONTH(MoM) ANALYSIS 
---What is the Month-over-month revenue growth?
WITH MonthlyRevenue AS(
SELECT
YEAR(Date) AS Year,
MONTH(Date) AS Month,
SUM(Total_Sales) AS Revenue
FROM [Walmart Sales 2025]
GROUP BY YEAR(Date), MONTH(Date)
)
SELECT 
Year,
Month,
Revenue AS Current_Month_Revenue,
LAG(Revenue) OVER (ORDER BY Year,Month) AS Previous_Month_Revenue,
Revenue - LAG(Revenue) OVER(ORDER BY Year,Month) AS MoM_Change,
CAST((Revenue - LAG(Revenue) OVER(ORDER BY Year,Month)) * 100.0 / NULLIF(LAG(Revenue) OVER(ORDER BY Year,Month),0) AS DECIMAL(5,1)) AS MoM_Growth_Percentage,
CASE 
    WHEN LAG(Revenue) OVER (ORDER BY Year,Month)  IS NULL THEN 'First Momth'
    WHEN Revenue > LAG(Revenue) OVER (ORDER BY Year,Month) THEN 'Revenue Increase'
    WHEN Revenue <  LAG(Revenue) OVER (ORDER BY Year,Month) THEN 'Revenue Decrease'
    ELSE 'No Change'
    END AS Revenue_Trend
FROM MonthlyRevenue
ORDER BY Year,Month;

---WEEK-OVER-WEEK(WoW) ANALYSIS
---What's the Week-Over-Week revenue growth?

WITH WeeklyRevenue  AS (
     SELECT
     YEAR(Date) AS Year,
     DATEPART(WEEK,Date) AS Week_Number,
     SUM(Total_Sales) AS Revenue
     FROM [Walmart Sales 2025]
     GROUP BY YEAR(Date), DATEPART(WEEK,Date)
     )
     SELECT 
     Year,
     Week_Number,
     Revenue AS Current_Week_Revenue,
     LAG(Revenue) OVER (ORDER BY Year, Week_Number) AS Previous_Week_Revenue,
     Revenue - LAG(Revenue) OVER (ORDER BY Year,Week_Number) AS WoW_Change,
     CAST((Revenue - LAG(Revenue) OVER (ORDER BY Year,Week_Number)) * 100.0 / NULLIF (LAG(Revenue) OVER (ORDER BY Year,Week_Number),0) AS DECIMAL(5,1)) AS WoW_Growth_Percentage,
       CASE 
       WHEN LAG(Revenue) OVER (ORDER BY Year,Week_Number) IS NULL THEN 'First Week'
       WHEN Revenue > LAG(Revenue) OVER (ORDER BY Year,Week_Number) THEN 'Revenue Increase'
       WHEN Revenue < LAG(Revenue) OVER (ORDER BY Year,Week_Number)  THEN 'Revenue Decrease'
       ELSE 'No Change'
       END AS Weekly_Trend
       FROM WeeklyRevenue
       ORDER BY Year, Week_Number ;


     ---QUARTER-OVER QUARTER (QoQ) ANALYSIS 
     ---What's the Quarter-Over-Quarter revenue growth?

      WITH QuarterlyRevenue AS (
      SELECT 
      YEAR(Date) AS Year,
      DATEPART(QUARTER,Date) AS Quarter,
      SUM(Total_Sales) AS Revenue
      FROM [Walmart Sales 2025]
      GROUP BY YEAR(Date), DATEPART(QUARTER,Date)
      )
      SELECT 
      Year,
      Quarter,
      Revenue AS Current_Quarter_Revenue,
      LAG(Revenue) OVER (ORDER BY Year,Quarter) AS Previous_Quarter_Revenue,
      Revenue - LAG(Revenue) OVER (ORDER BY Year,Quarter) AS QoQ_Change,
      CAST((Revenue- LAG(Revenue) OVER (ORDER BY Year,Quarter)) * 100.0 / NULLIF (LAG(Revenue) OVER (ORDER BY Year,Quarter),0) AS DECIMAL(5,2)) AS QoQ_Growth_Percentage,
      CASE
          WHEN LAG(Revenue) OVER (ORDER BY Year,Quarter) IS NULL THEN 'First Quarter'
          WHEN Revenue > LAG(Revenue) OVER (ORDER BY Year,Quarter) THEN 'Revenue Increase'
          WHEN Revenue < LAG (Revenue) OVER (ORDER BY Year,Quarter) THEN 'Revenue Decrease'
          ELSE 'No Change'
          END AS 'Quarterly_Trend'
          FROM QuarterlyRevenue
          ORDER BY Year , Quarter;

---What's the total revenue by quarter?
SELECT 
YEAR(Date) AS Year,
DATEPART(QUARTER,Date) AS Quarter,
SUM(Total_Sales) AS Revenue,
COUNT(*) AS Order_Count,
AVG(Total_Sales) AS Avg_Order_Value
FROM [Walmart Sales 2025]
GROUP BY YEAR(Date), DATEPART(QUARTER,Date)
ORDER BY Year,Quarter;

---What's the daily revenue trend?
SELECT
Date,
SUM(Total_Sales) AS Daily_Revenue_trend,
COUNT(*) AS Order_Count
FROM [Walmart Sales 2025]
GROUP BY Date
ORDER BY Date;

---What's the Day-Over-Day revenue growth?
WITH DailyRevenue AS (
SELECT
   Date,
   SUM(Total_Sales) AS Revenue
   FROM [Walmart Sales 2025]
   GROUP BY Date 
   )
   SELECT 
   Date,
   Revenue AS Current_Revenue,
   LAG(Revenue) OVER (ORDER BY Date) AS Previous_Day_Revenue,
   Revenue - LAG(Revenue) OVER (ORDER BY Date) AS DoD_Change,
   CASE 
       WHEN LAG(Revenue) OVER (ORDER BY Date) IS NULL THEN 'First Day'
       WHEN Revenue > LAG(Revenue) OVER (ORDER BY Date) THEN 'Revenue Increase'
       WHEN Revenue < LAG(Revenue) OVER (ORDER BY Date) THEN 'Revenue Decrease' 
       ELSE ' No Change ' 
       END AS Daily_Trend
   FROM DailyRevenue
   ORDER BY Date;

   ---DAY OF WEEK ANALYSIS 
 ---Which day of the week generates the most revenue?
 SELECT 
 DATENAME(WEEKDAY,Date) AS Day_Of_Week,
 SUM(Total_Sales) AS Revenue,
 COUNT(*) AS Order_Count,
 AVG(Total_Sales) AS Avg_Order_Value
 FROM [Walmart Sales 2025]
 GROUP BY DATENAME(WEEKDAY,Date), DATEPART(WEEKDAY,Date)
 ORDER BY DATEPART(WEEKDAY,Date);

 ---What's the order distribution by day of week?
 SELECT 
 DATENAME(WEEKDAY,Date) AS Day_Of_Week,
 COUNT(*) AS Order_Count,
 CAST(COUNT(*) * 100.0 / (SELECT COUNT(*)  FROM [Walmart Sales 2025]) AS DECIMAL (5,2)) AS Percentage
 FROM [Walmart Sales 2025]
 GROUP BY  DATENAME(WEEKDAY,Date), DATEPART(WEEKDAY,Date)
 ORDER BY DATEPART(WEEKDAY,Date);
 
 --- CUMULATIVE ANALYSIS 
 ---What's the cumulative revenue over time?
 SELECT
 Date,
 SUM(Total_Sales) AS Daily_Revenue,
 SUM(SUM(Total_Sales)) OVER (ORDER BY Date) AS Cumulative_Revenue
 FROM [Walmart Sales 2025]
 GROUP BY Date
 ORDER BY Date; 
  
  ---What's the running total of orders over time?
  SELECT 
  Date,
  COUNT(*) AS Daily_Orders,
  SUM(COUNT(*)) OVER (ORDER BY Date) AS Cumulative_Order
  FROM [Walmart Sales 2025]
  GROUP BY Date
  ORDER BY Date;

  ---What's the revenue for the last 30 days Vs Previous days?
  DECLARE @MaxDate DATE = (SELECT MAX(Date) FROM [Walmart Sales 2025])
  SELECT 
  CASE 
  WHEN Date >= DATEADD(DAY,-30, @MaxDate) THEN  'Last 30 days'
  WHEN Date >= DATEADD(DAY,-60,@MaxDate) AND Date < DATEADD(DAY,-30,@MaxDate) THEN 'Previous 30 Days'
  END AS Period,
  SUM(Total_Sales) AS Total_Revenue, 
  COUNT(*) AS Order_Count
  FROM [Walmart Sales 2025]
  WHERE Date > DATEADD(DAY,-60,@MaxDate) 
  GROUP BY 
  CASE 
  WHEN Date >= DATEADD(DAY,-30, @MaxDate) THEN  'Last 30 days'
  WHEN Date >= DATEADD(DAY,-60,@MaxDate) AND Date < DATEADD(DAY,-30,@MaxDate) THEN 'Previous 30 Days'
  END 


  DECLARE @Max_Date DATE = (SELECT MAX(Date) FROM [Walmart Sales 2025])
  
  SELECT 
  Customer_Name,
  MAX(Date) AS Last_Order_Date,
  DATEDIFF(DAY,MAX(Date),@Max_Date) AS Days_Since_Last_Order,
  SUM(Total_Sales) AS TOtal_Spent
  FROM [Walmart Sales 2025]
  GROUP BY Customer_Name
  HAVING MAX(Date) < DATEADD(DAY,-30, @Max_Date)
  ORDER BY TOtal_Spent DESC;
   
   ----What's the 7-day moving average of revenue ?
   WITH DailyRevenue AS (
   SELECT 
   Date,
   SUM(total_Sales) AS Revenue 
   FROM [Walmart Sales 2025]
   GROUP BY Date
   )
   SELECT 
   Date,
   Revenue,
   AVG(Revenue) OVER (ORDER BY Date  ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS Moving_Avg_7_Days
   FROM DailyRevenue
   ORDER BY Date;
  ---What's the 30-day moving average of orders?
  WITH DailyOrders AS (
  SELECT 
  Date,
  COUNT(*) AS Order_Count
  FROM [Walmart Sales 2025]
  GROUP BY Date
  )
  SELECT 
  Date,
  Order_Count,
  AVG(CAST(Order_Count AS float)) OVER (ORDER BY Date ROWS BETWEEN 29 PRECEDING AND CURRENT ROW) AS Moving_Avg_30_Days
  FROM DailyOrders
  ORDER BY Date;

  ----COHORT ANALYSIS 
  ---First-time vs repeat customer by month 
  WITH FirstOrder AS (
  SELECT 
  Customer_Name,
  MIN(Date) AS First_Order_Date
  FROM [Walmart Sales 2025]
  GROUP BY Customer_Name
  )
  SELECT
  YEAR(o.Date) AS Year,
  MONTH(o.Date) AS Month,
  DATENAME(MONTH,Date) AS Month_Name,
  SUM(CASE WHEN o.Date = fo.First_Order_Date THEN 1 ELSE 0 END) AS New_Customer,
  SUM(CASE WHEN o.Date > fo.First_Order_Date THEN  1 ELSE 0 END) AS Repeat_Customer,
  COUNT(*) AS Total_Orders 
  FROM [Walmart Sales 2025] o
  JOIN FirstOrder fo ON o.Customer_Name = fo.Customer_Name
  GROUP BY YEAR(o.Date), MONTH(o.Date),DATENAME(MONTH,Date)
  ORDER BY Year , Month;

  ---What are the top 5 revenue generating days?
  SELECT TOP 5 
  Date,
  SUM(Total_Sales) AS Daily_Revenue,
  COUNT(*) AS Order_Count
  FROM [Walmart Sales 2025]
  GROUP BY Date
  ORDER BY Daily_Revenue DESC;
  
  ---What's the best performing month historically?
  SELECT TOP 1
  DATENAME(MONTH, Date) AS Month_Name,
  SUM(Total_Sales) Total_Revenue
  FROM [Walmart Sales 2025]
  GROUP BY DATENAME(MONTH, Date)
  ORDER BY Total_Revenue DESC;

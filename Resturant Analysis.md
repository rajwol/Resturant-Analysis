```sql 
--Item Price Distribution
WITH PriceStats AS (SELECT TOP 1
PERCENTILE_CONT (0.25) within group (ORDER BY price) over() AS '25th Percentile',
PERCENTILE_CONT (0.5) within group (ORDER BY price) over() AS Median,
PERCENTILE_CONT (0.75) within group (ORDER BY price) over() AS '75th Percentile'
from menu_items)

SELECT 
    COUNT(DISTINCT menu_item_id) AS count,
    round(AVG(price),2) AS mean,
    MIN(price) AS min,
    MAX(price) AS max,
    (SELECT median FROM PriceStats) AS Median,
    STDEV(price) AS std_dev,
    (SELECT "25th Percentile" FROM PriceStats) AS "25th Percentile",
    (SELECT "75th Percentile" FROM PriceStats) AS "75th Percentile"
FROM 
    menu_items;
```
<img width="989" alt="B W plot" src="https://github.com/user-attachments/assets/e8941339-b911-4af9-be3b-e79cc2573584">

```sql
--Monthly performance
With Added_month_CTE AS (SELECT *,
CASE 
	WHEN order_date between '2023-01-01' and '2023-01-31' THEN 'january' 
	WHEN order_date between '2023-02-01' and '2023-02-28' THEN 'febuary'
	WHEN order_date between '2023-03-01' and '2023-03-31' THEN 'march'
	END AS Month
FROM order_details od
LEFT OUTER JOIN menu_items mi ON od.item_id = mi. menu_item_id)

SELECT 
	month,
	COUNT(DISTINCT order_id) AS customers,
	COUNT(order_id) AS items_served,
	SUM(price) AS revenue
FROM added_month_cte
GROUP BY month
ORDER BY 
	CASE month
	WHEN 'january' THEN 1
	WHEN 'febuary' THEN 2
	WHEN 'march' THEN 3 END
```
<img width="391" alt="MonthlyRevenueViz" src="https://github.com/user-attachments/assets/f6221375-27cc-4f54-9910-676a65f9262f">

```sql
-- Top 10 Most Frequently Ordered Dishes 
SELECT TOP 10 item_name, COUNT(item_name) AS orders
FROM order_details
LEFT OUTER JOIN menu_items ON order_details.item_id = menu_items.menu_item_id
GROUP BY item_name
ORDER BY orders DESC
```
<img width="872" alt="Top 10 Number of Orders Viz" src="https://github.com/user-attachments/assets/23cbac18-fafd-46d2-9afe-b61be18392bd">

```sql
--Top 10 Revenue-Generating Dishes
SELECT TOP 10 item_name, SUM(price) AS revenue
FROM order_details
LEFT OUTER JOIN menu_items ON order_details.item_id = menu_items.menu_item_id
GROUP BY item_name
ORDER BY revenue DESC
```
<img width="871" alt="Top 10 Revenue Generating Items Viz" src="https://github.com/user-attachments/assets/93a8d85e-b23e-4313-ae9e-9759f608b6f1">

```sql
--Orders & Revenue by Category
SELECT category, COUNT(category) AS orders, SUM(price) AS revenue
FROM order_details
LEFT OUTER JOIN menu_items ON order_details.item_id = menu_items.menu_item_id
WHERE category IS NOT NULL
GROUP BY category
ORDER BY revenue DESC, orders
```

```sql
--Customer traffic & items served by timeblock 
WITH Timeblock_CTE AS (SELECT *,
	CASE 
	WHEN order_time between '11:00:00 AM' and '11:59:59 AM' THEN '11AM'
	WHEN order_time between '12:00:00 PM' and '12:59:59 PM' THEN '12PM'
	WHEN order_time between '1:00:00 PM' and '1:59:59 PM' THEN '1PM'
	WHEN order_time between '2:00:00 PM' and '2:59:59 PM' THEN '2PM'
	WHEN order_time between '3:00:00 PM' and '3:59:59 PM' THEN '3PM'
	WHEN order_time between '4:00:00 PM' and '4:59:59 PM' THEN '4PM'
	WHEN order_time between '5:00:00 PM' and '5:59:59 PM' THEN '5PM'
	WHEN order_time between '6:00:00 PM' and '6:59:59 PM' THEN '6PM'
	WHEN order_time between '7:00:00 PM' and '7:59:59 PM' THEN '7PM'
	WHEN order_time between '8:00:00 PM' and '8:59:59 PM' THEN '8PM'
	WHEN order_time between '9:00:00 PM' and '9:59:59 PM' THEN '9PM'
	WHEN order_time between '10:00:00 PM' and '10:59:59 PM' THEN '10PM'
 END AS TimeBlock
FROM order_details)

SELECT TimeBlock, COUNT(DISTINCT order_id) AS customers, COUNT(*) AS items_served
FROM Timeblock_CTE
GROUP BY TimeBlock
ORDER BY TimeBlock
```
<img width="479" alt="Time Block Customers   Items Served " src="https://github.com/user-attachments/assets/b9c15372-1317-4e71-9d53-3d87e0fe0962">

```sql
-- Daily Revenue & Customer traffic 
WITH Dayofweek_CTE AS (SELECT *, DATENAME(WEEKDAY, order_date) AS DayOfWeek
FROM order_details 
LEFT OUTER JOIN menu_items on order_details.item_id = menu_items.menu_item_id)

SELECT DayOfWeek, SUM(price) AS revenue, COUNT(DISTINCT order_id) AS customers
FROM Dayofweek_CTE
GROUP BY DayOfWeek
ORDER BY 
    CASE DayOfWeek
        WHEN 'Sunday' THEN 1
        WHEN 'Monday' THEN 2
        WHEN 'Tuesday' THEN 3
        WHEN 'Wednesday' THEN 4
        WHEN 'Thursday' THEN 5
        WHEN 'Friday' THEN 6
        WHEN 'Saturday' THEN 7
    END;
```
<img width="509" alt="Revenue by Day Viz" src="https://github.com/user-attachments/assets/24269ada-6bde-48d5-9691-b808f9cc0550">
<img width="500" alt="Customer Traffic by Day Viz" src="https://github.com/user-attachments/assets/c867a01d-5101-4435-909d-0d6b34f51607">

```sql
--Average revenue per order
SELECT ROUND(AVG(order_revenue),2) AS average_revenue_per_order
FROM (
SELECT order_details.order_id, SUM(price) AS order_revenue
FROM order_details
LEFT OUTER JOIN menu_items 
ON order_details.item_id = menu_items.menu_item_id
GROUP BY order_id)
AS order_totals
```

```sql
--Customers Running Total
WITH RunningTotal_CTE AS(SELECT
  order_date,
  order_details_id,
  order_id,
  item_name,
  price,
  SUM(price) OVER (PARTITION BY order_id ORDER BY order_details_id) AS RunningTotal
FROM
  order_details
JOIN
  menu_items ON order_details.item_id = menu_items.menu_item_id)

SELECT order_details_id, MAX(RunningTotal) AS Total
FROM RunningTotal_CTE
GROUP BY order_details_id 
ORDER BY Total DESC
```



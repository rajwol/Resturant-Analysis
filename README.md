# Restaurant Data Analysis with SQL

## Project Overview
This project leverages SQL to analyze restaurant sales, customer traffic, and menu item performance, providing data-driven insights for management. By combining various SQL operations such as joins, aggregate functions, Common Table Expressions (CTEs), and time series analysis, this project delivers an actionable outline to support decision-making in areas like pricing strategy, inventory management, and targeted promotions.

The dataset includes transactional data on customer orders, menu items, and timestamps for each order. Through advanced SQL queries, we extract key metrics and patterns to help the restaurant management make informed strategic choices.

## SQL Techniques in Detail

- **Joins**: Multiple joins were used to merge order details with menu items, enabling detailed analysis of sales by item and category.
- **Aggregate Functions**: SUM, AVG, COUNT, MIN, and MAX functions allowed for efficient calculation of key metrics, from total revenue to average item prices.
- **CTEs**: Common Table Expressions were essential for organizing the analysis by month, day of the week, and time blocks, making the queries easier to read and manage.
- **CASE Statements**: CASE logic helped categorize data into custom labels, such as month names and time blocks, and conditionally classify data (e.g., by price range).
- **Time Series Analysis**: Grouping data by time dimensions (hourly, daily, and monthly) facilitated insights into customer traffic and revenue trends, vital for strategic planning.

## Analysis 

1. **Monthly Performance**: Analysis shows fluctuations in monthly revenue, with February having a slight drop compared to January and March. This seasonality suggests an opportunity for promotions during slower months.
   
2. **Top-Selling and High-Revenue Items**: The most ordered items (like Hamburgers and Edamame) differ from the highest revenue-generating items (like Korean Beef Bowl and Spaghetti & Meatballs). This insight suggests potential for upselling popular but lower-priced items.

3. **Category-Based Performance**: Italian and Asian cuisines are the top revenue-generating categories, indicating a strong customer preference. This insight supports expanding these categories and focusing on them in promotions.

4. **Customer Traffic and Peak Times**: The busiest time is during lunch (12 PM - 2 PM), with steady traffic into the evening. These insights allow for better staffing and inventory management during peak periods.

5. **Daily Revenue Patterns**: Monday has the highest revenue, while Wednesday sees the lowest. To address mid-week slumps, management could consider introducing Wednesday promotions to increase foot traffic.

6. **Item Price Distribution**: Most items are priced around the mean ($13.29), with a potential opportunity to add premium items above $20 for higher-end customers.

## Conclusion
This project demonstrates the power of SQL for business analysis, utilizing advanced techniques to transform raw transactional data into actionable insights. By understanding customer behavior, peak times, and category performance, the restaurant can make informed decisions to improve profitability, optimize staffing, and tailor promotions.

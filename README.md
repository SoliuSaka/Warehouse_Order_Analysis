# Warehouse Orders Analysis

## Table of Contents
1. [Project Overview](#project-overview)
2. [Objectives](#objectives)
3. [Data Source](#data-source)
4. [Tools Used](#tools-used)
5. [Methodology](#methodology)
6. [Results](#results)
7. [Conclusion](#conclusion)

## Project Overview
This project analyzes the `warehouse_orders` dataset in BigQuery, utilizing the `warehouse` and `orders` tables. The goal is to aggregate data to provide insights into warehouse performance based on the number of orders fulfilled.

## Objectives
The objective of this query is to create a table that includes:
- Each warehouse's ID, state, and alias.
- The number of orders fulfilled by each warehouse.
- The grand total of orders for all warehouses combined.
- A classification of each warehouse based on the percentage of grand total orders fulfilled: 
  - 0–20%
  - 21–60%
  - More than 60%

## Data Source
- **Dataset**: Warehouse Orders
- **Tables Used**: 
  - `warehouse` [click here to view](https://github.com/SoliuSaka/Warehouse_Order_Analysis/blob/main/Warehouse-Orders---Warehouse.csv)
  - `orders` [click here to view](https://github.com/SoliuSaka/Warehouse_Order_Analysis/blob/main/Warehouse-Orders---Orders.csv)
- **CSV File**: The cleaned dataset is available as a CSV file attachment.

## Tools Used
- **Excel**: Used for data cleaning to ensure accuracy and consistency before analysis.
- **SQL (BigQuery)**: Employed to analyze and interpret the data, allowing for effective aggregation and categorization of warehouse performance.

## Methodology
1. **Data Cleaning**:
   - Cleaned the dataset in Excel to remove any inconsistencies and prepare it for analysis.

2. **Data Analysis**:
   - Used SQL queries in BigQuery to aggregate the data according to the specified objectives.
   - Calculated the number of orders per warehouse and the grand total of orders.

   ```sql
   SELECT
     Warehouse.warehouse_id,
     CONCAT(Warehouse.state, ': ', Warehouse.warehouse_alias) AS warehouse_name,
     COUNT(Orders.order_id) AS number_of_orders,
     (SELECT COUNT(*) FROM graphic-transit-439221-e5.warehouse_orders.orders) AS total_orders,
     CASE
       WHEN COUNT(Orders.order_id) / (SELECT COUNT(*) FROM graphic-transit-439221-e5.warehouse_orders.orders) <= 0.20
       THEN 'Fulfilled 0-20% of Orders'
       WHEN COUNT(Orders.order_id) / (SELECT COUNT(*) FROM graphic-transit-439221-e5.warehouse_orders.orders) > 0.20
            AND COUNT(Orders.order_id) / (SELECT COUNT(*) FROM graphic-transit-439221-e5.warehouse_orders.orders) <= 0.60
       THEN 'Fulfilled 21-60% of Orders'
       ELSE 'Fulfilled more than 60% of Orders'
     END AS fulfillment_summary
   FROM graphic-transit-439221-e5.warehouse_orders.warehouse AS Warehouse
   LEFT JOIN graphic-transit-439221-e5.warehouse_orders.orders AS Orders
   ON Orders.warehouse_id = Warehouse.warehouse_id
   GROUP BY
     Warehouse.warehouse_id,
     warehouse_name
   HAVING
     COUNT(Orders.order_id) > 0;
   ```
## Results
   The final output includes a table with warehouse details, the number of orders, total orders, and fulfillment classification.
![Results Table](https://github.com/SoliuSaka/Warehouse_Order_Analysis/blob/main/Warehouse_analysis.jpg)

## Conclusion
The analysis of the warehouse_orders dataset provides valuable insights into warehouse performance based on order fulfillment. By categorizing each warehouse's fulfillment rates, stakeholders can identify areas for improvement and optimize operations to enhance overall efficiency and service levels.

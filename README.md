# Restaurant Orders Analysis SQL Project
A restaurant released a new menu in the starting of year, now they want to analyze the order data to understand sales trends, customer preferences, the most and least popular menu items, menu items which are doing well and more.

To help extract meaningful insights from their restaurant orders dataset, I will utilize SQL for data extraction and analysis. By performing a series of SQL queries, I will uncover key metrics and trends. This structured analysis will provide actionable insights to refine their menu, optimize inventory, and enhance overall customer satisfaction.

# Objectives 
- Explore the menu_items table to get an idea what is on the new menu.
- Explore the order_details table to get an idea of the data that has been collected.
- Use both tables to understend how customers are reacting to the new menu.

# Database Schema
The database consists of the following tables:
1. menu_items
- menu_item_id:	Unique ID of a menu item
- item_name:	Name of a menu item
- category:	Category or type of cuisine of the menu item
- price:	Price of the menu item (US Dollars $)

2. order_details
- order_details_id:	Unique ID of an item in an order
- order_id:	ID of an order
- order_date:	Date an order was put in (YYYY-MM-DD)
- order_time:	Time an order was put in (HH:MM:SS)
- item_id:	Matches the menu_item_id in the menu_items table

# menu_items table analysis
- View the menu_items table and write a query to find the number of items on the menu.<br/>
```
SELECT * FROM menu_items;
SELECT COUNT(menu_item_id) FROM menu_items;
```

- What are the least and most expensive items on the menu?<br/>
```
SELECT * FROM menu_items 
ORDER BY price LIMIT 1;
```
![1](https://github.com/shas87/sqlProject/assets/139848347/26dad6d0-e9f1-4b8b-8923-dabf4fae1a96)
```
SELECT * FROM menu_items
ORDER BY price DESC LIMIT 1;
```
![2](https://github.com/shas87/sqlProject/assets/139848347/e7ddcd6b-abb7-4f95-97f5-f82503b2d3d4)

- How many Italian dishes are on the menu? 
```
SELECT COUNT(*) FROM menu_items
WHERE category = 'Italian';
```
![3](https://github.com/shas87/sqlProject/assets/139848347/82b73cfc-c2a9-45e3-99ee-e9208c3da0c1)

- What are the least and most expensive Italian dishes on the menu?
```
SELECT * FROM menu_items
WHERE category = 'Italian'
ORDER BY price LIMIT 1;
```
![4](https://github.com/shas87/sqlProject/assets/139848347/1bc2c395-5514-4350-bea7-9ab8ba00c6c6)
```
SELECT * FROM menu_items
WHERE category = 'Italian'
ORDER BY price DESC LIMIT 1;
```
![5](https://github.com/shas87/sqlProject/assets/139848347/5b7472a1-1e66-44d3-a627-365d9af3ee46)

- How many dishes are in each category? 
```
SELECT category, COUNT(*)
FROM menu_items
GROUP BY category;
```
![6](https://github.com/shas87/sqlProject/assets/139848347/c00cc9aa-0918-4ab7-a4b2-bf85b7b80b8b)

- What is the average dish price within each category?
```
SELECT category, ROUND(AVG(price),2) AS avg_price 
FROM menu_items
GROUP BY category;
```
![7](https://github.com/shas87/sqlProject/assets/139848347/8b168770-eefd-4c23-bc3b-bab451676dd3)

# order_details table analysis
- View the order_details table. 
```
SELECT * FROM order_details;
```
![11](https://github.com/shas87/sqlProject/assets/139848347/fa79787a-6177-44e4-9db1-dc73b49dcb57)

- What is the date range of the table?
```
SELECT MIN(order_date) AS min_date, MAX(order_date) AS max_date
FROM order_details;
```
![12](https://github.com/shas87/sqlProject/assets/139848347/fe35dbd7-0a7e-46b4-8195-618c7ddb813f)

- How many orders were made within this date range? 
```
SELECT COUNT(DISTINCT(order_id)) AS no_of_orders FROM order_details;
```
![13](https://github.com/shas87/sqlProject/assets/139848347/99fe401e-2b50-4709-b47a-11a708330111)

- How many items were ordered within this date range?
```
SELECT COUNT(order_details_id) FROM order_details;
```
![14](https://github.com/shas87/sqlProject/assets/139848347/c68fd644-37d2-4078-9ffd-73b345ea9ed7)

- Which orders had the most number of items?
```
SELECT order_id, COUNT(item_id) AS no_of_items
FROM order_details
GROUP BY order_id
ORDER BY no_of_items DESC;
```
![15](https://github.com/shas87/sqlProject/assets/139848347/02678bab-c679-4d60-9d45-244bd3dd4d02)

- How many orders had more than 12 items?
```
WITH order_by_count AS
(SELECT order_id, COUNT(item_id) AS no_of_items
FROM order_details
GROUP BY order_id
HAVING no_of_items > 12
ORDER BY no_of_items DESC) 

SELECT COUNT(*) AS num_orders FROM order_by_count;
```
![16](https://github.com/shas87/sqlProject/assets/139848347/e2f75d87-52b2-4967-9849-478a89d4e125)

# Customer Analysis
- Combine the menu_items and order_details tables into a single table
```
SELECT * FROM order_details 
LEFT JOIN menu_items
ON menu_items.menu_item_id = order_details.item_id;
```
![20](https://github.com/shas87/sqlProject/assets/139848347/e2ff312e-4e09-4f49-83b4-27ec3abbdec5)

- What were the least and most ordered items? What categories were they in?
```
SELECT item_name, category, COUNT(*) AS num FROM order_details 
INNER JOIN menu_items
ON menu_items.menu_item_id = order_details.item_id
GROUP BY item_name, category
ORDER BY num;
```
![21](https://github.com/shas87/sqlProject/assets/139848347/a6f07a16-3914-4887-bf7e-eb89dd53cd44)

- What were the top 5 orders that spent the most money?
```
SELECT order_id, ROUND(SUM(price),2) AS order_wise_spend 
FROM order_details 
INNER JOIN menu_items
ON menu_items.menu_item_id = order_details.item_id
GROUP BY order_id
ORDER BY order_wise_spend DESC LIMIT 5;
```
![22](https://github.com/shas87/sqlProject/assets/139848347/e0a4e400-4471-4094-91df-e82518c78ad4)

- View the details of the highest spend order. Which specific items were purchased?
```
SELECT category, COUNT(item_id) FROM order_details 
LEFT JOIN menu_items
ON menu_items.menu_item_id = order_details.item_id
WHERE order_id = 440
GROUP BY category;
```
![23](https://github.com/shas87/sqlProject/assets/139848347/86f8426a-9137-4e72-806b-78593cff1b37)

- View the details of the top 5 highest spend orders
```
SELECT order_id, category, COUNT(item_id) FROM order_details 
LEFT JOIN menu_items
ON menu_items.menu_item_id = order_details.item_id
WHERE order_id IN(440, 2075, 1957, 330, 2675)
GROUP BY order_id, category;
```
![24](https://github.com/shas87/sqlProject/assets/139848347/7a50b9bb-7b90-478f-a412-1554c4606006)

# Insights
- Hamburger is the most ordered item from the restaraunt menu.
- Chicken Tacos is the least ordered item from the restaraunt menu.
- Menu Items from the Italian category are the most in demand, among the individuals who are spending higher.
  

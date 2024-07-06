# Restaurant Orders Analysis SQL Project
A restaurant released a new menu in the starting of year, now they want to analyze the order data to identify the most and least popular menu items and which menu items are doing well.

Tables : menu_items, order_details

# Objectives 
- Explore the menu_items table to get an idea what is on the new menu.
- Explore the order_details table to get an idea of the data that has been collected.
- Use both tables to understend how customers are reacting to the new menu.

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
```
SELECT * FROM menu_items
ORDER BY price DESC LIMIT 1;
```

- How many Italian dishes are on the menu? 
```
SELECT COUNT(*) FROM menu_items
WHERE category = 'Italian';
```

- What are the least and most expensive Italian dishes on the menu?
```
SELECT * FROM menu_items
WHERE category = 'Italian'
ORDER BY price LIMIT 1;
```
```
SELECT * FROM menu_items
WHERE category = 'Italian'
ORDER BY price DESC LIMIT 1;
```

- How many dishes are in each category? 
```
SELECT category, COUNT(*)
FROM menu_items
GROUP BY category;
```
# order_details table analysis
- View the order_details table. 
```
SELECT * FROM order_details;
```

- What is the date range of the table?
```
SELECT MIN(order_date) AS min_date, MAX(order_date) AS max_date
FROM order_details;
```

- How many orders were made within this date range? 
```
SELECT COUNT(DISTINCT(order_id)) AS no_of_orders FROM order_details;
```

- How many items were ordered within this date range?
```
SELECT COUNT(order_details_id) FROM order_details;
```

- Which orders had the most number of items?
```
SELECT order_id, COUNT(item_id) AS no_of_items
FROM order_details
GROUP BY order_id
ORDER BY no_of_items DESC;
```

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
# Customer Analysis
- Combine the menu_items and order_details tables into a single table
```
SELECT * FROM order_details 
LEFT JOIN menu_items
ON menu_items.menu_item_id = order_details.item_id;
```
- What were the least and most ordered items? What categories were they in?
```
SELECT item_name, category, COUNT(*) AS num FROM order_details 
INNER JOIN menu_items
ON menu_items.menu_item_id = order_details.item_id
GROUP BY item_name, category
ORDER BY num;
```

- What were the top 5 orders that spent the most money?
```
SELECT order_id, ROUND(SUM(price),2) AS order_wise_spend 
FROM order_details 
INNER JOIN menu_items
ON menu_items.menu_item_id = order_details.item_id
GROUP BY order_id
ORDER BY order_wise_spend DESC LIMIT 5;
```

- View the details of the highest spend order. Which specific items were purchased?
```
SELECT category, COUNT(item_id) FROM order_details 
LEFT JOIN menu_items
ON menu_items.menu_item_id = order_details.item_id
WHERE order_id = 440
GROUP BY category;
```

- View the details of the top 5 highest spend orders
```
SELECT order_id, category, COUNT(item_id) FROM order_details 
LEFT JOIN menu_items
ON menu_items.menu_item_id = order_details.item_id
WHERE order_id IN(440, 2075, 1957, 330, 2675)
GROUP BY order_id, category;
```

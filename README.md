# PIZZA SALES ANALYSIS WITH SQL

## PROJECT OVERVIEW
This project involves creating a comprehensive database management system to track and analyze pizza sales. The project includes SQL scripts, CSV data files, and detailed analysis questions related to the pizza sales data.

---

## Database Details & SQL Queries

### 1. Retrieve the total number of orders placed
```sql
SELECT COUNT(order_id) AS total_orders FROM orders;
```
**Result:**
| total_orders |
|-------------|
| 21350         |

### 2. Calculate the total revenue generated from pizza sales
```sql
SELECT ROUND(SUM(order_details.quantity * pizzas.price), 2) AS total_sales
FROM order_details JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id;
```
**Result:**
| total_sales |
|------------|
| 817860.05    |

### 3. Identify the highest-priced pizza
```sql
SELECT pizza_types.name, pizzas.price
FROM pizza_types JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
ORDER BY pizzas.price DESC LIMIT 1;
```
**Result:**
| name       | price  |
|-----------|--------|
| The Greek Pizza | 35.95  |

### 4. Identify the most common pizza size ordered
```sql
SELECT pizzas.size, COUNT(order_details.order_details_id) AS order_count
FROM pizzas JOIN order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizzas.size ORDER BY order_count DESC;
```
**Result:**
| size  | order_count |
|-------|------------|
| L| 18526       |
| M | 18526 |
| S | 18526 |
| XL | 544 |
| XXL | 28 |
### 5. List the top 5 most ordered pizza types along with their quantities
```sql
SELECT pizza_types.name, SUM(order_details.quantity) AS quantity
FROM pizza_types JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name ORDER BY quantity DESC LIMIT 5;
```
**Result:**
| name        | quantity |
|------------|----------|
| The Classic Deluxe Pizza    | 2453      |
| The Barbecue Chicken Pizza    | 2432     |
| The Hawaiian Pizza | 2422 |
| The Pepperoni Pizza | 2418|
| The Thai Chicken Pizza | 2371 |

### 6. Total quantity of each pizza category ordered
```sql
SELECT pizza_types.category, SUM(order_details.quantity) AS quantity
FROM pizzas JOIN pizza_types ON pizzas.pizza_type_id = pizza_types.pizza_type_id
JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category ORDER BY quantity DESC;
```
**Result:**
| category | quantity |
|----------|----------|
| Classic  | XXX      |

### 7. Distribution of orders by hour of the day
```sql
SELECT HOUR(order_time) AS hour, COUNT(order_id) AS order_count
FROM orders GROUP BY hour;
```
**Result:**
| hour | order_count |
|------|------------|
| 12   | XXX        |

### 8. Category-wise distribution of pizzas
```sql
SELECT category, COUNT(name) AS count FROM pizza_types GROUP BY category;
```
**Result:**
| category | count |
|----------|-------|
| Classic  | XXX   |

### 9. Average number of pizzas ordered per day
```sql
SELECT ROUND(AVG(quantity), 0) AS average_pizzas_ordered_per_day
FROM (SELECT DATE(orders.order_date) AS date, SUM(order_details.quantity) AS quantity
FROM orders JOIN order_details ON orders.order_id = order_details.order_id
GROUP BY date) AS order_quantity;
```
**Result:**
| average_pizzas_ordered_per_day |
|--------------------------------|
| XXX                            |

### 10. Top 3 most ordered pizza types based on revenue
```sql
SELECT pizza_types.name, SUM(order_details.quantity * pizzas.price) AS revenue
FROM pizza_types JOIN pizzas ON pizzas.pizza_type_id = pizza_types.pizza_type_id
JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name ORDER BY revenue DESC LIMIT 3;
```
**Result:**
| name       | revenue |
|-----------|---------|
| Pizza XYZ | XXX.XX  |

### 11. Percentage contribution of each pizza type to total revenue
```sql
SELECT pizza_types.category, ROUND(SUM(order_details.quantity * pizzas.price) /
(SELECT ROUND(SUM(order_details.quantity * pizzas.price), 2) AS total_sales FROM order_details
JOIN pizzas ON pizzas.pizza_id = order_details.pizza_id) * 100, 2) AS revenue
FROM pizza_types JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category ORDER BY revenue DESC;
```
**Result:**
| category | revenue (%) |
|----------|------------|
| Classic  | XX.XX      |

### 12. Cumulative revenue generated over time
```sql
SELECT order_date, SUM(revenue) OVER (ORDER BY order_date) AS cum_revenue
FROM (SELECT orders.order_date, SUM(order_details.quantity * pizzas.price) AS revenue
FROM order_details JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id
JOIN orders ON order_details.order_id = orders.order_id
GROUP BY orders.order_date) AS sales;
```
**Result:**
| order_date | cum_revenue |
|------------|------------|
| YYYY-MM-DD | XXX.XX      |

### 13. Top 3 most ordered pizza types based on revenue for each category
```sql
SELECT category, name, revenue FROM (
SELECT category, name, revenue, RANK() OVER (PARTITION BY category ORDER BY revenue DESC) AS rn
FROM (SELECT pizza_types.category, pizza_types.name,
SUM(order_details.quantity * pizzas.price) AS revenue FROM pizza_types
JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category, pizza_types.name) AS a) AS b
WHERE rn <= 3;
```
**Result:**
| category | name        | revenue |
|----------|------------|---------|
| Classic  | Pizza ABC  | XXX.XX  |

---

## THANK YOU!
This analysis provides insights into pizza sales, order distribution, revenue trends, and customer preferences.



# 🍕 Pizza Sales Analysis – SQL Project

## 📌 Project Overview

**Project Title**: Pizza Sales Analysis  
**Level**: Beginner – Intermediate  
**Database**: `pizza_sales_db`

This project demonstrates the use of **SQL for real-world business analysis** using a pizza delivery business dataset. The analysis covers data exploration, performance metrics, customer behavior, time-based patterns, and advanced revenue insights. It is ideal for aspiring data analysts to practice structured querying and derive business insights using SQL.

---

## 🎯 Objectives

- **Data Understanding**: Explore orders, pizza details, and customer preferences.
- **Business Metrics**: Calculate revenue, top-selling pizzas, size preferences.
- **Time-based Analysis**: Examine order distribution by time and date.
- **Revenue Breakdown**: Understand performance by category and product.
- **Advanced Insights**: Use window functions for rankings and cumulative metrics.

---

## 🗂 Project Structure

### 1. Database Setup

The database contains four tables:

- `orders`: Contains order-level data like `order_id`, `order_date`, `order_time`.
- `orders_details`: Links orders to pizzas and includes `quantity`.
- `pizzas`: Contains pizza-level data like `pizza_id`, `size`, and `price`.
- `pizza_types`: Includes metadata like `pizza_type_id`, `name`, and `category`.

---

### 2. Data Analysis & Business Queries

#### 🔢 Basic Metrics

- **Retrieve total number of orders placed**:
```sql
SELECT COUNT(order_id) AS total_orders FROM orders;
```

- **Calculate total revenue from pizza sales**:
```sql
SELECT 
  ROUND(SUM(orders_details.quantity * pizzas.price), 2) AS total_sales
FROM orders_details
JOIN pizzas ON pizzas.pizza_id = orders_details.pizza_id;
```

- **Identify the highest-priced pizza**:
```sql
SELECT pizza_types.name, pizzas.price
FROM pizza_types
JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
ORDER BY pizzas.price DESC
LIMIT 1;
```

- **Most commonly ordered pizza size**:
```sql
SELECT pizzas.size, COUNT(orders_details.order_details_id) AS order_count
FROM pizzas
JOIN orders_details ON pizzas.pizza_id = orders_details.pizza_id
GROUP BY pizzas.size
ORDER BY order_count DESC
LIMIT 1;
```

- **Top 5 most ordered pizza types**:
```sql
SELECT pizza_types.name, SUM(orders_details.quantity) AS quantity
FROM pizza_types
JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN orders_details ON orders_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY quantity DESC
LIMIT 5;
```

---

#### 📊 Intermediate Analysis

- **Total quantity sold per pizza category**:
```sql
SELECT pizza_types.category, SUM(orders_details.quantity) AS quantity
FROM pizza_types
JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN orders_details ON orders_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category
ORDER BY quantity DESC;
```

- **Order distribution by hour**:
```sql
SELECT HOUR(order_time) AS order_hour, COUNT(order_id) AS order_count
FROM orders
GROUP BY HOUR(order_time);
```

- **Pizza count by category**:
```sql
SELECT category, COUNT(name) AS pizza_count
FROM pizza_types
GROUP BY category;
```

- **Average number of pizzas ordered per day**:
```sql
SELECT ROUND(AVG(quantity), 0) AS avg_pizza_ordered_per_day
FROM (
  SELECT orders.order_date, SUM(orders_details.quantity) AS quantity
  FROM orders
  JOIN orders_details ON orders.order_id = orders_details.order_id
  GROUP BY orders.order_date
) AS daily_totals;
```

---

#### 🧠 Advanced Insights

- **Top 3 most ordered pizza types by revenue**:
```sql
SELECT pizza_types.name, SUM(orders_details.quantity * pizzas.price) AS revenue
FROM pizza_types
JOIN pizzas ON pizzas.pizza_type_id = pizza_types.pizza_type_id
JOIN orders_details ON orders_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY revenue DESC
LIMIT 3;
```

- **Percentage contribution of each pizza category to total revenue**:
```sql
SELECT 
  pizza_types.category,
  (SUM(orders_details.quantity * pizzas.price) / 
    (SELECT SUM(orders_details.quantity * pizzas.price)
     FROM orders_details
     JOIN pizzas ON pizzas.pizza_id = orders_details.pizza_id)) * 100 AS revenue_percentage
FROM pizza_types
JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN orders_details ON orders_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category
ORDER BY revenue_percentage DESC;
```

- **Cumulative revenue generated over time**:
```sql
SELECT order_date,
       SUM(revenue) OVER (ORDER BY order_date) AS cum_revenue
FROM (
  SELECT orders.order_date,
         SUM(orders_details.quantity * pizzas.price) AS revenue
  FROM orders_details
  JOIN pizzas ON orders_details.pizza_id = pizzas.pizza_id
  JOIN orders ON orders.order_id = orders_details.order_id
  GROUP BY orders.order_date
) AS daily_revenue;
```

- **Top 3 pizza types by revenue in each category**:
```sql
SELECT category, name, revenue,
       RANK() OVER (PARTITION BY category ORDER BY revenue DESC) AS rank
FROM (
  SELECT pizza_types.category, pizza_types.name,
         SUM(orders_details.quantity * pizzas.price) AS revenue
  FROM pizza_types
  JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
  JOIN orders_details ON orders_details.pizza_id = pizzas.pizza_id
  GROUP BY pizza_types.category, pizza_types.name
) AS ranked_revenue;
```

---

## 📈 Findings

- **Revenue Drivers**: Certain pizza types generate significantly more revenue.
- **Size Preference**: A particular size dominates in terms of order volume.
- **Category Insights**: Categories like 'Classic' and 'Veggie' are high performers.
- **Time Patterns**: Orders peak during specific hours of the day.
- **Top Customers**: Pizza types with higher prices are ordered less frequently but contribute more revenue.

---

## 🧪 How to Use

1. **Clone the Repository**:  
   Clone this project repo from GitHub.

2. **Set Up the Database**:  
   Use the provided SQL schema and CSVs to populate the tables.

3. **Run Queries**:  
   Run queries in your preferred SQL environment (MySQL/PostgreSQL).

4. **Explore & Modify**:  
   Customize the queries or add more to suit different analysis goals.

---

## 👨‍💻 Author

**Rahul Jangra**  
This project is part of my data analyst portfolio to showcase SQL query writing, data storytelling, and analytical reasoning.  
Feel free to connect for feedback, collaborations, or project discussions.

<h1>#2.A - Pizza Runner🍕🚴</h1>
<img width="400" alt="Coding" src="https://github.com/ShreyasAnalyst/-2---Pizza-Runner/blob/main/images/pizza%20runner%20a.png">
<h1>Contents</h1>
<ul>
  <li><a href="#introduction">Introduction</a></li>
  <li><a href="#available-data">Available Data</a></li>
  <li><a href="#entity-relationship">Entity Relationship Diagram</a></li>
  <li><a href="#case-study">Case Study Questions & Solutions</a></li>
  <li><a href="#Notes">Notes</a></li>
  <li><a href="#insights">Key Insights</a></li>
  <li><a href="#Suggestions">Suggestions</a></li>
</ul>

<h1><a id="introduction">Introduction</a></h1>
<p>Did you know that over 115 million kilograms of pizza is consumed daily worldwide??? (Well according to Wikipedia anyway…)</p>
<p>Danny was scrolling through his Instagram feed when something really caught his eye - “80s Retro Styling and Pizza Is The Future!”</p>
<p>Danny was sold on the idea, but he knew that pizza alone was not going to help him get seed funding to expand his new Pizza Empire - so he had one more genius idea to combine with it - he was going to Uberize it - and so Pizza Runner was launched!</p>
<p>Danny started by recruiting “runners” to deliver fresh pizza from Pizza Runner Headquarters (otherwise known as Danny’s house) and also maxed out his credit card to pay freelance developers to build a mobile app to accept orders from customers.</p>

<h1><a id="available-data">Available Data</a></h1>
<p>Because Danny had a few years of experience as a data scientist - he was very aware that data collection was going to be critical for his business’ growth.</p>
<p>He has prepared for us an entity relationship diagram of his database design but requires further assistance to clean his data and apply some basic calculations so he can better direct his runners and optimise Pizza Runner’s operations.</p>
<p>All datasets exist within the pizza_runner database schema - be sure to include this reference within your SQL scripts as you start exploring the data and answering the case study questions.</p>

<h1><a id="entity-relationship">Entity Relationship Diagram</a></h1>
<img width="400" alt="Coding" src="https://github.com/ShreyasAnalyst/-2---Pizza-Runner/blob/main/images/entity%20diagram.png">

<h1><a id="case-study">Case Study Questions & Solutions</a></h1>

A. Pizza Metrics

## 1. How many pizzas were ordered?

**SQL Code:**
```sql
SELECT
    COUNT(pizza_id) as total_pizza_ordered
FROM
    customer_orders;
```
<h6>Output</h6>

<img width="400" alt="Coding" src="https://github.com/ShreyasAnalyst/-2---Pizza-Runner/blob/main/images/1.png">

## 2. How many unique customer orders were made?

**SQL Code:**
```sql
SELECT 
    COUNT(DISTINCT order_id) unique_orders
FROM
    customer_orders;
```
<h6>Output</h6>

<img width="400" alt="Coding" src="https://github.com/ShreyasAnalyst/-2---Pizza-Runner/blob/main/images/2.png">

## 3. How many successful orders were delivered by each runner?

**SQL Code:**
```sql
SELECT 
    runner_id,
    COUNT(order_id) total_deliveries
FROM 
    runner_orders
WHERE
    duration <> 'null'
GROUP BY 1
ORDER BY 2 DESC;
```
<h6>Output</h6>

<img width="400" alt="Coding" src="https://github.com/ShreyasAnalyst/-2---Pizza-Runner/blob/main/images/3.png">

## 4. How many of each type of pizza was delivered?

**SQL Code:**
```sql
SELECT 
    pn.pizza_name,
    COUNT(co.pizza_id) delivered_pizzas
FROM 
    runner_orders ro
    JOIN customer_orders co ON ro.order_id = co.order_id
    JOIN pizza_names pn ON co.pizza_id = pn.pizza_id
WHERE
    ro.pickup_time <> 'null'
GROUP BY 1;
```
<h6>Output</h6>

<img width="400" alt="Coding" src="https://github.com/ShreyasAnalyst/-2---Pizza-Runner/blob/main/images/4.png">

## 5. How many Vegetarian and Meatlovers were ordered by each customer?

**SQL Code:**
```sql
SELECT 
    co.customer_id,
    pn.pizza_name,
    COUNT(co.pizza_id) ordered_pizzas
FROM
    customer_orders co
    JOIN pizza_names pn ON co.pizza_id = pn.pizza_id
GROUP BY 1,2
ORDER BY 1;
```
<h6>Output</h6>

<img width="400" alt="Coding" src="https://github.com/ShreyasAnalyst/-2---Pizza-Runner/blob/main/images/5.png">

## 6. What was the maximum number of pizzas delivered in a single order?

**SQL Code:**
```sql
SELECT 
    co.order_id,
    COUNT(co.order_id) pizzas_order
FROM
    customer_orders co
    JOIN runner_orders ro ON co.order_id = ro.order_id
WHERE 
    pickup_time <> 'null'
GROUP BY 1
ORDER BY 2 DESC
LIMIT 1;
```
<h6>Output</h6>

<img width="400" alt="Coding" src="https://github.com/ShreyasAnalyst/-2---Pizza-Runner/blob/main/images/6.png">

## 7. For each customer, how many delivered pizzas had at least 1 change and how many had no changes?

**SQL Code:**
```sql
SELECT 
    customer_id,
    SUM(
        CASE 
            WHEN 
                (LENGTH(exclusions) > 0 AND exclusions <> 'null' AND exclusions IS NOT NULL) OR 
                (LENGTH(extras) > 0 AND extras <> 'null' AND extras IS NOT NULL) = 'true'
            THEN 1
            ELSE 0
        END) pizzas_with_changes,
    SUM(
        CASE 
            WHEN 
                (LENGTH(exclusions) > 0 AND exclusions <> 'null' AND exclusions IS NOT NULL) OR 
                (LENGTH(extras) > 0 AND extras <> 'null' AND extras IS NOT NULL) = 'true'
            THEN 0
            ELSE 1
        END) pizzas_with_no_changes
FROM 
    customer_orders co
    JOIN runner_orders ro ON co.order_id = ro.order_id
WHERE 
    pickup_time <> 'null'
GROUP BY 1;
```
<h6>Output</h6>

<img width="400" alt="Coding" src="https://github.com/ShreyasAnalyst/-2---Pizza-Runner/blob/main/images/7.png">

## 8. How many pizzas were delivered that had both exclusions and extras?

**SQL Code:**
```sql
SELECT 
    SUM(
        CASE 
            WHEN 
                (LENGTH(exclusions) > 0 AND exclusions <> 'null' AND exclusions IS NOT NULL) AND 
                (LENGTH(extras) > 0 AND extras <> 'null' AND extras IS NOT NULL) = 'true'
            THEN 1
            ELSE 0
        END) pizzas_delivered_with_exclusions_extras
FROM 
    customer_orders co
    JOIN runner_orders ro ON co.order_id = ro.order_id
WHERE 
    pickup_time <> 'null';
```
<h6>Output</h6>

<img width="400" alt="Coding" src="https://github.com/ShreyasAnalyst/-2---Pizza-Runner/blob/main/images/8.png">

## 9. What was the total volume of pizzas ordered for each hour of the day?

**SQL Code:**
```sql
SELECT 
    EXTRACT(HOUR FROM order_time :: timestamp) AS hours,
    COUNT(pizza_id)
FROM 
    customer_orders
GROUP BY 1
ORDER BY 1;
```
<h6>Output</h6>

<img width="400" alt="Coding" src="https://github.com/ShreyasAnalyst/-2---Pizza-Runner/blob/main/images/9.png">

## 10. What was the volume of orders for each day of the week?

**SQL Code:**
```sql
SELECT 
    TO_CHAR(order_time::timestamp, 'Day') AS day_name,
    COUNT(order_id)
FROM 
    customer_orders
GROUP BY 1
ORDER BY 1;
```
<h6>Output</h6>
<img width="400" alt="Coding" src="https://github.com/ShreyasAnalyst/-2---Pizza-Runner/blob/main/images/10.png">


<h1><a id="Notes">Notes</a></h1>

- **EXTRACT function:** The EXTRACT function is used to extract specific components (e.g., year, month, day, hour, minute) from a date or timestamp.

- **TO_CHAR function:** The TO_CHAR function in PostgreSQL is used for formatting date and time values into specific string representations. It allows you to convert a date, timestamp, or interval value into a string in a format of your choice.

<h1><a id="insights">Key Insights</a></h1>

1. The total number of pizzas ordered is 14.
2. A total of 8 orders were delivered, and 2 were canceled.
3. The runner with ID 1 has delivered the maximum number of orders, i.e., 4.
4. Non-vegetarian pizzas are in high demand and have been ordered the most.
5. Order No. 4 has the most pizzas, i.e., 3.
6. More than half of customers (3) want pizzas with changes, and the rest (2) want pizzas with no changes.
7. Most pizzas are ordered between 1 PM and 6 PM and 9 PM and 11 PM. That is the afternoon and the late evening.
8. Most customers order on Saturdays and Wednesdays.

<h1><a id="Suggestions">Suggestions</a></h1>

1. The customer with ID 103 ordered the most pizzas (4), so we can give him some kind of VIP pass to encourage him.
2. The customer with ID 105 ordered the least amount of pizzas (1). We can give him discounts or reach out to him to encourage him to purchase more pizzas.
3. We can serve more varieties of non-vegetarian pizzas because they are in high demand.
4. The afternoon and evening are important time zones for us, so we can focus on that time and try to make and deliver pizzas fast. This will have a positive impact on the customer, as they will be happy with the service.
5. On Saturdays and Wednesdays, we can introduce discounts, which will gradually help the sales grow.


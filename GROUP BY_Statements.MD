<h1 align="center">GROUP BY Statements</h1>

### Aggregation Functions
* **AVG()**     - returns average value
* **COUNT()**   - returns number of values
* **MAX()**     - returns maximum value
* **MIN()**     - returns minimum value
* **SUM()**     - returns the sum of all values

#### _MIN_ Example
``SELECT MIN(replacement_cost) FROM film;``

#### _MAX_ Example
``SELECT MAX(replacement_cost) FROM film;``

#### _AVG_ Example
``SELECT AVG(replacement_cost) FROM film;``

### _AVG_ Example with ROUND and 2 decimal places
```
SELECT ROUND(AVG(replacement_cost),2)
FROM film;
```

#### _SUM_ Example
``SELECT AVG(replacement_cost) FROM film;``

#### _MIN_ and _MAX_ Example
```
SELECT MAX(replacement_cost), MIN(replacement_cost) 
FROM film;
```
#
### GROUP BY - Part One
Basic Syntax
```
SELECT category_col, AGG(data_col)
FROM table
GROUP BY category_col;
```
AGG - any Aggregation Functions

The **GROUP BY** clause must appear right after a **FROM** or **WHERE** statement.

#### Example
```
SELECT company, division, SUM(sales)
FROM finance_table
GROUP BY company, division;
```

#### Example
```
SELECT company, SUM(sales)
FROM finance_table
GROUP BY company
ORDER BY SUM(sales)
LIMIT 5;
```

### GROUP BY - Part Two

### Example - Returns two columns - customer_id and sum of amount column per customer_id

```
SELECT customer_id,SUM(amount) FROM payment
GROUP BY customer_id
ORDER BY SUM(amount);
```

### Challenge GROUP BY
#### 01. Return the staff member witht the most process payments
```
SELECT staff_id,COUNT(amount) FROM payment
GROUP BY staff_id
ORDER BY  staff_id;
```

#### 02. Return the AVG _replacement_cost_ per _rating_

```
SELECT rating, ROUND(AVG(replacement_cost),2) FROM film
GROUP BY rating
ORDER BY ROUND(AVG(replacement_cost),2);
```

#### 03. Return top 5 customers with most spending amount

```
SELECT customer_id, SUM(amount) FROM payment
GROUP BY customer_id
ORDER BY SUM(amount) DESC
LIMIT 5;
```

#
### HAVING
Allows us to filter **after** an aggregation has already taken place.

```
SELECT company,SUM(sales)
FROM finance_table
GROUP BY company
HAVING SUM(sales) > 1000;
```

#### Example - Return all the _amount_ SUM-s are grater 100
```
SELECT customer_id,SUM(amount) FROM payment
GROUP BY customer_id
HAVING SUM(amount) > 100;
```

### Challenge HAVING

#### Example - Return customers with more than 40 transaction payments
```
SELECT customer_id, COUNT(customer_id) FROM payment
GROUP BY customer_id
HAVING COUNT(customer_id) >= 40;
```

#### Example - Return customers who have spent more that 100$ with _staff_id_ member number 2
```
SELECT customer_id, SUM(amount) FROM payment
WHERE staff_id = 2
GROUP BY customer_id
HAVING SUM(amount) > 100;
```

### Assessment Test

#### 01. Return the customer IDs of customers who have spent at least $110 with the staff member who has an ID of 2.
```
SELECT customer_id,SUM(amount) FROM payment
WHERE staff_id =2
GROUP BY customer_id
HAVING SUM(amount) > 110;
```

#### 02. How many films begin with the letter J?
```
SELECT COUNT(title) FROM film
WHERE title LIKE 'J%';
```

#### 03. What customer has the highest customer ID number whose name starts with an 'E' and has an address ID lower than 500?
```
SELECT first_name, last_name FROM customer
WHERE first_name LIKE 'E%' AND address_id < 500
ORDER BY customer_id DESC
LIMIT 1;
```

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







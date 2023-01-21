<h1 align="center">JOINS</h1>

A join clause in SQL – corresponding to a join operation in relational algebra – combines columns from one or more tables into a new table. 

Informally, a join stitches two tables and puts on the same row records with matching fields:

``INNER``

``LEFT OUTER``

``RIGHT OUTER``

``FULL OUTER`` 

``CROSS``

### More info:

### [A Visual Explanation of SQL Joins](https://blog.codinghorror.com/a-visual-explanation-of-sql-joins/)

### [Join (SQL)](https://en.wikipedia.org/wiki/Join_(SQL))

#
### AS
Creating and _**"alias"**_ for column or result.

**NOTE:** Can't be used in _WHERE_ statement

``SELECT column AS new_name FROM table``

``SELECT SUM(column) AS new_name FROM table``

#### Example - Returns column name _new_revenue_ with the totao SUM amount
`` SELECT SUM(amount) AS net_revenue FROM payment;``

#### Example - Returns _total_spent_ per _customer_id_
```
SELECT customer_id, SUM(amount) AS total_spent
FROM payment
GROUP BY customer_id
HAVING SUM(amount) > 100;
```
#
### INNER JOIN

```
SELECT payment_id, payment.customer_id,first_name
FROM payment
INNER JOIN customer
ON payment.customer_id = customer.customer_id;
```
Output:

| |payment_id|customer_id|first_name |
|--|---------|-----------|-----------|
| 1|17503    | 314       | Peter     |
| 2|17503    | 314       | Peter     |
| 3|17504    | 314       | Peter     |
| 4|17505    | 314       | Peter     |
| 5|17506    | 314       | Peter     |
      


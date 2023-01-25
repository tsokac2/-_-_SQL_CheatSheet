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
This will result in the set of records that **"match in both"** tables.


#### Example - Returns new table where _reg_id_, _name_ and _log_id_ both have values

_Logins.name_ is a column that is present in both tables so we need to specify  from which table we want _name_ to be used

```
SELECT reg_id,Logins.name,log_id
FROM Registrations
INNER JOIN Logins
ON Registrations.name = Logins.name
```
#### Output:

|  |reg_id   |name       |log_id   |
|--|---------|-----------|---------|
| 1|17503    | 314       | L17503  |
| 2|17505    | 315       | L17504  |
| 3|17506    | 316       | L17505  |


#### Example
```
SELECT payment_id, payment.customer_id,first_name
FROM payment
INNER JOIN customer
ON payment.customer_id = customer.customer_id;
```
Output:

|  |payment_id|customer_id|first_name |
|--|---------|-----------|------------|
| 1|17503    | 314       | Peter      |
| 2|17503    | 314       | Peter      |
| 3|17504    | 314       | Peter      |
| 4|17505    | 314       | Peter      |
| 5|17506    | 314       | Peter      |
      
#
### FULL OUTER JOINS
This will result in the set of records that are not **"match in both"** tables returning a **null** value for an empty record.

#### Returning table _customer_ and _table_ payment merged together with all values from both tables.

```
SELECT * FROM customer
FULL OUTER JOIN payment
ON customer.customer_id = payment.customer_id;
```

#### FULL OUTER JOIN with WHERE Statement
Returning table _customer_ and table _payment_ merged together but excluding the rows that have **null** value either in _customer_ or _payment_ table.

```
SELECT * FROM customer
FULL OUTER JOIN payment
ON customer.customer_id = payment.customer_id
WHERE customer.customer_id IS NULL
OR payment.payment_id IS NULL;
```

#
### LEFT OUTER JOINS
Table order **DOES** matters for LEFT OUTER JOIN.

It will retrun an **NULL** values from Logins table if ther are some present.

```
SELECT * FROM Registrations
LEFT OUTER JOIN Logins
ON Registrations.name = Logins.name
```

#### LEFT OUTER JOIN with WHERE Statement
What if we only wanted entries unique to Table A?

Those rows found in Table A and **NOT** found in Table B.

#### Returns the _null_ values from the _Logins_ table for the _name_ column.
```
SELECT * FROM Registrations
LEFT OUTER JOIN Logins
ON Registrations.name = Logins.name
WHERE Logins.name IS NULL
```

#### Returns the new table where _film_id_ column is a starting point and in that order.

```
SELECT film.film_id, title, inventory_id, store_id
FROM film
LEFT OUTER JOIN inventory ON
film.film_id = inventory.film_id
WHERE inventory.film_id IS null;
```

#### Output:

|  |film_id  |title              |inventory_id | store_id|
|--|---------|-------------------|-------------|---------|
| 1|14       | Alice Fantasia    | null        |null     |
| 2|33       | Apollo Teen       | null        |null     |
| 3|33       | Argonauts Town    | null        |null     |
| 4|33       | Ark Ridgemont     | null        |null     |

#
### RIGHT OUTER JOINS
Is essentially the same as a **LEFT OUTER JOIN** except the tables are switched.

This would be the same as switching the table order in a **LEFT OUTER JOIN.**

```
SELECT * FROM TableA
RIGHT OUTER JOIN TableB
ON TableA.col_match = TableB.col_match;
```

#
### UNION
UNION operator is used to combine the result-set of two or more SELECT statements.

It basically serves to directly concatenate two results together, essentially "pasting" them together.

```
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2
```

#### Example Returns the new table that is UNION of Sales2021_Q1 and Sales2021_Q2.

```
SELECT * FROM Sales2021_Q1 
UNION
SELECT * FROM Sales2022_Q2
ORDER BY name;
```

#### Output:

|  |name     |amount |
|--|---------|-------|
| 1| David   | 100   | 
| 2| David   | 200   |
| 3| Claire  | 50    |
| 4| Claire  | 100   |

#
### Challenge

#### 01 - What are the emails of the customer who live in California?

```
SELECT district, email FROM customer
INNER JOIN address
ON address.address_id = customer.address_id
WHERE district = 'California';
```

#### Output:

|  |district    |email                                |
|--|------------|-------------------------------------|
| 1| California | patricia.johnson@sakilacustomer.org | 
| 2| California | betty.white@sakilacustomer.org      |
| 3| California | alice.stewart@sakilacustomer.org    |
| 4| California | rosa.reynolds@sakilacustomer.org    |


#### 02 - What are the emails of the customer who live in California?

A customer walks in and is a huge fan of the actor **"Nick Wahlberg"** and wants to know which movies he is in.

Get a list of all the movies "Nick Wahlberg" has been in.

```
SELECT title, first_name, last_name FROM film_actor
INNER JOIN actor
ON film_actor.actor_id = actor.actor_id
INNER JOIN film
ON film_actor.film_id = film.film_id
WHERE first_name = 'Nick' AND last_name = 'Wahlberg';
```

#### Output:

|  |title       |first_name |last_name        |
|--|------------|-----------|-----------------|
| 1| Adaptation Holes       | Nick | Wahlberg |
| 2| Apache Divine"         | Nick | Wahlberg |
| 3| Baby Hall              | Nick | Wahlberg |

                ....

|  |title       |first_name |last_name        |
|--|------------|-----------|-----------------|
|23| Rushmore Mermaid | Nick| Wahlberg        | 
|24| Smile Earring    | Nick| Wahlberg        |
|25| Wardrobe Phantom | Nick| Wahlberg        |





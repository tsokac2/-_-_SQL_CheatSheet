<h1 align="center">SQL_Statement_Fundamentals</h1>

### SELECT Statement
``SELECT column_name FROM table_name;``
#### Example for two or more columns
``SELECT column_1, column_2 FROM table_name;``

#### _SELECTING_ all data from table  use asterix symbol *
``SELECT * FROM table_name;``
#### _SELECTING_ First and Last name from table _actor_
``SELECT first_name, last_name FROM actor;``

#### _SELECTING_ First and Last Name and Email from table _customer_
``SELECT first_name,last_name, email FROM customer;``
#

### SELECT DISTINCT - returning a _unique_ values in a column
``DISTINCT`` keyword operates on a column.

``SELECT DISTINCT column FROM table_name;``

#### Returning _unique_ values in a coulmn _release_year_ from film _table_
``SELECT DISTINCT release_year FROM film;``

#### Returning _unique_ values in a coulmn _rating_ from _film_ table
``SELECT DISTINCT rating FROM film;``

#
### COUNT
Function returns the number of **input rows** that match a specific condition of a query

``SELECT COUNT(column_name) rating FROM table_name;``

#### Returning total count for rows in _payment_ table
``SELECT COUNT(*) FROM payment;``

#### Returning total count for rows in _amount_ column
``SELECT COUNT(amount) FROM payment;``

#### Returning total count for _unique_ rows in _amount_ column
``SELECT COUNT(DISTINCT amount) FROM payment;``

#
### SELECT WHERE
The ``WHERE`` statement allows us to specify conditions on columns for the rows to be returned.

```
SELECT col1, col2
FROM table
WHERE conditions;
```

#### Returning all the rows with _first_name_ is equal to _'Jared'_

```
SELECT * FROM customer
WHERE first_name = 'Jared';
```
#### Returning all the rows with _rental_rate_ is grather from 4
```
SELECT * FROM film
WHERE rental_rate > 4;
```
```
SELECT * FROM film
WHERE rental_rate > 4 AND replacement_cost >= 19.99
AND rating='R';
```
#### Returning the query for coulumn _title_ in _film_ table
```
SELECT title FROM film
WHERE rental_rate > 4 AND replacement_cost >= 19.99
AND rating='R';
```
#### Returning the total sum of rows that satisfied these conditions

```
SELECT COUNT(*) FROM film
WHERE rental_rate > 4 AND replacement_cost >= 19.99
AND rating='R';
```

#### Returning total sum of rows by using _OR_ operator
```
SELECT COUNT(*) FROM film
WHERE rating = 'R' OR rating = 'PG-13';
```

#### Returning total sum of rows by using _NOT EQUAL_ operator
```
SELECT * FROM film
WHERE rating != 'R';
```

#### Returning _email_ for "Nancy Thomas"
```
SELECT email FROM customer
WHERE first_name = 'Nancy' AND last_name = 'Thomas';
```
#
### ORDER BY
To store rows based on a column value, in either ascending or descending order.

**Note:** ASC by default
```
SELECT col1, col2
FROM table_name
ORDER BY col1 ASC/DESC
```

#### Returning column _first_name_ orderd by descending order
```
SELECT * FROM customer
ORDER BY first_name DESC;
```

#### Returning column _store_id_ orderd by descending order and column _first_name_ order by ascending order
```
SELECT store_id,first_name,last_name FROM customer
ORDER BY store_id DESC, first_name ASC;
```

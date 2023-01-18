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
DISTINCT keyword operates on a column.

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


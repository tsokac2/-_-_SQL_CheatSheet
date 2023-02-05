<h1 align="center">Conditional Expressions and Procedures Introduction</h1>

## Section Overview
* CASE
* COALESCE
* NULLIF
* CAST
* Views
* Import and Export Functionality
#
### CASE
We can use the CASE statement to only execute SQL code when certain conditions are met.

This is very similar to **IF/ELSE** statements in other programming languages.

Two man ways to use a CASE statement:
* General CASE
* CASE Expression

### General CASE
```
CASE
    WHEN condtition1 THEN result1
    WHEN condtition2 THEN result2
    ELSE some_other_result
END
```
#### Example

```
SELECT a,
CASE WHEN a = 1 THEN 'one'
     WHEN a = 2 THEN 'two'
ELSE 'other' AS label
END
FROM test;
```
#### Output:
|a |label|
|--|-----|
| 1| one |
| 2| two |

### CASE Expression
First evaluates an expression then compares the result with each value in the WHEN clauses sequentially.
```
CASE expression
    WHEN value1 THEN result1
    WHEN value 2 THEN result2
    ELSE some_other_result
END
```
#### Example
```
SELECT a,
CASE a 
    WHEN 1 THEN 'one'
    WHEN 2 THEN 'two'
ELSE 'other'
END
FROM test;
```
#### Output:
|a |label|
|--|-----|
| 1| one |
| 2| two |

#### Example
```
SELECT customer_id
CASE
    WHEN (customer_id <= 100) THEN 'Premium'
    WHEN (customer_id BETWEEN 100 and 200) THEN 'Plus'
    ELSE 'Normal'
END AS customer_class
FROM customer
```
#### Output:
|  |customer_id [PK] |customer_class|
|--|------------|--------------|
| 1| 524        | Normal       |
| 2| 1          | Premium      |
| 3| 2          | Premium      |
| 4| 3          | Premium      |
| 5| 4          | Premium      |

#### Example
```
SELECT customer_id
CASE customer_id
    WHEN 2 THEN 'Winner'
    WHEN 5 THEN 'Second Place'
    ELSE 'Normal'
END AS raffle_results
FROM customer
```
#### Output:
|  |customer_id [PK] |raffle_results|
|--|------------|--------------|
| 1| 524        | Normal       |
| 2| 1          | Normal       |
| 3| 2          | Winner       |
| 4| 3          | Normal       |
| 5| 4          | Normal       |
| 6| 5          | Second Place |

#### Example
```
SELECT rental_rate
CASE rental_rate
    WHEN 0.99 THEN 1
    ELSE 0
END
FROM film
```
#### Output:
|  |rental_rate | case |
|--|------------|------|
| 1| 4.99       | 0    |
| 2| 4.99       | 0    |
| 3| 4.99       | 0    |
| 4| 4.99       | 0    |
| 5| 4.99       | 1    |

#### Example with SUM
```
SELECT
SUM (CASE rental_rate
    WHEN 0.99 THEN 1
    ELSE 0
END) AS number_of_bargains
FROM film
```
#### Output:
|  |number_of_bargains|
|--|------------------|
| 1| 341              |

#### Example with SUM with two conditions '0.99' and '2.99'
```
SELECT
SUM (CASE rental_rate
    WHEN 0.99 THEN 1
    ELSE 0
END) AS number_of_bargains,
SUM (CASE rental_rate
    WHEN 2.99 THEN 1
    ELSE 0
END) AS regular
FROM film
```
#### Output:
|  |bargains|regular|
|--|--------|-------|
| 1| 341    |323    |

#
### COALESCE
Accepts an unlimited number of arguments.

It returns the first argumnet that is not null.

If all argumnets are null, the COALESCE function will return null.

Keep the COALESCE function in mind in case you encounter a table with null values that you want to perform operations on.

#
### CAST
The CAST operator let's you convert from one data type into another.

Keep in mind not every instance of a data type can be CAST to another data type, it must be reasonable to convert the data, for example '5' to an integer will work, 'five' to and integer will not.

#### Syntax for CAST function
`` SELECT CAST('5' AS INTEGER)``
#### PostgreSQL CAST operator - only avaiable in PostgreSQL Engine
`` SELECT '5'::INTEGER``

#### Example
`` SELECT CAST ('5' AS INTEGER) AS new_int ``
#### Output:
|  |new_int|
|--|-------|
| 1| 5     |

#### Example - Returning Charachter length
`` SELECT CHAR_LENGTH (CAST(inventory_id as VARCHAR) FROM rental ``
#### Output:
|  |char_length|
|--|-----------|
| 1| 4         |
| 2| 4         |
| 3| 4         |
| 4| 4         |
| 5| 4         |

#
### NULLIF
The NULLIF function takes in 2 inputs and returns NULL if both are equal, otherwise it returns the first argument passed.

``NULLIF(arg1,arg2)``

#### Example - Returns 10
`` NULLIF(10,12) ``

#### Example
```
SELECT (
    SUM (CASE WHEN department = 'A' THEN 1 ELSE 0 END) /
    NULLIF (SUM (CASE WHEN department = 'B' THEN 1 ELSE 0 END), 0)
) AS department_ratio
FROM depts;
```
#### Output:
|  |department_ratio|
|--|-----------------|
| 1| null            |

#
### VIEWS
Instead of having to perform the same query over and over again as a starting point, you can create a VIEW to quickly see this query with a simple call.

#### Example - Create View
```
CREATE VIEW customer_info AS
SELECT first_name,last_name,address FROM customer
INNER JOIN address
ON customer.address_id = address.address_id;
```
#### Output:
_CREATE VIEW_

_Query returned successfully in xxx msec_

#### Caling the previus query
``SELECT * FROM customer_info`` - will retrun the previus query with the 3 columns.

#### Example - Rename the View
``ALTER VIEW customer_info RENAME TO c_info``

#### Example - Drop View - DELETE the VIEW
``DROP VIEW customer_info``

#
### Import and Export

SUMMARY: This article explains how to import data from a CSV file into PostgreSQL and how to export it back from PostgreSQL to CSV. It includes an introduction to the CSV file format and some examples of its usage.

1. Creating a .csv file

2. Creating the table structure

3. Importing from a psql prompt

4. Importing from a shell prompt

5. Exporting data to a .csv file

### [Tutorial link](https://www.enterprisedb.com/postgres-tutorials/how-import-and-export-data-using-csv-files-postgresql)
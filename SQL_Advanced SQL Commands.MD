<h1 align="center">Advanced SQL Commands</h1>

### Overview of Advanced SQL Commands
* Timestamps and EXTRACT
* Math Functions
* String Functions
* Sub-query - query inside query
* Self-Join

#
### Timestamps and EXTRACT
#### ``TIME``  - Contains only time
#### ``DATE`` -  Contains only date
#### ``TIMESTAMP`` - Contains date and time
#### ``TIMESTAMPTZ`` - Contains date, time, and timezone

#
### SHOW
Show the value of a run-time parameter.

```SHOW ALL;``

### Output - 352 different paremeters

|  |name | setting |description|
|--|-----|---------|-----------|
| 1| allow_in_place_tablespaces| off | Allows tablespaces directly inside pg_tblspc, for testing.|

### ...  + 351 diferent paremeters

``SHOW TIMEZONE;``

### Output:

|  |TimeZone      |
|--|--------------|
| 1| Europe/London|


``SELECT NOW();``
### Output:

|  |now                           |
|--|------------------------------|
| 1| 2023-01-25 11:41:17.019535+00|

``SELECT TIMEOFDAY();``

|  |timeofday                           |
|--|------------------------------------|
| 1| Wed Jan 25 11:42:43.355134 2023 GMT|

``SELECT CURRENT_TIME;``

|  |current_time           |
|--|-----------------------|
| 1| 11:44:24.266380+00:00 |

``SELECT CURRENT_DATE;``

|  |current_date           |
|--|-----------------------|
| 1|  2023-01-25           |

#
### EXTRACT()
Allows you to "extract" or obtain a sub-component of a date value.
* YEAR
* MONTH
* DAY
* WEEK
* QUARTER

```
SELECT EXTRACT(YEAR FROM payment_date) AS my_year
FROM payment;
```
### Original column for _payment_date_

|  |payment_date               |
|--|---------------------------|
| 1| 2007-02-15 22:25:46.996577|

### Output:

|  |my_year |
|--|--------|
| 1|  2007  |

#### MONTH - EXTRACT(MONTH FROM col_name)
```
SELECT EXTRACT(MONTH FROM payment_date) AS my_month
FROM payment;
```
#### DAY - EXTRACT(DAY FROM col_name)
```
SELECT EXTRACT(DAY FROM payment_date) AS my_day
FROM payment;
```
#### WEEK - EXTRACT(WEEK FROM col_name)
```
SELECT EXTRACT(WEEK FROM payment_date) AS my_week
FROM payment;
```
#### QUARTER - EXTRACT(QUARTER FROM col_name)
```
SELECT EXTRACT(QUARTER FROM payment_date) AS my_quarter
FROM payment;
```
#
### AGE()
Calculates and returns the current age given a timestamp

#### Returns - 13 years 1 mon 5 days 01:34:13.003423

``AGE(date_col)``

#### Returns how old is that date for every row

#### Original column for _payment_date_

|  |payment_date               |
|--|---------------------------|
| 1| 2007-02-15 22:25:46.996577|

``SELECT AGE(payment_date) FROM payment;``

### Output:

|  |age |
|--|--------|
| 1| 15 years 11 mons 9 days 01:34:13.003423|

#
### TO_CHAR()
#### [Docs - Data Type Formatting Functions](https://www.postgresql.org/docs/12/functions-formatting.html)

General function convert data types to text. Useful for timestamp formatting:

`` TO_CHAR(date_col, 'mm-dd-yyyy') ``

#### Example for a _payment_date_ formating
```
SELECT TO_CHAR(payment_date,'MM-DD-YYYY') 
FROM payment;
```
### Output:

|  |to_char |
|--|--------|
| 1| 02-15-2007|

#
### Challenge

#### 01. During which months did payments occur?
Format your answer to return back the full month name.

```
SELECT DISTINCT(TO_CHAR(payment_date,'MONTH')) 
AS full_month
FROM payment;
```
### Output:

|  |full_month |
|--|-----------|
| 1| MARCH     |
| 2| MAY       |
| 3| FEBRUARY  |
| 4| APRIL     |

#### 02. How many payments occured on a Monday?

**SOLUTION 1:**
```
SELECT COUNT(*) FROM payment
WHERE (TO_CHAR(payment_date, 'FMDay')) = 'Monday';
```
**SOLUTION 2:**
```
SELECT COUNT(*) FROM payment
WHERE EXTRACT(dow FROM payment_date) = 1;
```
### Output:

|  |count |
|--|------|
| 1| 2948 |

#
## Mathematical Functions and Operators

### [Docs - Mathematical Functions and Operators](https://www.postgresql.org/docs/12/functions-math.html)

### ROUND function with 4 decimal places and converted in to %.

```
SELECT ROUND(rental_rate/replacement_cost, 4) * 100 AS percent_cost
FROM film;
```

### Output:

|  |percent_cost |
|--|-------------|
| 1| 33.2900     |
| 2| 24.9600     |
| 3| 31.2100     |
| 4| 38.4100     |
| 5| 4.7200      |
| 6| 38.4100     |
| 7| 15.2900     |
| 8| 11.0800     |

#
## String Functions and Operators
### ### [Docs - Mathematical Functions and Operators](https://www.postgresql.org/docs/15/functions-string.html)


### LENGTH - returns a number of charaters for every row

``SELECT LENGTH(frist_name) FROM customer;``

### Output:

|  |length |
|--|-------|
| 1| 5     |
| 2| 4     |
| 3| 8     |
| 4| 5     |


### Concatenates the two strings with space between them.

```
SELECT first_name || ' ' || last_name AS full_name
FROM customer;
```

### Concatenates the two strings with space between them and converts to _UPPER-case._

```
SELECT UPPER(first_name) || ' ' || UPPER(last_name) AS full_name
FROM customer;
```

### Concatenates the 3 strings by creating a customer email with first_name first letter in _LOWER_, last_name in _LOWER_ and '@gmail.com' email extention.

```
SELECT LOWER(LEFT(first_name,1)) || LOWER(last_name) || '@gmail.com'
AS custom_email
FROM customer;
```
### Output:

|  |custom_email      |
|--|------------------|
| 1| jely@gmail.com   |
| 2| msmith@gmail.com |



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

### Output

|  |TimeZone      |
|--|--------------|
| 1| Europe/London|


``SELECT NOW();``
### Output

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

### Output

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

### Output

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

### Output

|  |to_char |
|--|--------|
| 1| 02-15-2007|

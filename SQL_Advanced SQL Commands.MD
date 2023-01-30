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
### [Docs - Mathematical Functions and Operators](https://www.postgresql.org/docs/15/functions-string.html)


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


### SubQuery
Allows you to construct complex queries, essentially performing a query on the results of another query.

Good pratice, always start writing the subQuery first then the main query.

#### Example - Returns rental_rate grather then avarage rental_rate

```
SELECT title, rental_rate
FROM film
WHERE rental_rate > (SELECT AVG(rental_rate) FROM film);
```

#### Example - SubQuery using INNER JOIN and returning multiple results with IN operator.

```
SELECT film_id, title
FROM film
WHERE film_id IN
(SELECT inventory.film_id
FROM rental
INNER JOIN inventory 
ON inventory.inventory_id = rental.inventory_id
WHERE return_date BETWEEN '2005-05-29' AND '2005-05-30')
ORDER BY film_id;
```

#
## EXISTS
#### Example - Returns at customer that made at least one payment grather than 11$.

```
SELECT first_name, last_name
FROM customer AS c
WHERE EXISTS
(SELECT * FROM payment AS p
WHERE p.customer_id = c.customer_id
AND amount > 11)
```

#
## Self-Join
* Is a query in which a table is joined to itself.
* Self-joins are usefull for comparing values in a column of rows within the same table.
* The self-join can be viewed as a join of two copies of the same table.
* When using a self-join it is necessary to use an _**alias**_ ``AS`` for the table.


#### Syntax
```
SELECT emp.name, report.name AS rep
FROM employees AS emp
JOIN employess AS report
ON emp.emp_id = report.report_id;
```

#### Example - Find all the films pair that have same length in minutes and set tow sets of film _title_.

```
SELECT f1.title, f2.title, f1.length
FROM film AS f1
INNER JOIN film AS f2 ON 
f1.FILM_ID != f2.film_id
AND f1.length = f2.length;
```

### Output:

|  |title            |title                   |length  |
|--|-----------------|------------------------|--------|
| 1| Chamber Italian | Resurrection Silverado | 117    |


#
## Challenge SQL Assessment

### 01. How can you retrieve all the information from the cd.facilities table?
``SELECT * FROM cd.facilities;``

### 02. You want to print out a list of all of the facilities and their cost to members. How would you retrieve a list of only facility names and costs?
``SELECT name, membercost FROM cd.facilities;``

### 03. How can you produce a list of facilities that charge a fee to members?
```
SELECT * FROM cd.facilities
WHERE membercost != 0;
```

### 04. How can you produce a list of facilities that charge a fee to members, and that fee is less than 1/50th of the monthly maintenance cost? Return the facid, facility name, member cost, and monthly maintenance of the facilities in question.

```
SELECT facid,name, membercost, monthlymaintenance 
FROM cd.facilities
WHERE membercost > 0 AND 
membercost < (monthlymaintenance/50.0);
```
OR 
```
SELECT facid,name, membercost, monthlymaintenance 
FROM cd.facilities
WHERE membercost > 0 AND 
membercost < ((1/50.0)*monthlymaintenance);
```

### Output:

|  |facid |name         |membercost | monthlymaintenance|
|--|------|-------------|-----------|-------------------|
| 1| 4    | Massage Room 1 | 35        | 3000           |
| 2| 5    | Massage Room 2 | 35        | 3000           |

### 05. How can you produce a list of all facilities with the word 'Tennis' in their name?
```
SELECT *
FROM cd.facilities
WHERE name LIKE '%Tennis%';
```

### 06. How can you retrieve the details of facilities with ID 1 and 5? Try to do it without using the OR operator.

#### Using with OR Operator
```
SELECT *
FROM cd.facilities
WHERE facid=1 OR facid=5
```
#### Using without OR Operator
```
SELECT *
FROM cd.facilities
WHERE facid IN (1,5);
```

### 07. How can you produce a list of members who joined after the start of September 2012? Return the memid, surname, firstname, and joindate of the members in question.

```
SELECT memid,surname,firstname,joindate 
FROM cd.members
WHERE joindate >= '2012-09-01';
```

### 08. How can you produce an ordered list of the first 10 surnames in the members table? The list must not contain duplicates.

```
SELECT DISTINCT (surname)
FROM cd.members
ORDER BY surname
LIMIT 10;
```

### 09. You'd like to get the signup date of your last member. How can you retrieve this information?

```
SELECT joindate
FROM cd.members
ORDER BY joindate DESC
LIMIT 1;
```
OR

```
SELECT MAX(joindate)
FROM cd.members;
```

### 10. Produce a count of the number of facilities that have a cost to guests of 10 or more.

```
SELECT COUNT(guestcost)
FROM cd.facilities
WHERE guestcost >= 10;
```

### 11. Produce a list of the total number of slots booked per facility in the month of September 2012. Produce an output table consisting of facility id and slots, sorted by the number of slots.
```
SELECT facid, SUM(slots) AS total_slots
FROM cd.bookings
WHERE starttime >= '2012-09-01' AND 
starttime <= '2012-10-01'
GROUP BY facid ORDER BY SUM(slots);
```

### Output:

|  |facid |total_slots |
|--|------|------------|
| 1| 5    | 122 |
| 2| 3    | 422 |
| 3| 7    | 426 |
| 4| 8    | 471 |
| 5| 6    | 540 |
| 6| 2    | 570 |
| 7| 1    | 588 |
| 8| 0    | 591 |
| 9| 4    | 648 |


### 12. Produce a list of facilities with more than 1000 slots booked. Produce an output table consisting of facility id and total slots, sorted by facility id.

```
SELECT facid, SUM(slots) AS total_slots
FROM cd.bookings
GROUP BY facid
HAVING SUM(slots) > 1000
ORDER BY facid;
```

### Output:

|  |facid |total_slots |
|--|------|------------|
| 1| 0    | 1320       |
| 2| 1    | 1278       |
| 3| 2    | 1209       |
| 4| 4    | 1404       |
| 5| 6    | 1104       |

### 13. How can you produce a list of the start times for bookings for tennis courts, for the date '2012-09-21'? Return a list of start time and facility name pairings, ordered by the time.

```
SELECT cd.bookings.starttime, cd.facilities.name
FROM cd.facilities
INNER JOIN cd.bookings 
ON cd.facilities.facid = cd.bookings.facid
WHERE cd.facilities.facid IN (0,1)
AND cd.bookings.starttime >= '2012-09-21'
AND cd.bookings.starttime < '2012-09-22'
ORDER BY cd.bookings.starttime;
```

### 14. How can you produce a list of the start times for bookings by members named 'David Farrell'?

```
SELECT cd.bookings.starttime
FROM cd.bookings
INNER JOIN cd.members ON
cd.members.memid = cd.bookings.memid
WHERE cd.members.firstname = 'David'
AND cd.members.surname = 'Farrell';
```

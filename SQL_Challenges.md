<h1 align="center">SQL Challenges</h1>

### 1. From the following tables, write a SQL query to find the information on each salesperson of ABC Company. Return name, city, country and state of each salesperson.

### Input

### Table: _salespersons_

|salesperson_id|first_name|last_name|
|--------------|----------|---------|
|             1|Green     |Wright   |
|             2|Jones     |Collins  |
|             3|Bryant    |Davis    |

### Table: _address_

|address_id|salesperson_id|city       |state     |country|
|----------|--------------|-----------|----------|-------|
|         1|             2|Los Angeles|California|USA    |
|         2|             3|Denver     |Colorado  |USA    |
|         3|             4|Atlanta    |Georgia   |USA    |


### Solution

```
CREATE TABLE IF NOT EXISTS salespersons(salesperson_id int, first_name varchar(255), last_name varchar(255));
CREATE TABLE IF NOT EXISTS address (address_id int, salesperson_id  int, city varchar(255), state varchar(255), country varchar(255));
TRUNCATE TABLE address;
INSERT INTO salespersons (salesperson_id, first_name, last_name) VALUES ('1', 'Green', 'Wright');
INSERT INTO salespersons (salesperson_id, first_name, last_name) VALUES ('2', 'Jones', 'Collins');
INSERT INTO salespersons (salesperson_id, first_name, last_name) VALUES ('3', 'Bryant', 'Davis');

TRUNCATE TABLE address;
INSERT INTO address (address_id, salesperson_id, city, state, country) VALUES ('1', '2', 'Los Angeles','California', 'USA');
INSERT INTO address (address_id, salesperson_id, city, state, country) VALUES ('2', '3', 'Denver', 'Colorado','USA');
INSERT INTO address (address_id, salesperson_id, city, state, country) VALUES ('3', '4', 'Atlanta', 'Georgia','USA');

select * from address;
select * from salespersons;

SELECT first_name, last_name, city, state 
FROM salespersons LEFT JOIN address
ON salespersons.salesperson_id = address.salesperson_id;

```

### Output

|first_name|last_name|city       |state     |
|----------|---------|-----------|----------|
|Jones     |Collins  |Los Angeles|California|
|Bryant    |Davis    |Denver     |Colorado  |
|Green     |Wright   |           |          |

##

### 2. From the following table, write a SQL query to find the third highest sale. Return sale amount.

### Table: _salemast_

|sale_id|employee_id|sale_date |sale_amt|
|-------|-----------|----------|--------|
|      1|       1000|2012-03-08|    4500|
|      2|       1001|2012-03-09|    5500|
|      3|       1003|2012-04-10|    3500|
|      3|       1003|2012-04-10|    2500|

### Solution

```
CREATE TABLE If Not Exists salemast(sale_id int, employee_id int, sale_date date, sale_amt int);
TRUNCATE TABLE salemast;
INSERT INTO salemast (sale_id, employee_id, sale_date, sale_amt) VALUES ('1', '1000', '2012-03-08', 4500);
INSERT INTO salemast (sale_id, employee_id, sale_date, sale_amt) VALUES ('2', '1001', '2012-03-09', 5500);
INSERT INTO salemast (sale_id, employee_id, sale_date, sale_amt) VALUES ('3', '1003', '2012-04-10', 3500); 
INSERT INTO salemast (sale_id, employee_id, sale_date, sale_amt) VALUES ('3', '1003', '2012-04-10', 2500); 

SELECT  * FROM salemast;

SELECT DISTINCT sale_amt AS SecondHighestSale
FROM salemast
ORDER BY sale_amt DESC
LIMIT 1 OFFSET 1;
```

### Output

|SecondHighestSale|
|-----------------|
|             4500|

##

### 3.  From the following table, write a SQL query to find the N^th highest sale. Return sale amount.

### Input

### Table: _salemast_

|sale_id|employee_id|sale_date |sale_amt|
|-------|-----------|----------|--------|
|      1|       1000|2012-03-08|    4500|
|      2|       1001|2012-03-09|    5500|
|      3|       1003|2012-04-10|    3500|

### Solution
```
CREATE TABLE IF NOT EXISTS salemast(sale_id int, employee_id int, sale_date date, sale_amt int);
TRUNCATE TABLE salemast;
INSERT INTO salemast (sale_id, employee_id, sale_date, sale_amt) VALUES ('1', '1000', '2012-03-08', 4500);
INSERT INTO salemast (sale_id, employee_id, sale_date, sale_amt) VALUES ('2', '1001', '2012-03-09', 5500);
INSERT INTO salemast (sale_id, employee_id, sale_date, sale_amt) VALUES ('3', '1003', '2012-04-10', 3500); 
SELECT * FROM salemast;
```


### Output:

|getNthHighestSaleAmt(3)|
|-----------------------|
|                   3500|

## 

### 4. From the following table, write a SQL query to find the marks, which appear at least thrice one after another without interruption. Return the number.

### Input

### Table: _logs_

|student_id|marks|
|----------|-----|
|       101|   83|
|       102|   79|
|       103|   83|
|       104|   83|
|       105|   83|
|       106|   79|
|       107|   79|
|       108|   83|

```
CREATE TABLE IF NOT EXISTS logs (student_id int, marks int);
TRUNCATE TABLE logs;
INSERT INTO logs (student_id, marks) VALUES ('101', '83');
INSERT INTO logs (student_id, marks) VALUES ('102', '79');
INSERT INTO logs (student_id, marks) VALUES ('103', '83');
INSERT INTO logs (student_id, marks) VALUES ('104', '83');
INSERT INTO logs (student_id, marks) VALUES ('105', '83');
INSERT INTO logs (student_id, marks) VALUES ('106', '79');
INSERT INTO logs (student_id, marks) VALUES ('107', '79');
INSERT INTO logs (student_id, marks) VALUES ('108', '83');
select * from logs;
SELECT DISTINCT L1.marks AS  ConsecutiveNums
FROM (logs L1 JOIN logs L2 ON L1.marks = L2.marks AND L1.student_id = L2.student_id-1)
JOIN logs L3 ON L1.marks = L3.marks AND L2.student_id = L3.student_id-1;
```

### Output

|ConsecutiveNums|
|---------------|
|             83|
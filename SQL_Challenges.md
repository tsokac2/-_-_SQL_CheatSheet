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

### 4. From the following table, write a SQL query to find the marks, which appear at least 3 times one after another without interruption. Return the number.

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

### Solution

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

##

### 5. From the following table, write a SQL query to find all the duplicate emails (no upper case letters) of the employees. Return email id.

### Input

### Table: _employees_

|employee_id|employee_name|email_id     |
|-----------|-------------|-------------|
|        101|Liam Alton   |li.al@abc.com|
|        102|Josh Day     |jo.da@abc.com|
|        103|Sean Mann    |se.ma@abc.com|
|        104|Evan Blake   |ev.bl@abc.com|
|        105|Toby Scott   |jo.da@abc.com|


### Solution
```
CREATE TABLE IF NOT EXISTS employees(employee_id int, employee_name varchar(255), email_id varchar(255));
TRUNCATE TABLE employees;
INSERT INTO employees (employee_id,employee_name, email_id) VALUES ('101','Liam Alton', 'li.al@abc.com');
INSERT INTO employees (employee_id,employee_name, email_id) VALUES ('102','Josh Day', 'jo.da@abc.com');
INSERT INTO employees (employee_id,employee_name, email_id) VALUES ('103','Sean Mann', 'se.ma@abc.com');	
INSERT INTO employees (employee_id,employee_name, email_id) VALUES ('104','Evan Blake', 'ev.bl@abc.com');
INSERT INTO employees (employee_id,employee_name, email_id) VALUES ('105','Toby Scott', 'jo.da@abc.com');
SELECT * FROM employees;

SELECT email_id FROM
(
SELECT email_id, COUNT(email_id) AS nuOfAppearence
FROM employees
GROUP BY email_id
) AS countEmail
WHERE nuOfAppearence> 1;
```

### Output

|email_id     |
|-------------|
|jo.da@abc.com|

##

### 6. From the following tables, write a SQL query to find those customers who never ordered anything. Return customer name.

### Input

### Table: _customers_

|customer_id|customer_name|
|-----------|-------------|
|        101|Liam         |
|        102|Josh         |
|        103|Sean         |
|        104|Evan         |
|        105|Toby         |

### Table: _orders_

|order_id|customer_id|order_date|order_amount|
|--------|-----------|----------|------------|
|     401|        103|2012-03-08|        4500|
|     402|        101|2012-09-15|        3650|
|     403|        102|2012-06-27|        4800|

### Solution

```
CREATE TABLE IF NOT EXISTS customers (customer_id int, customer_name varchar(255));
TRUNCATE TABLE customers;
INSERT INTO customers (customer_id, customer_name) VALUES ('101', 'Liam');
INSERT INTO customers (customer_id, customer_name) VALUES ('102', 'Josh');
INSERT INTO customers (customer_id, customer_name) VALUES ('103', 'Sean');
INSERT INTO customers (customer_id, customer_name) VALUES ('104', 'Evan');
INSERT INTO customers (customer_id, customer_name) VALUES ('105', 'Toby');	
CREATE TABLE IF NOT EXISTS orders (order_id int, customer_id int, order_date Date, order_amount int);
TRUNCATE TABLE orders;
INSERT INTO orders (order_id, customer_id,order_date,order_amount) VALUES ('401', '103','2012-03-08','4500');
INSERT INTO orders (order_id, customer_id,order_date,order_amount) VALUES ('402', '101','2012-09-15','3650');
INSERT INTO orders (order_id, customer_id,order_date,order_amount) VALUES ('403', '102','2012-06-27','4800');
SELECT * FROM customers;
SELECT * FROM orders;

SELECT customer_name as customers
FROM customers
WHERE customer_id NOT IN
(
SELECT customer_id FROM orders
);
```

### Output

|Customers|
|---------|
|Evan     |
|Toby     |

##

### 7. From the following table, write a SQL query to remove all the duplicate emails of employees keeping the unique email with the lowest employee id. Return employee id and unique emails.

### Input

### Table: _employees_

|employee_id|employee_name|email_id     |
|-----------|-------------|-------------|
|        101|Liam Alton   |li.al@abc.com|
|        102|Josh Day     |jo.da@abc.com|
|        103|Sean Mann    |se.ma@abc.com|
|        104|Evan Blake   |ev.bl@abc.com|
|        105|Toby Scott   |jo.da@abc.com|

### Solution
```
CREATE TABLE IF NOT EXISTS employees(employee_id int, employee_name varchar(255), email_id varchar(255));
TRUNCATE TABLE employees;
INSERT INTO employees (employee_id,employee_name, email_id) VALUES ('101','Liam Alton', 'li.al@abc.com');
INSERT INTO employees (employee_id,employee_name, email_id) VALUES ('102','Josh Day', 'jo.da@abc.com');
INSERT INTO employees (employee_id,employee_name, email_id) VALUES ('103','Sean Mann', 'se.ma@abc.com');	
INSERT INTO employees (employee_id,employee_name, email_id) VALUES ('104','Evan Blake', 'ev.bl@abc.com');
INSERT INTO employees (employee_id,employee_name, email_id) VALUES ('105','Toby Scott', 'jo.da@abc.com');
SELECT * FROM employees;

DELETE e1 FROM employees e1,  employees e2
WHERE
    e1.email_id = e2.email_id AND e1.employee_id > e2.employee_id;
SELECT * FROM employees;
```

### Output

|employee_id|employee_name|email_id     |
|-----------|-------------|-------------|
|        101|Liam Alton   |li.al@abc.com|
|        102|Josh Day     |jo.da@abc.com|
|        103|Sean Mann    |se.ma@abc.com|
|        104|Evan Blake   |ev.bl@abc.com|

##

### 8. From the following table, write a SQL query to find all dates' city ID with higher pollution compared to its previous dates (yesterday). Return city ID, date and pollution.

### Input

### Table: _so2_pollution_

|city_id|date      |so2_amt|
|-------|----------|-------|
|    701|2015-10-15|      5|
|    702|2015-10-16|      7|
|    703|2015-10-17|      9|
|    704|2018-10-18|     15|
|    705|2015-10-19|     14|

### Solution

```
CREATE TABLE IF NOT EXISTS so2_pollution (city_id int, date date, so2_amt int);
TRUNCATE TABLE so2_pollution;
INSERT INTO so2_pollution (city_id, date, so2_amt) VALUES ('701', '2015-10-15', '5');
INSERT INTO so2_pollution (city_id, date, so2_amt) VALUES ('702', '2015-10-16', '7');
INSERT INTO so2_pollution (city_id, date, so2_amt) VALUES ('703', '2015-10-17', '9');
INSERT INTO so2_pollution (city_id, date, so2_amt) VALUES ('704', '2018-10-18', '15');
INSERT INTO so2_pollution (city_id, date, so2_amt) VALUES ('705', '2015-10-19', '14');
SELECT * FROM so2_pollution;
 
SELECT so2_pollution.city_id AS 'City ID'
FROM so2_pollution
        JOIN
so2_pollution p ON DATEDIFF(so2_pollution.date, p.date) = 1
        AND so2_pollution.so2_amt > p.so2_amt;
```

### Output

|City ID|
|-------|
|    702|
|    703|

##


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

### 3.  From the following table, write a SQL query to find the $N^/th/$ highest sale. Return sale amount.

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

### 9. A salesperson is a person whose job is to sell products or services. From the following tables, write a SQL query to find the top 10 salesperson that have made highest sale. Return their names and total sale amount.

### Input

### Table: _sales_

|TRANSACTION_ID|SALESMAN_ID|SALE_AMOUNT|
|--------------|-----------|-----------|
|           501|         18|    5200.00|
|           502|         50|    5566.00|
|           503|         38|    8400.00|
...
|           599|         24|   16745.00|
|           600|         12|   14900.00|

### Table: _salesman_

|SALESMAN_ID|SALESMAN_NAME        |
|-----------|---------------------|
|         11|Jonathan Goodwin     |
|         12|Adam Hughes          |
|         13|Mark Davenport       |
....
|         59|Cleveland Hart       |
|         60|Marion Gregory       |

### Solution
```
DROP TABLE IF EXISTS sales;
CREATE TABLE sales (
TRANSACTION_ID INTEGER(5) NOT NULL,
SALESMAN_ID   INTEGER(4) NOT NULL,
SALE_AMOUNT  decimal(8,2),
PRIMARY KEY (TRANSACTION_ID)
) ENGINE=MyISAM DEFAULT CHARSET=latin1;
INSERT INTO sales VALUES(501,18,5200.00),(502,50,5566.00),(503,38,8400.00),(504,43,8400.00),(505,11,9000.00);
INSERT INTO sales VALUES (506,42,5900.00),(507,13,7000.00),(508,33,6000.00),(509,41,8200.00),(510,11,4500.00);
INSERT INTO sales VALUES (511,51,10000.00),(512,29,9500.00),(513,59,6500.00),(514,38,7800.00),(515,58,9800.00);
INSERT INTO sales VALUES (516,60,12000.00),(517,58,13900.00),(518,23,12200.00),(519,34,5480.00),(520,35,8129.00);
INSERT INTO sales VALUES (521,49,9323.00),(522,46,8200.00),(523,47,9990.00),(524,42,14000.00),(525,44,7890.00);
INSERT INTO sales VALUES (526,47,5990.00),(527,21,7770.00),(528,57,6645.00),(529,56,5125.00),(530,25,10990.00);
INSERT INTO sales VALUES (531,21,12600.00),(532,41,5514.00),(533,17,15600.00),(534,44,15000.00),(535,12,17550.00);
INSERT INTO sales VALUES (536,55,13000.00),(537,58,16800.00),(538,25,19900.00),(539,57,9990.00),(540,28,8900.00);
INSERT INTO sales VALUES (541,44,10200.00),(542,57,18000.00),(543,34,16200.00),(544,36,19998.00),(545,30,13500.00);
INSERT INTO sales VALUES (546,37,15520.00),(547,36,20000.00),(548,20,19800.00),(549,22,18530.00),(550,19,12523.00);
INSERT INTO sales VALUES (551,46,9885.00),(552,22,7100.00),(553,54,17500.00),(554,19,19600.00),(555,24,17500.00);
INSERT INTO sales VALUES (556,38,7926.00),(557,49,7548.00),(558,15,9778.00),(559,56,19330.00),(560,24,14400.00);
INSERT INTO sales VALUES (561,18,16700.00),(562,54,6420.00),(563,31,18720.00),(564,21,17220.00),(565,48,18880.00); 
INSERT INTO sales VALUES (566,33,8882.00),(567,44,19550.00),(568,22,14440.00),(569,53,19500.00),(570,30,5300.00);
INSERT INTO sales VALUES (571,30,10823.00),(572,35,13300.00),(573,35,19100.00),(574,18,17525.00),(575,60,8995.00);
INSERT INTO sales VALUES (576,53,9990.00),(577,21,7660.00),(578,27,18990.00),(579,11,18200.00),(580,30,12338.00);
INSERT INTO sales VALUES (581,37,11000.00),(582,27,11980.00),(583,18,12628.00),(584,52,11265.00),(585,53,19990.00);
INSERT INTO sales VALUES (586,27,8125.00),(587,25,7128.00),(588,57,6760.00),(589,19,5985.00),(590,52,17641.00);
INSERT INTO sales VALUES (591,53,11225.00),(592,22,12200.00),(593,59,16520.00),(594,35,19990.00),(595,42,19741.00);
INSERT INTO sales VALUES (596,19,15000.00),(597,57,19625.00),(598,53,9825.00),(599,24,16745.00),(600,12,14900.00);
DROP TABLE IF EXISTS salesman;
CREATE TABLE salesman (
SALESMAN_ID 	            INTEGER(4) NOT NULL,
SALESMAN_NAME               varchar(30),
PRIMARY KEY (SALESMAN_ID)
) ENGINE=MyISAM DEFAULT CHARSET=latin1;

INSERT INTO salesman VALUES(11	,'Jonathan Goodwin     ');
INSERT INTO salesman VALUES(12	,'Adam Hughes          ');
INSERT INTO salesman VALUES(13	,'Mark Davenport       ');
INSERT INTO salesman VALUES(14	,'Jamie Shelley        ');
INSERT INTO salesman VALUES(15	,'Ethan Birkenhead     ');
INSERT INTO salesman VALUES(16	,'Liam Alton           ');
INSERT INTO salesman VALUES(17	,'Josh Day             ');
INSERT INTO salesman VALUES(18	,'Sean Mann            ');
INSERT INTO salesman VALUES(19	,'Evan Blake           ');
INSERT INTO salesman VALUES(20	,'Rhys Emsworth        ');
INSERT INTO salesman VALUES(21	,'Kian Wordsworth      ');
INSERT INTO salesman VALUES(22	,'Frederick Kelsey     ');
INSERT INTO salesman VALUES(23	,'Noah Turner          ');
INSERT INTO salesman VALUES(24	,'Callum Bing          ');
INSERT INTO salesman VALUES(25	,'Harri Wilberforce    ');
INSERT INTO salesman VALUES(26	,'Gabriel Gibson       ');
INSERT INTO salesman VALUES(27	,'Richard York         ');
INSERT INTO salesman VALUES(28	,'Tobias Stratford     ');
INSERT INTO salesman VALUES(29	,'Will Kirby           ');
INSERT INTO salesman VALUES(30	,'Bradley Wright       ');
INSERT INTO salesman VALUES(31	,'Eli Willoughby       ');
INSERT INTO salesman VALUES(32	,'Patrick Riley        ');
INSERT INTO salesman VALUES(33	,'Kieran Freeman       ');
INSERT INTO salesman VALUES(34	,'Toby Scott           ');
INSERT INTO salesman VALUES(35	,'Elliot Clapham       ');
INSERT INTO salesman VALUES(36	,'Lewis Moss           ');
INSERT INTO salesman VALUES(37	,'Joshua Atterton      ');
INSERT INTO salesman VALUES(38	,'Jonathan Reynolds    ');
INSERT INTO salesman VALUES(39	,'David Hill           ');
INSERT INTO salesman VALUES(40	,'Aidan Yeardley       ');
INSERT INTO salesman VALUES(41	,'Dan Astley           ');
INSERT INTO salesman VALUES(42	,'Finlay Dalton        ');
INSERT INTO salesman VALUES(43	,'Toby Rodney          ');
INSERT INTO salesman VALUES(44	,'Ollie Wheatley       ');
INSERT INTO salesman VALUES(45	,'Sean Spalding        ');
INSERT INTO salesman VALUES(46	,'Jason Wilson         ');
INSERT INTO salesman VALUES(47	,'Christopher Wentworth');
INSERT INTO salesman VALUES(48	,'Cameron Ansley       ');
INSERT INTO salesman VALUES(49	,'Henry Porter         ');
INSERT INTO salesman VALUES(50	,'Ezra Winterbourne    ');
INSERT INTO salesman VALUES(51	,'Rufus Fleming        ');
INSERT INTO salesman VALUES(52	,'Wallace Dempsey      ');
INSERT INTO salesman VALUES(53	,'Dan McKee            ');
INSERT INTO salesman VALUES(54	,'Marion Caldwell      ');
INSERT INTO salesman VALUES(55	,'Morris Phillips      ');
INSERT INTO salesman VALUES(56	,'Chester Chandler     ');
INSERT INTO salesman VALUES(57	,'Cleveland Klein      ');
INSERT INTO salesman VALUES(58	,'Hubert Bean          ');
INSERT INTO salesman VALUES(59	,'Cleveland Hart       ');
INSERT INTO salesman VALUES(60	,'Marion Gregory       ');

SELECT salesman_name, SUM(sale_amount) as total_sale
FROM salesman  a JOIN sales b ON a.salesman_id = b.salesman_id
GROUP BY 1
ORDER BY 2 DESC
LIMIT 10;
```


### Output

|salesman_name        |total_sale|
|---------------------|----------|
|Dan McKee            |  70530.00|
|Cleveland Klein      |  61020.00|
|Elliot Clapham       |  60519.00|
|Evan Blake           |  53108.00|
|Ollie Wheatley       |  52640.00|
|Frederick Kelsey     |  52270.00|
|Sean Mann            |  52053.00|
|Callum Bing          |  48645.00|
|Kian Wordsworth      |  45250.00|
|Bradley Wright       |  41961.00|

##

### 10. An active customer is simply someone who has bought company's product once before and has returned to make another purchase within 10 days.From the following table, write a SQL query to identify the active customers. Show the list of customer IDs of active customers.

### Input

### Table: _orders_

|ORDER_ID|CUSTOMER_ID|ITEM_DESC|ORDER_DATE|
|--------|-----------|---------|----------|
|     101|       2109|juice    |2020-03-03|
|     102|       2139|chocolate|2019-03-18|
|     103|       2120|juice    |2019-03-18|
|...
|     199|       2130|juice    |2019-03-16|
|     200|       2117|cake     |2021-03-10|


### Solution
```
DROP TABLE IF EXISTS `orders`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!40101 SET character_set_client = utf8 */;
CREATE TABLE `orders` (
`ORDER_ID`            INTEGER(5) NOT NULL,
`CUSTOMER_ID` 	            INTEGER(4) NOT NULL,
`ITEM_DESC` 	            varchar(30) NOT NULL,
`ORDER_DATE`               date NOT NULL,
PRIMARY KEY (`ORDER_ID`)
) ENGINE=MyISAM DEFAULT CHARSET=latin1;

INSERT INTO orders VALUES(101,2109,'juice','2020-03-03');
INSERT INTO orders VALUES(102,2139,'chocolate','2019-03-18');
INSERT INTO orders VALUES(103,2120,'juice','2019-03-18');
INSERT INTO orders VALUES(104,2108,'cookies','2019-03-18');
INSERT INTO orders VALUES(105,2130,'juice','2020-03-28');
INSERT INTO orders VALUES(106,2103,'cake','2019-03-29');
INSERT INTO orders VALUES(107,2122,'cookies','2021-03-07');
INSERT INTO orders VALUES(108,2125,'cake','2021-03-13');
INSERT INTO orders VALUES(109,2139,'cake','2019-03-30');
INSERT INTO orders VALUES(110,2141,'cookies','2019-03-17');
INSERT INTO orders VALUES(111,2116,'cake','2020-03-31');
INSERT INTO orders VALUES(112,2128,'cake','2021-03-04');
INSERT INTO orders VALUES(113,2146,'chocolate','2021-03-04');
INSERT INTO orders VALUES(114,2119,'cookies','2020-03-28');
INSERT INTO orders VALUES(115,2142,'cake','2019-03-09');
INSERT INTO orders VALUES(116,2122,'cake','2019-03-06');
INSERT INTO orders VALUES(117,2128,'chocolate','2019-03-24');
INSERT INTO orders VALUES(118,2112,'cookies','2019-03-24');
INSERT INTO orders VALUES(119,2149,'cookies','2020-03-29');
INSERT INTO orders VALUES(120,2100,'cookies','2021-03-18');
INSERT INTO orders VALUES(121,2130,'juice','2021-03-16');
INSERT INTO orders VALUES(122,2103,'juice','2019-03-31');
INSERT INTO orders VALUES(123,2112,'cookies','2019-03-23');
INSERT INTO orders VALUES(124,2102,'cake','2020-03-25');
INSERT INTO orders VALUES(125,2120,'chocolate','2020-03-21');
INSERT INTO orders VALUES(126,2109,'cake','2019-03-22');
INSERT INTO orders VALUES(127,2101,'juice','2021-03-01');
INSERT INTO orders VALUES(128,2138,'juice','2019-03-19');
INSERT INTO orders VALUES(129,2100,'juice','2019-03-29');
INSERT INTO orders VALUES(130,2129,'juice','2021-03-02');
INSERT INTO orders VALUES(131,2123,'juice','2020-03-31');
INSERT INTO orders VALUES(132,2104,'chocolate','2020-03-31');
INSERT INTO orders VALUES(133,2110,'cake','2021-03-13');
INSERT INTO orders VALUES(134,2143,'cake','2019-03-27');
INSERT INTO orders VALUES(135,2130,'juice','2019-03-12');
INSERT INTO orders VALUES(136,2128,'juice','2020-03-28');
INSERT INTO orders VALUES(137,2133,'cookies','2019-03-21');
INSERT INTO orders VALUES(138,2150,'cookies','2019-03-20');
INSERT INTO orders VALUES(139,2120,'juice','2020-03-27');
INSERT INTO orders VALUES(140,2109,'cake','2021-03-02');
INSERT INTO orders VALUES(141,2110,'cake','2021-03-13');
INSERT INTO orders VALUES(142,2140,'juice','2019-03-09');
INSERT INTO orders VALUES(143,2112,'cookies','2021-03-04');
INSERT INTO orders VALUES(144,2117,'chocolate','2019-03-19');
INSERT INTO orders VALUES(145,2137,'cookies','2020-03-23');
INSERT INTO orders VALUES(146,2130,'cake','2021-03-09');
INSERT INTO orders VALUES(147,2133,'cake','2020-03-08');
INSERT INTO orders VALUES(148,2143,'juice','2019-03-11');
INSERT INTO orders VALUES(149,2111,'chocolate','2020-03-23');
INSERT INTO orders VALUES(150,2150,'cookies','2021-03-04');
INSERT INTO orders VALUES(151,2131,'cake','2020-03-10');
INSERT INTO orders VALUES(152,2140,'chocolate','2019-03-17');
INSERT INTO orders VALUES(153,2147,'cookies','2020-03-22');
INSERT INTO orders VALUES(154,2119,'chocolate','2019-03-15');
INSERT INTO orders VALUES(155,2116,'juice','2021-03-12');
INSERT INTO orders VALUES(156,2141,'juice','2021-03-14');
INSERT INTO orders VALUES(157,2143,'cake','2019-03-16');
INSERT INTO orders VALUES(158,2105,'cake','2020-03-21');
INSERT INTO orders VALUES(159,2149,'chocolate','2019-03-11');
INSERT INTO orders VALUES(160,2117,'cookies','2020-03-22');
INSERT INTO orders VALUES(161,2150,'cookies','2020-03-21');
INSERT INTO orders VALUES(162,2134,'cake','2019-03-08');
INSERT INTO orders VALUES(163,2133,'cookies','2019-03-26');
INSERT INTO orders VALUES(164,2127,'juice','2019-03-27');
INSERT INTO orders VALUES(165,2101,'juice','2019-03-26');
INSERT INTO orders VALUES(166,2137,'chocolate','2021-03-12');
INSERT INTO orders VALUES(167,2113,'chocolate','2019-03-21');
INSERT INTO orders VALUES(168,2141,'cake','2019-03-21');
INSERT INTO orders VALUES(169,2112,'chocolate','2021-03-14');
INSERT INTO orders VALUES(170,2118,'juice','2020-03-30');
INSERT INTO orders VALUES(171,2111,'juice','2019-03-19');
INSERT INTO orders VALUES(172,2146,'chocolate','2021-03-13');
INSERT INTO orders VALUES(173,2148,'cookies','2021-03-14');
INSERT INTO orders VALUES(174,2100,'cookies','2021-03-13');
INSERT INTO orders VALUES(175,2105,'cookies','2019-03-05');
INSERT INTO orders VALUES(176,2129,'juice','2021-03-02');
INSERT INTO orders VALUES(177,2121,'juice','2019-03-16');
INSERT INTO orders VALUES(178,2117,'cake','2020-03-11');
INSERT INTO orders VALUES(179,2133,'juice','2020-03-12');
INSERT INTO orders VALUES(180,2124,'cake','2019-03-31');
INSERT INTO orders VALUES(181,2145,'cake','2021-03-07');
INSERT INTO orders VALUES(182,2105,'cookies','2019-03-09');
INSERT INTO orders VALUES(183,2131,'juice','2019-03-09');
INSERT INTO orders VALUES(184,2114,'chocolate','2020-03-31');
INSERT INTO orders VALUES(185,2120,'juice','2021-03-06');
INSERT INTO orders VALUES(186,2130,'juice','2021-03-06');
INSERT INTO orders VALUES(187,2141,'chocolate','2019-03-11');
INSERT INTO orders VALUES(188,2147,'cake','2021-03-14');
INSERT INTO orders VALUES(189,2118,'juice','2019-03-15');
INSERT INTO orders VALUES(190,2136,'chocolate','2020-03-22');
INSERT INTO orders VALUES(191,2132,'cake','2021-03-06');
INSERT INTO orders VALUES(192,2137,'chocolate','2019-03-31');
INSERT INTO orders VALUES(193,2107,'cake','2021-03-01');
INSERT INTO orders VALUES(194,2111,'chocolate','2019-03-18');
INSERT INTO orders VALUES(195,2100,'cake','2019-03-07');
INSERT INTO orders VALUES(196,2106,'juice','2020-03-21');
INSERT INTO orders VALUES(197,2114,'cookies','2019-03-25');
INSERT INTO orders VALUES(198,2110,'cake','2019-03-27');
INSERT INTO orders VALUES(199,2130,'juice','2019-03-16');
INSERT INTO orders VALUES(200,2117,'cake','2021-03-10');
SELECT * FROM orders;

SELECT DISTINCT a.customer_id
FROM orders a, orders  b
where (a.customer_id=b.customer_id) AND (a.order_id!=b.order_id) AND (b.order_date - a.order_date) BETWEEN 0 AND 10
ORDER BY customer_id;
```

### Output

|customer_id|
|-----------|
|       2103|
|       2110|
|       2111|
|       2112|
|       2129|
|       2130|
<h1 align="center">SQL Challenges</h1>

## 1. From the following tables, write a SQL query to find the information on each salesperson of ABC Company. Return name, city, country and state of each salesperson.

### Input

### Table: _salespersons_

|salesperson_id|first_name|last_name|
|--------------|----------|---------|
|             1|Green     |Wright   |
|            2|Jones     |Collins  |
|            3|Bryant    |Davis    |

### Table: _address_

|address_id|salesperson_id|city       |state     |country|
|----------|--------------|-----------|----------|-------|
|         1|             2|Los Angeles|California|USA    |
|         2|             3|Denver     |Colorado  |USA    |
|         3|             4|Atlanta    |Georgia   |USA    |

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


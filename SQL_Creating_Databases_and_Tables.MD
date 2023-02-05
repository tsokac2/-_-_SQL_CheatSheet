<h1 align="center">SQL Creating Databases and Tables</h1>

## Data Types
### Boolean
* True or False

### Character
* Char
* Varchar
* Text

### Numeric
* Integer
* Floating-point number - Float

### Temporal
* Data
* Time
* Timestamp
* Interval

### UUID
* Universally Uniquw Identifiers

### Arry
* Stores and array of strings, numbers etc.

### JSON
### Hstore key-value pair
### Special types such as network address and geometric data.
#

## Primary Keys and Foreign Keys

### Primary Keys - PK
Is a column or a group of columns used to identify a row uniquely in a table.

**NOTE**: its integer based and unique.

**Example** - non-null _customer_id_ as primary key.

Thye are important since they allow us to easily discern what columns should be used for joining tables together.


### Foreign Keys
Is a field or group of fields in a table that uniquely identifies a row in another table.

Foreign key is defined in a table that references to the primary key of the other table.

#### Example - Multiple Foreign Key References

|payment_id [PK] integer |customer_id|staff_id|rental_id|amount|
|------|-----|---|------|------|
| 17503| 341 | 2 | 1520 | 7.99 |
| 17504| 341 | 1 | 1778 | 1.99 |
| 17505| 341 | 1 | 1849 | 7.99 |
| 17506| 341 | 2 | 2819 | 2.99 |

### Constraints
Can be divided into two main categories:
#### Column Constraints
Constrains the date in a column to adhere to certain conditions.
#### Table Constraints
Applied to the entire table rather than to an individual column.

### The most common Constraints used on a _columns_
#### NOT NULL Constraint
* Ensures that a column cannot have NULL value.
#### UNIQUE CONSTRAINT
* Ensires that all values in a column are different.
#### PRIMARY Key
* Uniquely identifies each row/record in a database table.
#### FOREIGN Key
* Constrains data based on _**columns**_ in other tables.
#### CHECK Constraint
* Ensures that all values in a column satisfy certain conditions.
#### EXCLUSION Constraint
* Ensures that if any two rows are compared on the specified column or expression using the specified operator, not all of these comparisons will return TRUE.

### Table Constraints
#### CHECK (condition)
* to check a condition when inserting or updating data.
#### REFERENCES
* to constrain the value stored in the column that must exist in a column in another table.
#### UNIQUE (column_list)
* forces the values stored in the columns listed inside the parentheses to be unique
#### PRIMARY KEY(column_list)
* allows you to define the primary key that consist of multiple columns.

#
## CREATE Table
### CREATE
Common Simple Syntax
```
CREATE TABLE table_name(
    column_name TYPE column_constraint,
    column_name TYPE column_constraint,
);
```

Creating new table _players_:
```
CREATE TABLE players(
    player_id SERIAL TYPE PRIMARY KEY,
    age SMALLINT NOT NULL
);
```

### SERIAL
A sequence is a special kind of database object that generates a sequence of integers.

A sequence is often used as the primary key column in a table.

It logs unique integer entries for you automatically upon insertion.

#### Example
```
CREATE TABLE account(
	user_id SERIAL PRIMARY KEY,
	username VARCHAR(50) UNIQUE NOT NULL,
	password  VARCHAR(50) NOT NULL,
	email VARCHAR(255) UNIQUE NOT NULL,
	created_on  TIMESTAMP NOT NULL,
	last_login TIMESTAMP
);
```
### INSERT INTO
Allows you to add in rows to a table.

#### Example 1
```
INSERT INTO account(username, password, email, created_on)
VALUES
('Jose','password','jose@mail.com', CURRENT_TIMESTAMP);
```
#### Example 2
```
INSERT INTO job(job_name)
VALUES
('Astrounaut');
```
### UPDATE
Keyword allows for the changing of values of the columns in a table.

```
UPDATE account
SET last_login = CURRENT_TIMESTAMP;
```
#### Example 1
```
UPDATE account
SET last_login = created_on
```
#### Output:

|created_on  |last_login|
|------|-----|
| 2023-02-04 22:49:36.500633 | 2023-02-04 22:49:36.500633|

#### Example 2 - Update a Timestamp value from another table.
```
UPDATE account_job
SET hire_date = account.created_on
FROM account
WHERE account_job.user_id = account.user_id
```
#### Example 3 - Update a one row with specific id.
```
UPDATE job
SET job_name = 'President'
WHERE job.job_id = 2;
```
### DELETE
To remove rows from a table.

#### Delete one row in a table
```
DELETE FROM table
WHERE row_id = 1;
```
#### Delete rows based on their presence in other tables
```
DELETE FROM tableA
USING tableB
WHERE tableA.id = tableB.id;
```
#### Delete all rows from a table
``DELETE FROM table_name;``

#### Delete one row with specific _job_name_ value of _"Cowboy"_ and show deleted row in a output table.
```
DELETE FROM job
WHERE job_name = 'Cowboy'
RETRUNING job_id, job_name;
```
#### Output:
|job_id  |job_name|
|--------|--------|
| 3      | Cowboy |

### ALTER
Allows for changes to an existing table structure, such as:
* Adding, droping, or renaming columns
* Changing a columns' data type
* Set DEFAULT values for a column
* Add CHECK constraints
* Rename table

#### Rename Table
```
ALTER TABLE information
RENAME TO new_info;
```
#### Rename just a column
```
ALTER new_info
RENAME COLUMN person TO people;
```
#### Removing constrains from a table
```
INSERT INTO new_info(title)
VALUES
('some new title')
```
**ERROR**:
_null value in column "person" of relation "new_info" violates not-null constraint
DETAIL:  Failing row contains (1, some new title, null).
SQL state: 23502_

**FIX:**
```
ALTER TABLE new_info
ALTER COLUMN person DROP NOT NULL;
```
### DROP Table
Allows for the complete removal of a column in a table.
```
ALTER TABLE table_name
DROP COLUMN col_name;
```
### Drop multiple columns
```
ALTER TABLE table_name
DROP COLUMN col_one,
DROP COLUMN col_two;
```
### When you are calling the same query yet again
```
ALTER TABLE new_info
DROP COLUMN IF EXISTS person;
```
**NOTICE:**  column "person" of relation "new_info" does not exist, skipping
ALTER TABLE

Query returned _successfully_ in 36 msec.
### CHECK Constraint
Allows us to create more customized constraints that adhere to a certin condition.

Such as making sure all inserted integer values fall below a certain threshold.

```
INSERT INTO employees (
    first_name, 
    last_name,
    birthdate,
    hire_date,
    salary) 
VALUES (
    'Jose',
    'Portilla',
    '1899-11-03',
    '2010-01-01',
    100
);
```
**ERROR:**  _new row for relation "employees" violates check constraint "employees_birthdate_check"
DETAIL:  Failing row contains (1, Jose, Portilla, 1899-11-03, 2010-01-01, 100)._

#
## Assessment Test 
Welcome to your final assessment test! This will test your knowledge of the previous section, focused on creating databases and table operations. This test will actually consist of a more open-ended assignment below:

Complete the following task:

Create a new database called "School" this database should have two tables: teachers and students.

The students table should have columns for student_id, first_name,last_name, homeroom_number, phone,email, and graduation year.

The teachers table should have columns for teacher_id, first_name, last_name,

homeroom_number, department, email, and phone.

The constraints are mostly up to you, but your table constraints do have to consider the following:

 We must have a phone number to contact students in case of an emergency.

 We must have ids as the primary key of the tables

Phone numbers and emails must be unique to the individual.

Once you've made the tables, insert a student named Mark Watney (student_id=1) who has a phone number of 777-555-1234 and doesn't have an email. He graduates in 2035 and has 5 as a homeroom number.

Then insert a teacher names Jonas Salk (teacher_id = 1) who as a homeroom number of 5 and is from the Biology department. His contact info is: jsalk@school.org and a phone number of 777-555-4321.

Keep in mind that these insert tasks may affect your constraints!

### Solutions to Assessment Test

To create the database  simply right-click on the databases drop down menu and select "New Database".

Then you can use the following SQL scripts to execute the tasks:

To create the students table:
```
CREATE TABLE students(
student_id serial PRIMARY KEY,
first_name VARCHAR(45) NOT NULL,
last_name VARCHAR(45) NOT NULL, 
homeroom_number integer,
phone VARCHAR(20) UNIQUE NOT NULL,
email VARCHAR(115) UNIQUE,
grad_year integer);
```

To create the teachers table:
```
CREATE TABLE teachers(
teacher_id serial PRIMARY KEY,
first_name VARCHAR(45) NOT NULL,
last_name VARCHAR(45) NOT NULL, 
homeroom_number integer,
department VARCHAR(45),
email VARCHAR(20) UNIQUE,
phone VARCHAR(20) UNIQUE);
```
Then for inserting the student information:
```
INSERT INTO students(first_name,last_name, homeroom_number,phone,grad_year)VALUES ('Mark','Watney',5,'7755551234',2035);
```

Then for inserting the teacher information:
```
INSERT INTO teachers(first_name,last_name, homeroom_number,department,email,phone)VALUES ('Jonas','Salk',5,'Biology','jsalk@school.org','7755554321');
```
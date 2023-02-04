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







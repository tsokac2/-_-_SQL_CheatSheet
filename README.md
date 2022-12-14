<h1 align="center">SQL AND DATABASES</h1>

### ERRO  - ER_NOT_SUPPORTED_AUTH_MODE: Client does not support authentication protocol requested by server; consider upgrading
### FIX: ``alter user 'root'@'localhost' identified with mysql_native_password by 'your-password';``

### SQL
Structured Query Language<br/>
Attributes: the columns or fields of a table are termed as Attributes.

### Types of Databases
#### Hierarchical Database
Data is organized in the parent-child relationship.

#### Network Databases
Hierarchical Database with a major tweak - the child records are given the freedom to associate with multiple parent records.

#### Object-Oriented Database
Information stored in a database is capable of being represented as an object which responds as an instance of the database model.

#### Relation Databases
Every piece of information has a relationship with every other piece of information
Primary key - every row in the database is linked with another row
Foreign key - every table is linked with another table using a foreign key.

### NoSQL Database
- No-relation database
- Advantages of NoSql
- High scalability and high availability

### Disadvantages of NoSql
- Is an open-source database
- GUI is not available - Graphical User Interface
- Backup is weak - MongoDB
- Large document size

### Primary key and foreign key in a relational database?
* **Primary key** ensures unique row identification in a table.
* **Foreign key** is reference of a **_primary key_** in another table.
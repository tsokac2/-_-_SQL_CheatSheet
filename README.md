<h1 align="center">SQL AND DATABASES</h1>


# PostgreSQL - Windows Installation

### Step 1: Download [PostgreSQL](https://www.postgresql.org/)
[Direct Link](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)

### Step 2: Download and Install [PgAdmin](https://www.pgadmin.org/)

[Direct Link](https://www.pgadmin.org/download/pgadmin-4-windows/)

Download ``pgadmin4-6.19-x64.exe``

### Step 3: Download (BUT DO NOT DIRECTLY OPEN) ``*.tar`` file
Is a database backup file

### Step 4: Restart your computer

### Step 5: Restore the Database (_ignore Failed Exit Code if it appears_)
* Open pgAdmin
* Enter your PostgreSQL password
* Right-click on ``PostgreSQL`` - ``Create`` - ``Database``
* Right-click on new created database and select - ``Restore``

#### If you are facing this error do the following:

* Open up pgAdmin4 and go to:
``File > Preferences > Paths > Binary paths > PostgreSQL Binary Path``
Then all you need to do is manually type out the file path to the bin file location.

* If you are using Windows then provide path like, ``C:\Program Files\PostgreSQL\13\bin``

#### If not continue with next steps:
* Add the path for ``dvdrental.tar`` file

* Under ``Data/Objects``  - ``Sections`` turn on - ``Pre-data``, ``Data`` and ``Post-data``

* If the process is successful - right click on new creates the database and ``Refresh``

* Right-click on the database and select ``Query Tool``

* Write this query ``SELECT * FROM table_name;``

#
## MySQL Workbench RESET Password

#### ER_NOT_SUPPORTED_AUTH_MODE: Client does not support authentication protocol requested by server; consider upgrading

### FIX - create restorSqlPass.txt file with following: 
```
alter user 'root'@'localhost' identified with mysql_native_password by 'your-password';
```

#
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
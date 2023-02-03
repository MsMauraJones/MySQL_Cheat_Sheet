# MySQL Cheat Sheet - Maura Jones
---


## Login from terminal

  1. SSH into your server
  2. Enter the following after the > symbol: mysql -u [username] -p and then hit enter
  3. Enter your password and hit enter
  4. A new prompt line should appear: mysql>

  The above assumes you have created a username and password already with appropriate permission to access the    databases you need - if this is not the case you may need to log in as root user. 
  
  


## SHOW USERS

Use the following query to show all MySQL users created in the database server: 

  SELECT user FROM mysql. user; 



## CREATE USERS

To create a user, enter the following:

  CREATE USER 'new_user'@'localhost' IDENTIFIED BY 'password';

After a new user has been created, you will need to determine what permissions the user will need. 



## PRIVILEGES

1. Grant a user all permissions (equivalent to root):

    GRANT ALL PRIVILEGES ON * . * TO 'new_user'@'localhost';
    

2. The following are examples of granting a user specific permissions:

    GRANT ALL PRIVILEGES ON 'database_name'.* TO 'user_name'@'localhost'.      

    GRANT CREATE, SELECT ON * . * TO 'user_name'@'localhost';
    

3. To show the privileges granted for a specific user:

    SHOW GRANTS FOR 'web'@'localhost';


It is easiest to review the privileges for all users through phpAdmin or Workbench


4. Removing all privileges:

    REVOKE ALL PRIVILEGES ON * . * FROM 'new_user'@'localhost';
    

5. Removing specific privileges: 

    REVOKE CREATE, SELECT ON database_name.table_name FROM ‘user_name’@‘localhost’;



## DATABASES


1. Display all databases:

    SHOW DATABASES;
    

2. Delete a specific database:

    DROP DATABASE database_name;

3. Create a new database:

    mysql> CREATE DATABASE database_name;

4. Select a database to use: 

After a database has been created, you need to confirm that you wish to use that specific database for subsequent operations.  

    mysql> USE database_name;


## TABLES


Please note: 

Tables hold the data inside a database. You need to have selected the database first before you can proceed. 

1. Create a new table:

At the most basic structure, a table has at least one column with a specific data constraint, as below: 

    CREATE TABLE table_name (
        column1 INT
    );

The synatx for creating a table with multiple columns and different data constraints, as below: 

    CREATE TABLE table_name2 (
        column1 INT,
        column2 VARCHAR(#),
        column3 DATE
    );

2. Data 

    2.1: Data Types
    
    When creating a table, each column is required to have a data type. Commonly, the data will be a string, numeric or date/time. 

    Some common examples:

    |  Constraint  |  Type       |  Notes                                     | 
    |  ----------  |  ---------  |  ----------------------------------------  |
    |  INT         |  Numeric    |  Stores numeric values                     |
    |              |             |  Range of -2147483648 to 2147483647        | 
    |              |             |                                            |
    |  VARCHAR(#)  |  String     |  Stores variable-length strings            |
    |              |             |  Syntax: include max # in brackets         | 
    |              |             |  Range of up to 65,535 characters          | 
    |              |             |                                            |
    |  DATE        |  Date/time  |  Stores date values in YYYY-MM-DD format   |
    |              |             |                                            |
    |  DATETIME    |  Date/time  |  Stores combined date and time values      | 
    |              |             |  Format: YYYY-MM-DD HH:MM:SS               |

    2.2: Data Constraints 

    Constraints allow you to set up additional rules for data that can be different in each column.

    UNIQUE - Each field in a column must have a different value
    NOT NULL - Fields cannot have null values
    PRIMARY KEY - Both unique and not null. Only one per table 
    FOREIGN KEY - Identifies data connected to another table

    AUTO_INCREMENT - automatically assigns a value to a field when a new record is added to a table. Only available for numeric data types. 

    2.3: Constraints - Primary Key and Foreign Key: 

        2.3.a PRIMARY KEY 
        
        A primary key is a column (or columns) that can uniquely identify each row in a table. By definition, each value must be unique in the column and cannot contain NULL.

            Example: Parent Table

            CREATE TABLE recipe_categories (
                category_id INT NOT NULL AUTO_INCREMENT,
                category_name VARCHAR(100) NOT NULL,
                PRIMARY KEY (category_id)
            );

        2.3.b FOREIGN KEY

        A FOREIGN KEY is a column (or columns) in one table that refers to the PRIMARY KEY in another table. This constraint is used to preserve consistancy within a database by ensuring that data used in both tables is handled consistantly.

            Example: Child Table

            CREATE TABLE recipes (
                recipe_id INT NOT NULL AUTO_INCREMENT,
                recipe_name VARCHAR(100) NOT NULL,
                serving INT,
                calories INT,
                PRIMARY KEY (recipe_id),
                FOREIGN KEY (category_id) REFERENCES recipe_categories (category_id)
            )
        
        2.3.c ONE-TO-MANY RELATIONSHIP

        A single record from one table can be linked to multiple rows in another table 

        In the example above the receipe_categories table and receipes table are linked by the key column category_id. There can be only one category_id value per row in the parent table, but many rows can have the same category_id value in the recipes table.

    2.4 Show the schema of a table

    To see a description of the table structure enter the following: 
    
        DESCRIBE tableName;

    2.5 Updating/Altering Tables

        2.5.a Add a new column (defaults to the end of the table)

            ALTER TABLE table_name ADD column_name data_type constraints; //defaults to the end of the table

            ALTER TABLE table_name ADD newColumn constraints AFTER existingColumn; //specifying a location

         NOTE: Unless you specify another default value, this will result in a NULL entry in the new column for all previously existing rows

        2.5.b Modify an existing column

            ALTER TABLE table_name MODIFY column_name column_definition AFTER column_name; //Changing column position

            ALTER TABLE table_name ADD CONSTRAINT (column_name,...);// Adding a constraint

    2.6 Index

    An index can help MYSQL undertake faster searches by specifying the column values with which to start the search in the database; however it can make it more challenging to update a table. 

        2.6.a CREATE INDEX

        CREATE INDEX indexName ON tableName(columnName);

        2.6.b REMOVE INDEX

        ALTER TABLE tableName DROP INDEX indexName;

    2.7 Drop Table

    To remove an existing table in a database enter the following:

    DROP TABLE tableName;

## RECORDS AND ROWS -

1. Inserting

    New records can be added to a table with the INSERT INTO statement

    1.1 Single field

    INSERT INTO table_name (columnName) VALUES (newValue);

    1.2 Multiple specific fields

    INSERT INTO table_name (columnNameB, columnNameD) VALUES (newValueB, newValueD);

    1.3 All fields in a row of a table

    INSERT INTO table_name (newValueA, newValueB, newValueC, newValueD);

2.  Selecting

    The SELECT statment allows you to view the records in a table

    2.1 Select all records

    SELECT * FROM table_name;

    2.2 Select records from specific columns

    SELECT columnNameA, columnNameD FROM table_name;

3.  Select records with specific conditions

    Including the WHERE clause in a select statement allows for filtering through the records in a table

    3.1 The synatx:

        SELECT columnName FROM table_name WHERE columnName operator condition;
    
    3.2 Operators for Numeric Fields 

    The following operators can be used with the WHERE clause for numeric values:

    =   Equal
    >   Greater than
    <   Less than
    >=  Greater than or eqal
    <=  Less than or equal
    
    Example: 
    
    SELECT * FROM Products WHERE Price > 30; // This would list all the enteries from the products table where the price was greater than 30.

    3.3   Operators for Numeric Fields - Range (BETWEEN)

    To select records with a numeric value that falls within a certain range use the following syntax:

    SELECT * FROM table_name WHERE columnName BETWEEN value1 AND value2;

    3.4 Operators for Text Fields

    The following operators can used with the WHERE clause for text:

        3.4.a = Equal 
        
        For an exact text match, remember to use single quotes around the values

        SELECT * FROM table_name WHERE columnName = 'exactText';

        3.4.b BETWEEN

        To select the records in an alphabetical range use the following syntax:

        SELECT * FROM table_name WHERE columnName BETWEEN valueC AND valueH;

        3.4.c LIKE

        This operator can be used with WHERE to search for a pattern (e.g. all values that start with a certain letter or finish with a certain letter or include a certain series of letters).

        SELECT column1, column2 FROM table_name WHERE columnN LIKE pattern;

        To describe the pattern, there are two wildcards:

            The percent sign, % , used to represent any characters
            The underscore sign, _ , used to represent a single character
        
        They can be used in combination or separately.

        Exmaples:

        All values that start with m

            WHERE columnName LIKE 'm%';

        All values that end with m

            WHERE columnName LIKE '%m';

        All values that include 'ive'

            WHERE columnName LIKE '%ive%';

        3.4.d IN

        This operator can give multiple values for a WHERE clause

        SELECT * FROM table_name WHERE columName IN (value1, value2);

        3.4.e IN (SELECT STATEMENT)

        The IN operator can also allow for further filtering with a select statement 

        SELECT * FROM table_name WHERE columnName IN (SELECT columnName FROM table_name);
    
    3.5 ORDER BY

    Adding this clause to a SELECT statement allows control over if the results are presented in ascending or descending order

        3.5.a Syntax - Ascending:

        SELECT columnName FROM tableName ORDER BY columnName; //Ascending is default

        3.5.b Syntax - Descending:

        SELECT columnName FROM tableName ORDER BY columnName DESC; 

        3.5.c Syntax - Multicolumn 

        SELECT * FROM tableName ORDER BY columnName1 ASC, columnName2 DESC;
    
    3.6 DISTINCT

    This clause removes duplicate values from the results of the SELECT statement

        3.6.1 Syntax

        SELECT DISTINCT columnName FROM tableName;
    
    3.7 CONCAT

    This clause adds two or more expressions together for the results. 

        3.7.1 Syntax 

        SELECT CONCAT (columnA, " ", columnB, " ", columnC) AS newColumn FROM tableName; //the " " adds space between for readability if needed

    3.8 INNER JOIN

    This clause displays results with the same value in related columns

        3.8.1 Syntax

        SELECT columnA, columnB FROM table1 INNER JOIN table2 ON table1.columnA = table2.columnB;

        3.8.2 Syntax - multiple tables 

        SELECT table1.columnA, table2.columnAA, table3.columnAAA
        FROM ((table1
        INNER JOIN table2 ON table1.columnAA = table2.columnAA)
        INNER JOIN table3 ON table1.columnAAA = table3.columnAAA);

4. Updating existing rows

    4.1 Syntax: 

        UPDATE table_name SET column1_name = value1, column2_name = value2,...WHERE condition;
    
        REMEMBER: If you don't include the WHERE it will update all the records in the columns in the SET portion of the statement!

5. Deleting rows

    5.1 Syntax:

        DELETE FROM table_name WHERE condition; 

        REMEMBER: If you don't include the WHERE it will delete all the records from the table!




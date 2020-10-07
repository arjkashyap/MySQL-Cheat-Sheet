<!-- A quick cheat sheet of all relevant SQL queries and examples on how to use them.  -->

# A Brief Introduction to SQL

SQL is a standard language for storing, manupulating, and retrieving data in relational database systems (RDMS). Some examples of such RDMS are:

- MySQL
- Oracle
- MS Access
- Postgres

After installing MySQL on your system, make sure that your enviroment variables are all set. To boot MySQL, open your standard command line terminal and enter.

```sql
$ mysql -u root -p
```

Here root, specifier the username with which you want to login into your database. You will need to enter your password after this. You can also use MYSQL workbench instead of terminal. This makes your workflow bettter.

---

## Table of Contents

1. [Users](#users)
   - Creating a new user
   - Setting Permissions
2. [Database](#database)
   - Creating DB
   - See existing databases
   - Selecting and using a database
3. [Tables](#tables)
   - Creating a table
   - Delete table
   - Data types

---

<a name="users"></a>

## 1. Users

### Creating A New User:

After loging into your sql shell, you can create a new user by the following command

```sql
    CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
```

Managing your database as a root user is a very bad practise and can be dangerous when you're dealing with sensitive enterprise data, so it is always recomended to first create a new user (from root) and specify the access roles and permissions for this user.

### Grant permissions:

After creating your new users, you would like to assign some permissions. To keep this cheat sheet simple, we will grant all permissions to this user. Please note that, granting all permissions to a user is a bad practise, but we will let this slip for the purpose of this tutorial. We will use the following queries:

```sql
    GRANT ALL PRIVILEGES ON * . * TO 'username'@'localhost';
    FLUSH PRIVILEGES;
```

The Query for grant permissions is followed by flush privileges because it reloads the grant tables in mysql database. This enables the effects to take place instantly wihtout the need to restart server.

### Show permissions for specific user

```sql
    SHOW GRANTS FOR 'username'@'localhost';
```

<a name="database"></a>

## 2. Database

Now we'll get started with some standard database queries. But first we need to create a database. It should be clear by now that in RDMS, data is stored in form of tables in a database.

### Creating a new database

Lets create a new database by the name of test so we can do some experiments on it.

```sql
    CREATE DATABASE test;
```

We create a new database with the **_CREATE DATABASE_** clause. The name of the database should be unique on your server. To see the list of databases on your server you can use the **_SHOW DATABASES_** command, which will query us a list of available databases.

```sql
    SHOW DATABASES;

    +--------------------+
    | Database           |
    +--------------------+
    | test              |
    | information_schema |
    | mysql              |
    | performance_schema |
    | sys                |
    +--------------------+
```

### Using a database

Now to perform queries, You have to **_USE_** the database which you want to work on.

```sql
    USE test
```

<a name="tables"></a>

## 3. Tables

Now we are ready to create our first database table. We will call it users, and it will store the information such as id, name, address etc.

### Creating a new Table

The syntax for creating a new table follows: <br/>

CREATE TABLE table_name( <br/>
column_1_definition, <br/>
column_2_definition, <br/>
..., <br/>
table_constraints <br/>
) <br/>

To explain it a little further, **_CREATE TABLE_** clause is followed by the name of the table, which takes definitions of each column. The columns definitions are seperated by space. <br/>

The following is the syntax for Column name definitions:<br/>

column_name data_type(length) [NOT NULL] [DEFAULT value] [AUTO_INCREMENT];

- data_type as the name suggests, specifies the which type of data is stored in this column. Ex: VARCHAR9255

- **_NOT NULL_** constrain makes sure that the column value is not null.

- **_DEFAULT value_** can be used to assign a default value for the calumn.

- **_AUTO_INCREMENT_** is used when you want to assign a value to the column automatically by incrementing its value whenever a new row is added to the table.

Let us get back to our test database and create a users table.

```sql
    CREATE TABLE users(
        id INT AUTO_INCREMENT,
        first_name VARCHAR(100),
        last_name  VARCHAR(100),
        email VARCHAR(75),
        house_address VARCHAR(255),
        register_date DATETIME,
        PRIMAY KEY(id)
    );
```

Primary key is a field added to the end of the column defination which specifies the column that should be treated as pk. In this case we are treating our user id as a primary case and want to specify an **_AUTO_INCREMENT_** clause so that it is automatically assigned to every user.

> > Make sure that your primary key is a unique value

Now you can use the **_SHOW TABLES_** command to view tables in your db.

### Delete a table

You can use the same **_DROP_** command that we used earlier with the database to delete a table.

```sql
    DROP users
```

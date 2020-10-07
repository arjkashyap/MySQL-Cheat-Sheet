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

Here root, specifier the username with which you want to login into your database. You will need to enter your password after this.

## Table of Contents

1. [Users](#users)
   - Creating a new user
   - Setting Permissions
2. [Database](#database)
   - Creating DB
   - See existing databases
   - Selecting and using
3. [Tables](#tables)
    - Creating a table
    

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

## 2. Database & Tables

Now we'll get started with some standard database queries. But first we need to create a database. It should be clear by now that in RDMS, data is stored in form of tables in a database.

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

Now to perform queries, You have to **_USE_** the database which you want to work on.

```sql
    USE test
```

<a name="tables"></a>

## 3. Database & Tables
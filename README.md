# A Brief Introduction to SQL [[Part I]]

SQL is a standard language for storing, manipulating, and retrieving data in relational database systems (RDMS). Some examples of such RDMS are:

- MySQL
- Oracle
- MS Access
- Postgres

After installing MySQL on your system, make sure that your enviroment variables are all set. To boot MySQL, open your standard command line terminal and enter.

```sql
$ mysql -u root -p
```

Here root, specifier the username with which you want to login into your database. You will need to enter your password after this. You can also use MYSQL workbench instead of terminal. This makes your workflow better.
<br/>

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
   - Insert Data row/record
4. [Querying Data](#querying)
   - The SELECT statement
   - Where Clause
5. [Delete & Update](#delupdate)
   - Delete Operation
   - Update row data
6. [Modify Table Values](#alter)
   - Adding new column to the table
   - Update column
7. [The Select Statement](#select)
   - Querying in ascending or descending order
   - Concatenating Columns
   - Ranges
   - Like
8. [Foreign Key](#foreignkey)
   - What is foreign key ?
   - Creating a table & setting foreign key.
9. [Join Clause](#join)
   - What are Joins
   - Inner Join
   - Left & Right Join
10. [Aggregate Functions](#aggregate)

   - Count
   - Max
   - Min
   - Sum
   - Cases

---

<a name="users"></a>

## 1. Users

### Creating A New User:

After loging into your sql shell, you can create a new user by the following command

```sql
    CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
```

Managing your database as a root user is a very bad practice and can be dangerous when you're dealing with sensitive enterprise data, so it is always recomended to first create a new user (from root) and specify the access roles and permissions for this user.

### Grant permissions:

After creating your new users, you would like to assign some permissions. To keep this cheat sheet simple, we will grant all permissions to this user. Please note that, granting all permissions to a user is a bad practice, but we will let this slip for the purpose of this tutorial. We will use the following queries:

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

- **_DEFAULT value_** can be used to assign a default value for the column.

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

Primary key is a field added to the end of the column definition which specifies the column that should be treated as pk. In this case we are treating our user id as a primary case and want to specify an **_AUTO_INCREMENT_** clause so that it is automatically assigned to every user.

> > Make sure that your primary key is a unique value

Now you can use the **_SHOW TABLES_** command to view tables in your db.

### Delete a table

You can use the same **_DROP_** command that we used earlier with the database to delete a table.

```sql
    DROP users
```

### Inserting data into Table

So now we have created our first table and inserted columns attributes in it. Now all we need is some data. To insert data into MySQL we use the **_INSERT_** statement . <br/>

The Insert statement allows you to insert one or more rows into the table. The syntax for Inserting data rows is as follows: <br/>

INSERT INTO table_name(c1, c2, c3, . . . ) <br/>
VALUES (v1, v2, v3 . . .); <br/>

We have to make sure that the column names we specify match with our data entry. For our users table, we will insert data as follows:

```sql
    INSERT INTO users(first_name, last_name, email, house_address, register_date)
    VALUES ('John', 'Doe', 'johndoe@gmail.com', '26 second street NY', now());
```

In our above example, we have not inserted the id filed because we have specified it to be **_AUTO_INCREMENT_** which means it will be auto generated and incremented in each row.
**_now()_** is a function which is used to get the current date and time.

### Inserting Multiple rows

Inserting Multiple rows is same as inserting a single record, the only difference is that the values are seperated by commas.

```sql
    INSERT INTO users(first_name, last_name, email, house_address, register_date)
    VALUES ('Nathan', 'Dave', 'nateDave@gmail.com', '296/9 Dummy Lane', now()),
    ('Jane', 'Doe', 'jane@gmail.com', 'Rhode Island', now()),
    ('Netalie', 'Reeves', 'nat767@gmail.com', 'new hemisphere', now()),
    ('Shane', 'Borne', 'shanebore@gmail.com', 'Boston', now()),
    ('Johanthan', 'Cooper', 'johncooper123.com', 'Rhode Island', now()),
```

<a name="querying"></a>

## 4. Querying Data

We have already used the **_SELECT_** statement for viewing our table entries.

```sql
    SELECT * FROM users
```

Here, \* represents the wild card known as all. The above statement specifies, select ALL from the users table. <br/>

We can instead use the names of column to display the relevant information. Suppose you want to see only the first and the last name from the users table:

```sql
    SELECT first_name, last_name FROM users;

    +------------+-----------+
    | first_name | last_name |
    +------------+-----------+
    | John       | Doe       |
    | Nathan     | Dave      |
    | Jane       | Doe       |
    | Netalie    | Reeves    |
    | Shane      | Borne     |
    | Johanthan  | Cooper    |
    +------------+-----------+
```

### Where Clause

The **_WHERE_** clause comes in handy, when you want to filter your queried data based on some specific attributes. <br/> It is used to specify a condition while fetching the data from a table.
You can mix select statement with where to make some very powerful query filter.
Suppose I want to filter the first_name, email and the house_address of the users who live on Rhode Island

```sql
    SELECT first_name, email, house_address FROM users WHERE house_address="Rhode Island";
    +------------+-------------------+---------------+
    | first_name | email             | house_address |
    +------------+-------------------+---------------+
    | Jane       | jane@gmail.com    | Rhode Island  |
    | Johanthan  | johncooper123.com | Rhode Island  |
    +------------+-------------------+---------------+
```

Not only this, we can add multiple conditions. Say we want to query users who are from Rhode Island and named Jane.

```sql
        SELECT first_name, email, house_address FROM users WHERE house_address="Rhode Island" AND first_name="Jane";
    +------------+-------------------+---------------+
    | first_name | email             | house_address |
    +------------+-------------------+---------------+
    | Jane       | jane@gmail.com    | Rhode Island  |
    +------------+-------------------+---------------+
```

You can use all kinds of operators here, such as **_<_** or **_>_** to filter results. We will cover these operators later.

<a name="delupdate"></a>

## 5. Delete & Update row data

### Deletion

Deletion can be done with the help of the **_DELETE_** clause. Mostly, it is used with the **_WHERE_** statement in reference to the primary key. In the below example we will delete a user with the primary key 5.

```sql
    DELETE FROM users WHERE id=5;
```

Now when you run **_SELECT _ FROM users;\***, you will observe that the user with primary key 5 is no longer in the table.

> > Please note that deletion is a very delicate operation. Be sure to use the WHERE clause, otherwise you may endup deleting the whole table.

### Update info

Update is one of the most frequent operations which is performed on a database. It can be undertaken with the help of the **_UPDATE_** and **_SET_** clause. The basic syntax for updating an entry is as follows: <br/>

UPDATE table_name
SET column1 = value1, column2 = value2...., columnN = valueN
WHERE [condition];

Lets say that Nathan had a typo in his email address and wants to change it.

```sql
    UPDATE users SET email="nathandave@gmail.com" WHERE id=2;
```

<a name="alter"></a>

## 6. Modify Table

### Add new column

Let us add an **age** column to our table.

```sql
    ALTER TABLE users ADD age VARCHAR(3);
```

In the above query, we added a new column by the name **_age_** to our table and we set its datatype as a varchar with max size as 3. Our new column will be appended to the users table and the default value for all the rows will be set to **_NULL_**. <br/>

But wait are we forgetting something ? <br/>

### Update column

Most of the readers must have realised by now that Age column is supposed to be a number not a string. So we were supposed to set it as an **_INT_** not a **_VARCHAR_**. Lets fix it. <br/>

```sql
    ALTER TABLE users MODIFY age INT(3);
```

> > Note: The number 3 in the parenthesis specifies the maximum width of the
> > that can be added, not the max value. Here a three digit number can be
> > specified.

<a name="select"></a>

## 7. The Select Statement

There is a lot we can do with the **_Select Statement_**. I Will try to cover some important clauses that can be used to query data.

### Query in Ascending or Descending Order

In our users table, we have just added an age column. Lets say we want to query results in ascending order of the age. Here we can use **_ASC_**.

```sql
    SELECT * FROM users ORDER BY age ASC;
```

To items in descending order, we can use DESC;

### Concatenating Columns

We have two seperate columns by the name, first_name and last_name. For some reason we require to query them as a single column 'name'. We can use the **\*CONCAT** clause with select.

```sql
    SELECT CONCAT(first_name, ' ', last_name) AS 'name', email, age FROM users;
```

What we are doing here is, we are Concatenating the two columns, first*name and last_name and setting them \_as* 'name' column. We want to query email, age aside from our newly created 'name' column.

At this point, you might have started putting the various queries together.
Lets arrange the above query in descending order.

```sql
    SELECT CONCAT(first_name, ' ', last_name) AS 'name', email, age
    FROM users ORDER BY age DESC;
```

### Ranges

Range as the name tells, queries results between upper and a lower bound range. We use the **_BETWEEN_** clause for setting up an upper and lower limit of the range. It is a very usefull util. Say we want to query users whose age is within 20 to 30.

```sql
    SELECT * FROM users WHERE age BETWEEN 20 AND 30;
```

Let us start stacking what we have done for select statements so far. When you use all these statements together, you start to understand how much you can refine your query results.

```sql
    SELECT CONCAT(first_name, ' ', last_name) as 'Name', email, age
    FROM USERS WHERE age BETWEEN 20 AND 30 ORDER BY age DESC;
```

### Like

The **_Like_** clause is used for full text search. This is generally used when you are making a search box. Example searching for a blog post for articles with specific names.<br/>

Suppose we want to search the user whose name starts with "j". We can write:

```sql
    SELECT * FROM users WHERE first_name LIKE 'j%';
```

We have the expression **_j%_** followed by like key word. This means that we are looking for text where j is followed by any string i;e j should be the first letter of the name. It will query us all the results whose first_name start with the letter "j". <br/>

The **_LIKE_** clause is an extreamly usefull expression. We can make anykind of expression for searching using this. Below are some examples:

- Query users who have account in **gmail**.

  ```sql
      SELECT * FROM users WHERE email LIKE '%gmail.com';
  ```

- Query users who have the letter t in between their last name
  ```sql
      SELECT * FROM users WHERE last_name LIKE "%t%";
  ```

We can also do **_NOT LIKE_** which can be used to query results which don't match the specified expression

```sql
    SELECT * FROM users WHERE first_name NOT LIKE 'j%';
```

<a name="foreignkey"></a>

## 8. Foreign Key.

### What is Foreign Key ?

> > In a nutshell, the whole point of foreign keys is to allow you to reference one table from antoher

To explain the concept of foreign key in detail, we will take an example. Suppose we are building a blogging site where users can add posts. We have to break the whole database of this website into different tables to maintain order. <br/>

Now aside from the users table (which we have already created), there would a table by the name **Posts** which will store the posts made on the site. This table will have attributes such as

- post_id
- title of post
- body (content)
- date
- user who made the post

So do you see any relation between the user table and the post table ?

The table, posts bears a direct relation with the users table in respect to the fact that each row in the table posts, will belong to a user (which resides in users table). Foreign key is a way to specify this relation. In our posts table, we create a column, **_user_id_** and set it as foregin key.<br/>

**_The value of this filed will be equal to the id of the user in the table, who made this post_**

<br/>
Now let us actually perform these steps in our database.<br/>
First we need to create a new table called posts with a foreign key refering to users table.

```sql
    CREATE TABLE posts(
        id INT AUTO_INCREMENT,
        user_id INT,
        title VARCHAR(100),
        body TEXT,
        publish_date DATETIME DEFAULT CURRENT_TIMESTAMP,
        PRIMARY KEY(id),
        FOREIGN KEY(user_id) REFERENCES users(id)
    );
```

The way we define a foreign key is: **_FOREIGN KEY(column_name) REFERENCES table_ref(ref_table_column_name)_** <br/>
So Now our user_id column refers to the users table -> id section. <br/>

Now I'll quickly add some data to our newly created table.

```sql
    INSERT INTO posts(user_id, title, body) VALUES
    (1, 'Post One', 'This is post one'),
    (3, 'Post Two', 'This is post two'),
    (1, 'Post Three', 'This is post three'),
    (2, 'Post Four', 'This is post four'),
    (4, 'Post Six', 'This is post six'),
    (2, 'Post Seven', 'This is post seven'),
    (1, 'Post Eight', 'This is post eight'),
    (3, 'Post Nine', 'This is post none'),
    (4, 'Post Ten', 'This is post ten');
```

<a name="join"></a>

## 9. Join Clause

A relational database consists of multiple related tables linking together using common columns which are known as Foreign key. For example, our users table is linked to the posts column by user_id column in post table.
<br/>

> > I found a really cool video on YouTube by the channel <a href="https:/www.youtube.com/channel/UCW6TXMZ5Pq6yL6_k5NZ2e0Q">Socratica</a> which has explained Joins in Sql in a very fun to watch manner. Here is the link: <br/> > > https://www.youtube.com/watch?v=9yeOJ0ZMUYw

### Inner Join

Suppose we would like to see the posts made by users along with the users detail. These two pieces of info lie in two different tables linked by a single column. This is where we use **_Joins_**.

```sql
    SELECT
    users.first_name,
    users.last_name,
    users.email,
    posts.title
    posts.publish_date
    FROM users
    INNER JOIN posts
    ON users.id=posts.user_id
    ORDER BY posts.title;
```

The **_ON_** clause is used to specify the common column in respect to which we have to find and display data from both table. We can even add some search filter using **_WHERE_** clause in the above query.

### Left Join

The LEFT JOIN allows you to query data from two or more tables. Similar to the INNER JOIN clause, the LEFT JOIN is an optional clause of the SELECT statement, which appears immediately after the FROM clause.

### Right Join

MySQL RIGHT JOIN is similar to LEFT JOIN, except that the treatment of the joined tables is reversed.

<a name="aggregate"></a>

## 10. Aggregate Functions

### Count

We can use the count clause to count the number of entries a specific column in the table. We can count the total entries by simply counting the id column.

### Max

Max function returns the maximum from the column.

### Min

Min returns the minimum from the column.

### Sum

Returns Sum of the column.

```sql
SELECT COUNT(id) FROM users;
SELECT MAX(age) FROM users;
SELECT MIN(age) FROM users;
SELECT SUM(age) FROM users;
```

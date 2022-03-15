Database Basics
===============

We’ll start by covering the very basics of databases:

What is a database?
-------------------

Databases are essentially the storage shelves of the digital world, we can put information in them, from a couple of entries to absolutely enormous files that contain user information on a global scale for example.

How do we manage databases?
---------------------------

Every database has a **Database Management System (DBMS)** which structures how we organise and interact with all of our stored data.

DBMS’s come in two different styles.

### Relational DBMS’s

* Relational databases store data in tables made of columns and rows, very similar to what we see in Excel, or more accurately, MS Access.
* The columns are the categories, and each row inside them holds a data entry.
* These columns have very strict data types, the data stored in that row **must** be of the same data type or this will not work.
  * This enforcement of strict data type’s makes it very good for handling complex data sets.
* How we arrange the columns and rows is what’s known as a **schema**.
* A schema **must **be defined **before** we can store any data in the database

### Non-Relational DBMS’s

* On the other side of the fence, we have non-relational databases - an umbrella term for any kind of database that isn’t strictly relational.
* These types are databases are more flexible systems as they enforce much less strictness around data types.
* Whereas our relational DB is strictly columns and rows, non-relations DB’s come in many forms:
  * Key-Value Pairs
  * Graphs - Quite a complex system, used for efficiently storing entities that have heavy relationships.
    * An example would be purchsasing and manufacturing systems that are hugely intertwined
  * Documents - typically a set of named string fields and object values in a document, usually a JSON.
  * Column family - similar conceptually to a relational database, with a denormalised approach to sparse data.
* As no strict schema is required, these databases can get up and running quickly.
* Great for deployement across decentralised networks.

How do we interact with our stored data?
----------------------------------------

Once again, this falls into the responsibility of our DBMS.

There are 4 fundamental ways we interact with our data, known as **CRUD**:

1. C - Create
2. R - Read
3. U - Update
4. D - Delete

These do exactly what they say on the tin.

### How do we communicate with the database?

* In a relational database, we perform our CRUD actions and queries using the language **SQL (Structured Query Language)**, we can use this language to fulfill all of our needs.
* SQL is a huge topic in and of itself, it’s a little out-of-scope for this course to try and remember *everything* that SQL can do, but we will cover some of the basics.
* Non-relational databases are a different story; they are also known as no-sql databases and use a custom implementation to interact with the database.
  * Often this is simpler to use to begin with, but as the application grows in complexity, it loses out to SQL strict enforcements that help to keep our codebase robust, thus causing more work for the developer.
* At Makers, we will start by using the DBMS open-source software **PostgreSQL**.

SQL
===

The next section will touch briefly on some of the basic tools of SQL and PostgreSQL:

Basic Interactions
------------------

### Getting started

To get a PostgreSQL database up and running, we can do the following:

```sh
1) postrgres psql # start the postgres CLI
2) \c database_name # connect to your database, usually at this step this would be your username
2) CREATE DATABASE database_name; # create the new database
3) psql database_name; # start editing the new database
4) \c database_name; # connect to the specific database
5) CREATE TABLE table_name(column_name COLUMN_TYPE, column_2_name COLUMN_2_TYPE); # create a table with whatever columns and data types you like
```

### Creating data in our table

To perform the first of our CRUD operations:

```sql
INSERT INTO table_name(column_1, column_2)
  VALUES
    (column_1_data, column_2_data),
    (more_column_1_data, more_column_2_data)
  ;
```

### Reading data from our table

This particular CRUD operation has *lots* that could be covered, this is when we get into the world of querying which is enormous, but we will cover some of the very basics:

```sql
/* to read a specific row(s) from a certain column */
SELECT column_name FROM table_name
  WHERE another_column = 'A value'

/* to read data from columns, if the list contains an item. For this example, imagine we have a table of country names and population: */
SELECT name, population FROM countries
  WHERE name IN ('United Kingdom', 'Guatemala'); /* if the database contains the UK and Guatemala it will return their names and population
```

### Updating data in our table

To update a row in our table, we can do the following:

```sql
UPDATE table_name
  SET column_to_update = 'Value to update'
  WHERE any_column_value = identifying_criteria;
```

### Deleting data in our table

To achieve the final operation of CRUD, we use a very similar syntax to updating:

```sql
DELETE FROM table_name WHERE identifying_column = 'Identifying data' /* whichever column you use to identify the row does not matter, it will delete                                                                      the entire row
```

Relationships Between Tables
============================

We’ve spoken about relational databases, but up until now we haven’t really touched on the *relational* aspect of those databases.

Our database can have as many tabes as we would like for example we could have the following user table:

```text
id  | user_name | password |  email        |  city  | postcode |
----+-----------+----------+---------------+--------+----------+
 1  |   adam    |  123456  | adam@adam.com | London | SW1A 1AA |
----+-----------+----------+---------------+--------+----------+
 2  |   nico    |  654321  | nico@nico.com | London |  W1S 1JA |
```

This table looks fine, however it would make more sense for all of the addresses to be stored in a seperate table, and setup a *relationship* between the two:

```text
id  | user_name | password |  email        | address_id |                |  id    |  city  | postcode |
----+-----------+----------+---------------+------------+                |--------+--------+----------+
 1  |   adam    |  123456  | adam@adam.com |     27     |                |   27   | London | SW1A 1AA |
----+-----------+----------+---------------+------------+                +--------+--------+----------+
 2  |   nico    |  654321  | nico@nico.com |     36     |                |   36   | London |  W1S 1JA |
```

### Querying the full user information

There will of course be times when we want to see the entirety of the user’s information; SQL’s inner join can be used to achieve this:

```sql
SELECT * FROM User INNER JOIN Address ON User.address_id = Address.id;
```

* This command joins the 2 tables using the `​address_id`​ field on the user table to match to the `​id`​ of the address table. If there is no address, you would **not see the user**, this exclusively shows data that matches.

What if this example is for an site like AirBNB, and the user has multiple addresses linked to their host account?

### One-to-many relationships

There are thousands of examples where a user would want to store multiple lots of data in another table, I’ve used the address example, but this could be: blog posts, workouts, recipes, notes and so much more. This is when we would use a **one-to-many** relationship, in this example, *one* user linked *to many* different addresses.

To achieve this we would simply have another column on the address table that listed the id of the user in which it belonged to.

We could now query this data as follows:

```sql
SELECT * FROM User LEFT JOIN Address ON User.id = Address.user_id;
```

* The `​LEFT JOIN`​ refers to all of the data on the left-hand side of the query, in this case the user. If there is no matching data on the right we’ll just receive an empty cell, this will let us see all of the users regardless of whether they have any stored addresses or not.

### Many-to-many relationships

Things start to get a little murkier here. Sticking with our Airbnb example, imagine that these are addresses that *guests* have stayed, rather than properties belonging to a host. A guest could stay at many addresses, and that address could have had many guests in it.

Sadly, we cannot use the same principle of adding an additional column, this time to the user, as our tables would end up becoming very, very wide. Instead, we can use a **join table**, this is simply a table that stores the IDs of other tables:

```text
| user_id | address_id |
+---------+------------+
|   1     |     14     |
+---------+------------+
|   2     |     27     |
+---------+------------+
|   1     |     39     |
+---------+------------+
|   2     |     11     |
+---------+------------+
|   2     |     84     |
```

This keeps everything nice and tidy, this table simply sits in the middle of our User and Address tables and stores the many-to-many relationships between them.
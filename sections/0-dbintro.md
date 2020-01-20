[<<< Back](../README.md) - [Next >>>](1-builddb.md)

# Introduction to databases

## What is a database?

A database is a collection of data that is structured to allow for manipulation. There are different kinds of databases—one kind is a relational database. In a relational database, the data is contained in different tables.

## What is SQL?

SQL (Structured Query Language) is a programming language for interacting with data in a relational database. There are different implementations of SQL—one implementation is [SQLite](https://www.sqlite.org/). Different implementations (such as [PostgreSQL](https://www.postgresql.org/) and [MySQL](https://www.mysql.com/)) have their own higher level specialized functions, but they all handle the same basic operations covered today.

We’re going to use SQLite in this session because getting set up requires less work. SQLite is a little different from other implementations of SQL because it operates on regular plain old files and does not require a server connection, unlike PostgreSQL and MySQL. The databases you work with in SQLite exist in `.db` files that you can store anywhere on your computer.

### How do I use SQL?

The database holds your data, but you need some way to interact with the database. We will be using a [command line utility](http://www.sqlite.org/cli.html).

[<<< Back](../README.md) - [Next >>>](1-builddb.md)

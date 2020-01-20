[<<< Back](0-dbintro.md) - [Next >>>](2-buildtable.md)

# Building a database

To get started, we're going to use the `sqlite` tool to create the database file we will be using during this session.

In these sessions, we've been placing files in a `projects` folder on our desktop. Let's create a folder for this session on databases. 

Go to the `projects` directory in your command line and enter these commands:

```bash
mkdir database-practice
cd database-practice
```

This will create a new folder for this session and also move into it using the `cd` command.

Now that you're in the correct folder, start the Sqlite program:

```bash
sqlite3
```

In your program, type this code:

```bash
.open firstdb.sqlite
```

Congratulations! You've successfully created and accessed your database on the command line. This is an excellent first step toward performing data analysis on large data sets or creating your own applications!

By the way, if you ever want to quit out of sqlite, the command is:

```sqlite
.quit
```

[<<< Back](0-dbintro.md) - [Next >>>](2-buildtable.md)

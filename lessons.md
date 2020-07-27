# Introduction to databases

## What is a database?

A database is a collection of data that is structured to allow for manipulation. There are different kinds of databases—one kind is a relational database. In a relational database, the data is contained in different tables.

## What is SQL?

SQL (Structured Query Language) is a programming language for interacting with data in a relational database. There are different implementations of SQL—one implementation is [SQLite](https://www.sqlite.org/). Different implementations (such as [PostgreSQL](https://www.postgresql.org/) and [MySQL](https://www.mysql.com/)) have their own higher level specialized functions, but they all handle the same basic operations covered today.

We’re going to use SQLite in this session because getting set up requires less work. SQLite is a little different from other implementations of SQL because it operates on regular plain old files and does not require a server connection, unlike PostgreSQL and MySQL. The databases you work with in SQLite exist in `.db` files that you can store anywhere on your computer.

### How do I use SQL?

The database holds your data, but you need some way to interact with the database. We will be using a [command line utility](http://www.sqlite.org/cli.html).

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

# Building tables

The next step is to create tables to hold your data. From here on, we are going to be using SQL to execute database queries.

The syntax for creating a table in SQLite is:

```sql
CREATE TABLE table_name ( field_name data_type constraints );
```

- The field name describes some aspect of the record, such as `name`, `id`, or `last_updated_on`.
- The [data type](https://www.sqlite.org/datatype3.html) will affect the behavior of the data in that field. For example, whether the data itself is treated as text or a number. 
- The [constraints](http://www.tutorialspoint.com/sqlite/sqlite_constraints.htm) will affect the behavior of that field. For example, a field with a `NOT NULL` constraint means that each record must have some data in this field.


Let's create a table to store data about academic programs. Name the table "programs" and give it two fields (aka, columns): one for `id`, the other for `program_name`.

Here's the SQL we need:

```sql
CREATE TABLE programs  (
	id INTEGER PRIMARY KEY,
	program_name VARCHAR
);
```
Remember, you can press `enter` at any time to start a new line, without executing the code. A `;` is necessary to finish a line of SQL. SQLite is not case-sensitive. Press `enter` after the `;` to create this table in your SQLite database.

A table is most useful when it represents a category of object you are tracking. A record in a table is one instance of that category, i.e. a book in a collection of books, or a person in a list of people. Each field is an aspect of that record, like the author of a book, or the age of a person. This will become clearer as we continue.

# Inserting data into an SQL table

Now that we have a table structure, we can insert some data. Let's make a table of departments in a school.

The syntax for inserting multiple records is:

```sql
INSERT INTO table_name (field_name) VALUES (record1), (record2), (record3);
```

Insert `Anthropology`, `Biology`, and `Linguistics` into the table we just created. Here is the SQL we need:

```sql
INSERT INTO programs (program_name) VALUES
("Anthropology"),
("Biology"),
("Linguistics");
```

Execute that code in the SQLite REPL. Now you have a database table with data in it! But it's all rather abstract without visible evidence, isn't it? In order to visualize our data, we need to change some settings in the SQLite program:
```sql
 .mode column
 .headers on
```
Now add this code:
```sql
select * from programs;
```
Here is the shape of our table:

```
id          program_name
----------  ------------
1           Anthropology
2           Biology
3           Linguistics
```
If you see the programs we added to the database, it worked! 

## A note about mistakes

Mistakes happen! Constantly, in fact. In Python, fixing your mistake usually is a matter of correcting something in your script and running it again. One of the difficulties of learning SQL is that your mistake may end up entered into the database, and reversing that error is more difficult.

There are processes to remove mistakes in SQL. In a simple table like the one above, if we want to delete a row, we can do this:
```sql
DELETE FROM programs WHERE id = 1;
```
This would delete "Anthropology" from our table. If we want to edit our table instead, we would type:
```sql
UPDATE programs SET program_name = 'Physics' WHERE id = 3;
```
And now we have "Physics" in place of "Linguistics".

As we go through this tutorial, keep these `delete` and `update` commands in mind. But if you find your database has reached a state of complete confusion, don't be afraid of starting over!

# A more readable approach
We've been using pure SQL in our scripts so far. Ah, but there's a more pythonic way of accomplishing our goal. Consider the script below:

```python
# import sqlite library
import sqlite3

# create a database and make a connection.
conn = sqlite3.connect("first.db")
cursor = conn.cursor()

sql = """CREATE TABLE boardgames (
    boardgame VARCHAR,
    length_in_hours INTEGER,
    players INTEGER
)"""

cursor.execute(sql)

conn.commit()

# insert multiple records using the "?" method
boardgames = [('Settlers of Catan', 3, 5),
          ('Settlers of Catan', 3, 5),
          ('Carcasonne', 1, 10),
          ('Diplomacy', 2, 4),
          ]

cursor.executemany("INSERT INTO boardgames VALUES (?,?,?)", boardgames)
conn.commit()
```

What might be the advantage of such an approach? It is cleaner, and after a bit of practice with Python, easier to read. Being more compact, it is easier to take in with one glance.

# Updating Fields

You can alter tables after they've been created. The SQL syntax below adds another field to the existing table and then populates that field with data.

```sql
ALTER TABLE programs    --selects the "programs" table to update
ADD program_level VARCHAR;    --adds a "program_level" column, which is a string
```
By the way, in the example above, the words after the '--' symbols are comments, and don't affect the query. You do not have to include the comments in your code.

Next, add another field for `program_level` to the existing table, using the SQL code above.

Now, let's populate the new empty "program_level" field with some data.

```sql
UPDATE programs		--select the table to update
SET program_level = "Ph.D."		--select the field and value to update
WHERE program_name = 'Linguistics';		--select the condition for updating
```

And now we have:
```
id          program_name  program_level
----------  ------------  -------------
1           Anthropology
2           Biology
3           Linguistics   Ph.D.
```

### Challenge

Update the `program_level` field for `Biology` and `Anthropology` with `Master's`.

Hint: You can do this with one statement using `IN`. The solution is [here](solution1.sql).

# Relating tables with foreign keys

At this point, we're going to create a second table called "students" to illustrate the relational nature of relational databases. We use the same syntax that we used to create the `programs` table, but with one extra element: *a foreign key*.

Create a table called "students" with a field for: (1) a primary key, (2) student name, and (3) a foreign key that will reference the "programs" table. The SQL looks like this:

```sql
CREATE TABLE students (
	id INTEGER PRIMARY KEY,
	student TEXT,
	id_program INTEGER,
	FOREIGN KEY (id_program) REFERENCES programs(id) -- this establishes the reference!
);
```

The foreign key points to a primary key in another table, in this case the `programs` table. This relationship is specified with the clause `FOREIGN KEY (id_program) REFERENCES programs(id)`, which links the "id_program" field in the "students" table to the "id" field in the `programs` table.

All records in the `students` table must point to a valid primary key in the `programs` table.

The last step is to add some data to the new "students" table. Try adding some data on your own (for example, student: 'John Doe', id_program: '3')—if you get stuck, a solution is [here](solution2.sql).

Remember:

- The primary key will be generated automatically.
- The foreign keys must be entered manually 
- You decide which program to associate with each student.

We will make use of the foreign key in the next step.

# Challenge: build a table for GPA!

Now we have a student table that looks something like this:

```
id          student     id_program
----------  ----------  ----------
1           Josefina    3
2           Cecilia     2
3           Nico        2
4           Sarah       1
```

Let's imagine that we want to connect each student to their GPA. How can we do this?

Things to consider:

- Where will we put the GPAs?
- What kind of data type will they be?
- How will we connect each GPA to the correct student?

## Solution 

### Create a New Table For GPAs

```sql
CREATE TABLE gpas (
	id INTEGER PRIMARY KEY,
	gpa DOUBLE PRECISION,
	id_student INTEGER,
	FOREIGN KEY (id_student) REFERENCES students(id)
);
```

You may notice a new term in the above example. `DOUBLE PRECISION` is just a number that can have a decimal point.

### Populate the GPA Table with GPA Score and Foreign Key

```sql
INSERT INTO gpas (gpa, id_student) VALUES
	(2.67, 2),
	(3.9, 1),
	(1.23, 3),
	(4.0, 4);
```

And thus our table for GPAS looks like:
```
id          gpa         id_student
----------  ----------  ----------
1           2.67        2
2           3.9         1
3           1.23        3
4           4.0         4
```

# Querying your database

Now that we have a decent-looking database, we can execute some queries to manipulate our data.

Each query is made up of the same basic set of clauses:

- The `SELECT` clause indicates the fields that you want to return.
- The `FROM` clause indicates the table that the fields belong to.
- The `WHERE` clause filters the results of the query.

Together, these clauses create a new temporary table based on the criteria specified in each one.

Practice executing these queries and see what they return.

1. This query returns all of the records (i.e., rows) in the "students" table:

```sql
SELECT * FROM students;
```
```
id          student     id_program
----------  ----------  ----------
1           Josefina    3
2           Cecilia     2
3           Nico        2
4           Sarah       1
```
2. This query returns only the values in the "student" field for all records in the `students` table:

```sql
SELECT student FROM students;
```
```
student
----------
Josefina
Cecilia
Nico
Sarah
```
3. This query returns two fields from the "students" table:

```sql
SELECT student, id FROM students;
```
```
student     id
----------  ----------
Josefina    1
Cecilia     2
Nico        3
Sarah       4
```
### Challenge

Write a query that returns `program_name` and `program_level` for each record in the `programs` table. A solution is [here](solution3.sql).

In the following query, `WHERE` filters the records by their value in the "id" field:

```sql
# Show all fields for each record in the table 'students' 
# where the value of the 'id' field is equal to '3'
SELECT * FROM students WHERE id = '3'
```
```
id          student     id_program
----------  ----------  ----------
3           Nico        2
```
### Challenge

Write a query that returns entire records for _**only**_ Ph.D programs in the 'programs' table. You can find a solution [here](solution4.sql).

# Joins

Each of the queries up to now has just returned data from a single table in the database. This final query combines our "students" and "programs" tables using the `INNER JOIN` and `ON` clause:

```sql
# Show all the records for students with information 
# about their respective programs
SELECT *
FROM students INNER JOIN programs 
ON students.id_program = programs.id;
```
This query should return what you see below:
```
id          student     id_program  id          program_name  program_level
----------  ----------  ----------  ----------  ------------  -------------
1           Josefina    3           3           Linguistics   Ph.D.
2           Cecilia     2           2           Biology       Master's
3           Nico        2           2           Biology       Master's
4           Sarah       1           1           Anthropology  Master's
```

This query demonstrates the power of relational databases by using the foreign key in the "students" table to coordinate data with the "programs" table.

### Challenge One

Write a query that returns only the name of each student and their respective program level. You can find a solution [here](solution5.sql).

### Challenge Two

Write a query that returns the name of each student, their program name, and their GPA with results sorted by GPA from low to high. You will likely need to do some Googling, or you can find a solution [here](solution6.sql).

*How can you make sure that the data from 'gpas', 'students', and 'programs' is aligning correctly?*

# Import data into table

Now we are moving back into the world of Python! Let's create a new file in the `database-practice` directory.

Our goal is to create a database table from an [existing csv file](https://github.com/GCDigitalFellows/nypl_data/blob/master/nypl_items.csv). Click the link, then right-click the `Download` button on that page, and click `save link as` to get the csv we want. Move the csv file to your `database-practice` directory.

We're going to use the Pandas library to import our data.

```python
import pandas
import sqlite3

# make the connection
conn = sqlite3.connect('nypldb.db')

# have Pandas read our CSV
df = pandas.read_csv('nypl_items.csv', low_memory=False)

# convert our data into a SQlite table called 'nypl_items'
df.to_sql('nypl_items', conn)

# print a list of every record in  the table
print (pandas.read_sql_query("SELECT * FROM nypl_items", conn))
```

# What makes a good dataset / cleaning data

Given a dataset that we want to use, how do we know that we can trust it to tell us what we want to learn? Of course, we have to ask the right questions&mdash;those that are possible for the dataset to answer. But we also have to test the integrity and consistency of the dataset. We have to make sure that the dataset is not incomplete, or messy, or liable to mislead us when we attempt to analyze it. Indeed, data scientists and engineers often spend the lion’s share of their time on various forms of “data cleaning” and much less on the “fun” part of the job.

There are many different levels to this kind of work and ways to prepare data for analysis, from checking for outliers in your data, to parsing dates, to determining the correct values in missing data.

# Cleansing data

At one level, cleaning data means ensuring accuracy and consistency of individual entries&mdash;each cell in a row of a table.

This can take the form of:
- ensuring consistent nomenclature
- identifying and addressing missing values
- removing duplicate entries
- correcting typos and removing additional spaces
- for text-based data, ensuring capitalization is consistent
- parsing dates and numbers
- correcting character encodings (for international data)

It is possible to use Python (and Pandas) to do these things, but programs like Excel will also have tools for each of these (Find & Replace, Remove Duplicates, etc.).

Performing basic descriptive statistics on a dataset can also help to flag extreme outliers that may be incorrect (negative values in a set of ages, for example). In Excel specifically, it helps to get comfortable using formulas like COUNT, MIN, MAX, and AVERAGE for numerical data, COUNTIF for text entries, or COUNTBLANK for empty cells.


It is essential to create unique numerical identifiers for a dataset (eg, using column A of a spreadsheet for consecutive ID numbers), so that, after sorting and re-sorting, the dataset can be restored to its original order with ease and certainty.

# Tidying data

In addition to “cleansing” a dataset of typos, inconsistencies, etc., a process called “tidying” is employed to correct its formatting. This is more involved and complicated than it might at first appear.

A “tidy” data structure has been defined by [Hadley Wickham](http://vita.had.co.nz/papers/tidy-data.html) as having the following qualities:
- Variables are arranged in columns: each variable forms a column and contains values
- Observations are arranged in rows: each observation forms a row
- Each type of observational unit forms a table

Thus, messy datasets may have issues such as:
- Columns without headers (or headers that are values, rather than the names of variables)
- More than one variable within a single column
- Variables stored in both rows and columns
- Several types of observational units in a single table
- A single observational unit stored across multiple tables

Most of these “messiness” issues can be solved with the following tools:
- Melting
- String splitting
- Casting

# Querying to summarize your data

Using the SQLITE REPL, let's see what the nypl_items table looks like. To load the database, first confirm that you are in the `database-practice` directory. Start the Sqlite program:

`sqlite3`

Then, type this text to open our database:

`.open file:nypldb.db`

1. How many records are in the table?

```sql
SELECT COUNT(*) FROM nypl_items;
```

2. What do the records look like?

```sql
SELECT * FROM nypl_items LIMIT 3;
```

3. How many different languages do you have items in?

```sql
SELECT DISTINCT language FROM nypl_items;
```

# Challenge: Find out more about what's in nypl_items

## Take 5 minutes to explore the nypl_items&mdash;what can you tell us about it?  

- How many publishers are there? How many unique publishers?
- What is the oldest item?
- How many items are in the genre "Engravings"?
- Where are the greatest number of items from?

# Pause and reflect

### Why learn relational databases when I know Excel?

One key reason to favor databases over Excel is data integrity: databases guarantee a certain level of tidiness by ensuring that every value in a particular field will be of a single type. Volume is another consideration: Excel quickly gets bogged down or even broken by larger data sets, and databases can make performing analysis, calculations, etc. much faster. Finally, databases allow for more robust connections, enabling us to work with multiple related data sets (comparing, contrasting, joining, etc.) and to plug into other applications.

# SQL + Python = Awesome!

Let's write a short program that asks a user for a place name and returns all records from the database that contain the value in the "place" field.

0\. Using a text editor, create a file called "nypl_search.py" in the directory where the database containing the "nypl_items" table is located.

1\. Write some pseudocode in "nypl_search.py" that describes what our code will do once it's finished:

```Python
# import sqlite3 library

# connect to the database

# say hello to the user

# ask the user for a place

# search the place field in the "nypl_items" table for the place name

# return a list of records from the database that are from the place name
```

2\. Import the sqlite3 library, connect to the database, and create a cursor object.

```Python
# import sqlite3 library
import sqlite3

# connect to the database
conn = sqlite3.connect('nypldb.db')

# create a cursor object
cur = conn.cursor()
```

3\. Replace pseudocode with working Python and SQL

```Python
# say hello to the user
print("Hello! I will search your database for items from any place you tell me! ")

# ask the user for a place name
place_name = input("Please give me a place name: ")

# search the place field for the place name
cur.execute("SELECT * FROM nypl_items WHERE place = ?", (place_name,))

# return a list of records from the database that contain the keyword
record_list = cur.fetchall()

for i in record_list:
	print("\n\n", i)
```

4\. Run the program. First, open the command line, `cd` to the directory containing your "nypl_search.py" file and "nypldb.db" database. Then type

    python nypl_search.py

and hit Enter. You'll be prompted to enter a place name, which is case-sensitive. Try "Paris" or some other location, and you should see output from the database returned to you.

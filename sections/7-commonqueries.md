[<<< Back](6-buildtable_challenge.md) - [Next >>>](8-innerjoin.md)  

# Querying your database  

Now that we have a decent looking database, we can execute some queries to manipulate our data. For this section, we will be using the Python REPL. At the command line, navigate to the directory where you are keeping the "firstdb.db" database, and type:

```
python
```

When you are in the REPL, type:

```python
import createdb
```

This imports the file we wrote at the beginning of this workshop, which connects us to our database. So, now we can play around with querying our data.

Each query is made up of the same basic set of clauses:  
- The `SELECT` clause indicates the fields that you want to return.  
- The `FROM` clause indicates the table that the fields belong to.  
- The `WHERE` clause filters the results of the query.  

Together, these clauses create a new temporary table based on the criteria specified in each one.  

Practice executing these queries and see what they return.  

1. This query returns all of the records (i.e., rows) in the "students" table:  
```python
# query all fields for each record in the table "students"
cursor.execute("SELECT * FROM students")
# print results
print(cursor.fetchall())
```  

2. This query returns only the values in the "student" field for all records in the "students" table:  
```python
cursor.execute("SELECT student FROM students")
# print results
print(cursor.fetchall())
```  

3. This query returns two fields from the "students" table:  
```python
cursor.execute("""SELECT student, id
FROM students""")
# print results
print(cursor.fetchall())
```  

### Challenge

Write a query that returns "program_name" and "program_level" for each record in the "programs" table.


### Solution

```python
cursor.execute("""SELECT program_name, program_level
FROM programs""")
# print results
print(cursor.fetchall())
```

4. In this query, `WHERE` filters the records by their value in the "id" field:  

```python
"""show all fields for each record in the table 'students' where the value of the
'id' field is equal to '3'"""
cursor.execute("""SELECT *
FROM students
WHERE id = '3'""")
# print results
print(cursor.fetchall())
```

### Challenge

Write a query that returns entire records for _**only**_ Ph.D programs in the 'programs' table.


### Solution

```sql
SELECT *
FROM programs
WHERE program_level = "Ph.D.";
```
	
[<<< Back](6-buildtable_challenge.md) - [Next >>>](8-innerjoin.md)  

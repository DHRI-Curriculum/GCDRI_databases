[<<< Back](6-buildtable_challenge.md) - [Next >>>](8-innerjoin.md)

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
SELECT * FROM students WHERE id = '3';
```
```
id          student     id_program
----------  ----------  ----------
3           Nico        2
```
### Challenge

Write a query that returns entire records for _**only**_ Ph.D programs in the 'programs' table. You can find a solution [here](solution4.sql).

[<<< Back](6-buildtable_challenge.md) - [Next >>>](8-innerjoin.md)

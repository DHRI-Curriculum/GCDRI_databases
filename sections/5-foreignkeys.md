[<<< Back](4-updatefield.md) - [Next >>>](6-buildtable_challenge.md)

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

[<<< Back](4-updatefield.md) - [Next >>>](6-buildtable_challenge.md)

[<<< Back](3b-pythonic.md) - [Next >>>](5-foreignkeys.md)

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

[<<< Back](3-insertdata.md) - [Next >>>](5-foreignkeys.md)

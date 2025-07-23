
Here’s a quick rundown of how to use the `sqlite3` command-line tool.

### 1. Open or Create a Database

To get started, you open the `sqlite3` tool by giving it the name of a database file. If the file doesn't exist, SQLite will create it for you.

```bash
# This will open the database 'test.db' in the sqlite3 shell
# If test.db doesn't exist, it will be created.
sqlite3 test.db
```

After running this, your terminal prompt will change to `sqlite>`.

### 2. Run SQL Commands

Now you can run any standard SQL command. **Just remember to end every command with a semicolon (`;`)**.

```sql
-- Create a new table called 'users'
CREATE TABLE users (
  id INTEGER PRIMARY KEY,
  name TEXT,
  email TEXT
);

-- Insert some data into the table
INSERT INTO users (name, email) VALUES ('Alice', 'alice@example.com');
INSERT INTO users (name, email) VALUES ('Bob', 'bob@example.com');

-- Select data from the table to view it
SELECT * FROM users;
```

The output for the `SELECT` statement will look something like this:

```
1|Alice|alice@example.com
2|Bob|bob@example.com
```

### 3. Use "Dot Commands" for Utility Tasks

The `sqlite3` tool has special commands that start with a period (`.`). These are not SQL commands; they are for controlling the tool itself. You **don't** need a semicolon for these.

Here are some of the most useful ones:

*   `.tables`: List all the tables in your database.
*   `.schema <table_name>`: Show the `CREATE` statement for a table, so you can see its structure.
*   `.headers on`: Turn on column headers for your query results.
*   `.mode column`: Display results in nicely formatted columns.
*   `.help`: Show a list of all available dot commands.
*   `.quit` or `.exit`: To leave the `sqlite3` shell and return to your regular terminal prompt.

**Example of improving the output:**

```sqlite
sqlite> .headers on
sqlite> .mode column
sqlite> SELECT * FROM users;
id  name   email
--  -----  -------------------
1   Alice  alice@example.com
2   Bob    bob@example.com
```

### 4. Exit the Tool

When you're finished, just type:

```sqlite
.quit
```

That's the basic workflow! This should be enough to get you started with creating databases, adding data, and querying it from your terminal.
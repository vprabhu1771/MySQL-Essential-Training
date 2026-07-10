To store a JSON array in MySQL, use the native JSON data type introduced in MySQL 5.7.8. This data type provides automatic validation and optimized binary storage for quick access.

# 1. Create a Table with a JSON Column

Define the column using the JSON data type:

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    tags JSON
);
```

# 2. Insert a JSON Array

You can insert an array directly as a standard JSON-formatted string or construct it programmatically:

## Method A: Insert as a raw string literal

```sql
INSERT INTO users (name, tags) 
VALUES ('Alice', '["admin", "developer", "remote"]');
```

## Method B: Use the JSON_ARRAY() function

```sql
INSERT INTO users (name, tags) 
VALUES ('Bob', JSON_ARRAY('manager', 'support'));
```

# 3. Common Operations on JSON Arrays

Read the whole array:

```sql
SELECT tags FROM users WHERE id = 1;
```

Extract a specific element by index: Use the `->` operator with path notation (`$[index]`).

```sql
-- Gets the first item ("admin")
SELECT tags->'$[0]' FROM users WHERE id = 1; 
```

Append a new value to the array: Use JSON_ARRAY_APPEND().

```
UPDATE users 
SET tags = JSON_ARRAY_APPEND(tags, '$', 'billing') 
WHERE id = 1;
```

Search for rows containing a specific value: Use JSON_CONTAINS().

```sql
-- Finds users who have the "admin" tag
SELECT * FROM users 
WHERE JSON_CONTAINS(tags, '"admin"');
```

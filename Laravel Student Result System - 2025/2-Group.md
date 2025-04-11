Here is the **SQL equivalent** of the Laravel migration you provided for the `groups` table:

```sql
CREATE TABLE groups (
    id BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    status VARCHAR(255) DEFAULT '0',
    created_at TIMESTAMP NULL,
    updated_at TIMESTAMP NULL
);
```

### Explanation:
- `id()` → `BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY`
- `string('name')` → `VARCHAR(255) NOT NULL`
- `string('status')->default(false)` → Since it's a string, `false` becomes `'0'`
- `timestamps()` → Adds `created_at` and `updated_at` as `TIMESTAMP NULL`

Let me know if you want `status` as a `BOOLEAN` or `ENUM` instead of a `VARCHAR`.



Here is the **SQL equivalent** for inserting the `$groups` array into the `groups` table:

```sql
INSERT INTO groups (name, status, created_at, updated_at) VALUES
('CS', '1', NOW(), NOW()),
('BIO', '1', NOW(), NOW()),
('COM CS', '1', NOW(), NOW()),
('COM MAT', '1', NOW(), NOW()),
('PS', '1', NOW(), NOW()),
('FS', '1', NOW(), NOW());
```

### Notes:
- `'status' => true` becomes `'1'` in SQL since `status` is defined as a string (`VARCHAR`).
- `NOW()` is used to populate `created_at` and `updated_at`.

Let me know if you're using a different data type for `status`, like `BOOLEAN` or `ENUM`, and I can adjust the SQL accordingly.


---

Here are some commonly used **`SELECT` queries** for the `groups` table based on your structure:

---

### 🔹 1. **Select all groups**
```sql
SELECT * FROM groups;
```

---

### 🔹 2. **Select only active groups (`status = '1'`)**
```sql
SELECT * FROM groups
WHERE status = '1';
```

---

### 🔹 3. **Select only inactive groups (`status = '0'`)**
```sql
SELECT * FROM groups
WHERE status = '0';
```

---

### 🔹 4. **Find a group by name (e.g., 'CS')**
```sql
SELECT * FROM groups
WHERE name = 'CS';
```

---

### 🔹 5. **List groups ordered by most recent**
```sql
SELECT * FROM groups
ORDER BY created_at DESC;
```

---

Let me know if you want joins with `grades`, `students`, or `exams` using the `group_id` field!
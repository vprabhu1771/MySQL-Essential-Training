Here's the **SQL equivalent** for your `grades` table migration:

```sql
CREATE TABLE grades (
    id BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    status VARCHAR(255) DEFAULT '0',
    created_at TIMESTAMP NULL,
    updated_at TIMESTAMP NULL
);
```

### Notes:
- `status` is defined as a string in your migration, so `'false'` becomes `'0'` or any string (e.g., `'active'`, `'inactive'`, etc.).
- If you prefer using `BOOLEAN` for `status`, you can modify your migration to use `$table->boolean('status')->default(false);`.

Let me know if you need the seed SQL for this or want to switch `status` to an `enum` or `boolean`!



----

Here’s the **SQL equivalent** of your `GradeSeeder` data insertion:

```sql
INSERT INTO grades (name, status, created_at, updated_at) VALUES
('1', '1', NOW(), NOW()),
('2', '1', NOW(), NOW()),
('3', '1', NOW(), NOW()),
('4', '1', NOW(), NOW()),
('5', '1', NOW(), NOW()),
('6', '1', NOW(), NOW()),
('7', '1', NOW(), NOW()),
('8', '1', NOW(), NOW()),
('9', '1', NOW(), NOW()),
('10', '1', NOW(), NOW()),
('11', '1', NOW(), NOW()),
('12', '1', NOW(), NOW());
```

### Notes:
- Since your `status` field is defined as `string`, the `true` values are represented as `'1'`.
- `NOW()` sets the current timestamp for `created_at` and `updated_at`.

Let me know if you'd like the SQL to select, update, or delete these rows too!


---

Sure! Here are several **`SELECT` queries** for your `grades` table to help you retrieve data in different ways:

---

### ✅ 1. **Get all grades:**
```sql
SELECT * FROM grades;
```

---

### ✅ 2. **Get all active grades (`status = '1'`):**
```sql
SELECT * FROM grades
WHERE status = '1';
```

---

### ✅ 3. **Get all inactive grades (`status = '0'`):**
```sql
SELECT * FROM grades
WHERE status = '0';
```

---

### ✅ 4. **Get grade by name (e.g., grade "10"):**
```sql
SELECT * FROM grades
WHERE name = '10';
```

---

### ✅ 5. **List grades ordered by name:**
```sql
SELECT * FROM grades
ORDER BY name ASC;
```

---

### ✅ 6. **Show recently created grades (newest first):**
```sql
SELECT * FROM grades
ORDER BY created_at DESC;
```

---

### ✅ 7. **Count how many grades are active:**
```sql
SELECT COUNT(*) AS active_count
FROM grades
WHERE status = '1';
```

---

Would you like the Laravel **Eloquent version** of these queries too?
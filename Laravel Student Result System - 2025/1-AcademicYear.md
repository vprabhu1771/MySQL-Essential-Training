Here is the equivalent **SQL statement** that represents the Laravel migration you provided for creating the `academic_years` table:

```sql
CREATE TABLE academic_years (
    id BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    status VARCHAR(255) DEFAULT '0',
    created_at TIMESTAMP NULL,
    updated_at TIMESTAMP NULL
);
```

### Notes:
- `id()` in Laravel maps to `BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY`.
- `string('name')` maps to `VARCHAR(255) NOT NULL`.
- `string('status')->default(false)` is a bit ambiguous, because `false` is not typically a string. Laravel will cast `false` to `'0'` as a string. If you're planning to use an enum like `GeneralStatus`, consider using `ENUM` or a proper boolean type instead.
- `timestamps()` adds `created_at` and `updated_at` columns, both `TIMESTAMP NULL`.

Let me know if you’d like the `status` field to be an enum based on your `App\Enums\GeneralStatus`, and I can update the SQL accordingly.


Here’s the **SQL equivalent** of your `AcademicYearSeeder` for inserting the academic years into the `academic_years` table:

```sql
INSERT INTO academic_years (name, status, created_at, updated_at) VALUES
('2020-2021', '0', NOW(), NOW()),
('2021-2022', '0', NOW(), NOW()),
('2022-2023', '0', NOW(), NOW()),
('2023-2024', '0', NOW(), NOW()),
('2024-2025', '0', NOW(), NOW()),
('2025-2026', '1', NOW(), NOW());
```

### Notes:
- Since `status` is stored as a `string` in the migration, `false` becomes `'0'` and `true` becomes `'1'` when cast to strings.
- `NOW()` is used to fill `created_at` and `updated_at` timestamps automatically.

Let me know if you're using a specific enum or want to convert this field to a boolean or ENUM type in SQL.


---

Here are useful **`SELECT` queries** for the `academic_years` table based on your schema and seed data:

---

### 🔹 1. **Select all academic years**
```sql
SELECT * FROM academic_years;
```

---

### 🔹 2. **Get only the active academic year**
```sql
SELECT * FROM academic_years
WHERE status = '1';
```

---

### 🔹 3. **Get inactive academic years**
```sql
SELECT * FROM academic_years
WHERE status = '0';
```

---

### 🔹 4. **Get academic years ordered by latest created**
```sql
SELECT * FROM academic_years
ORDER BY created_at DESC;
```

---

### 🔹 5. **Find a specific academic year**
```sql
SELECT * FROM academic_years
WHERE name = '2024-2025';
```

---

Let me know if you'd like to join this with related tables like `students`, `grades`, or `exams`!
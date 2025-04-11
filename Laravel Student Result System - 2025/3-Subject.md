Here is the **SQL equivalent** of your Laravel migration for creating the `subjects` table:

```sql
CREATE TABLE subjects (
    id BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    status VARCHAR(255) DEFAULT '0',
    created_at TIMESTAMP NULL,
    updated_at TIMESTAMP NULL
);
```

### Notes:
- The `group_id` line is commented out, so it’s excluded from the SQL.
- The `status` column is a `VARCHAR` with default `'0'` (because `false` is cast to string `'0'`).
- `timestamps()` adds `created_at` and `updated_at` as nullable timestamps.

Let me know if you plan to re-enable the `group_id` foreign key—I'll help with the SQL for that too.



Here's the **SQL equivalent** to insert your `$subjects` data into the `subjects` table:

```sql
INSERT INTO subjects (name, status, created_at, updated_at) VALUES
('ENGLISH', '1', NOW(), NOW()),
('MATHS', '1', NOW(), NOW()),
('PHYS TOT', '1', NOW(), NOW()),
('CHE TOT', '1', NOW(), NOW()),
('BIO', '1', NOW(), NOW()),
('CS', '1', NOW(), NOW()),
('ACCOUNTANCY', '1', NOW(), NOW()),
('ECONOMICS', '1', NOW(), NOW()),
('BS', '1', NOW(), NOW()),
('ENT', '1', NOW(), NOW()),
('PAINT', '1', NOW(), NOW()),
('FASHION STUDIES', '1', NOW(), NOW());
```

### Notes:
- Since your `status` field is defined as `string`, `true` becomes `'1'`.
- `NOW()` sets current timestamps for both `created_at` and `updated_at`.

Let me know if you’d like the `status` column changed to a boolean or enum in the migration for cleaner logic.
Here is the **SQL equivalent** of your Laravel migration for creating the `group_subject` pivot table with foreign key constraints:

```sql
CREATE TABLE group_subject (
    id BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    group_id BIGINT UNSIGNED NOT NULL,
    subject_id BIGINT UNSIGNED NOT NULL,
    created_at TIMESTAMP NULL,
    updated_at TIMESTAMP NULL,
    CONSTRAINT group_subject_group_id_foreign FOREIGN KEY (group_id) REFERENCES groups(id) ON DELETE CASCADE,
    CONSTRAINT group_subject_subject_id_foreign FOREIGN KEY (subject_id) REFERENCES subjects(id) ON DELETE CASCADE
);
```

### Notes:
- `foreignId('...')->constrained()` defaults to referencing the `id` column on a table matching the field name (`group_id` → `groups`, `subject_id` → `subjects`).
- `onDelete('cascade')` means that if a `group` or `subject` is deleted, the corresponding entries in this pivot table will also be deleted.
- `timestamps()` adds `created_at` and `updated_at` columns.




Thanks for the data! Now, based on your `$groupData` structure and assuming that:

- `groups` and `subjects` tables already have data.
- You're inserting into the pivot table: `group_subject(group_id, subject_id)`.

Here’s the **SQL equivalent** to insert the group–subject relationships into the `group_subject` table:

```sql
-- CS Group
INSERT INTO group_subject (group_id, subject_id, created_at, updated_at) VALUES
((SELECT id FROM groups WHERE name = 'CS'), (SELECT id FROM subjects WHERE name = 'ENGLISH'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'CS'), (SELECT id FROM subjects WHERE name = 'MATHS'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'CS'), (SELECT id FROM subjects WHERE name = 'PHYS TOT'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'CS'), (SELECT id FROM subjects WHERE name = 'CHE TOT'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'CS'), (SELECT id FROM subjects WHERE name = 'CS'), NOW(), NOW());

-- BIO Group
INSERT INTO group_subject (group_id, subject_id, created_at, updated_at) VALUES
((SELECT id FROM groups WHERE name = 'BIO'), (SELECT id FROM subjects WHERE name = 'ENGLISH'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'BIO'), (SELECT id FROM subjects WHERE name = 'MATHS'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'BIO'), (SELECT id FROM subjects WHERE name = 'PHYS TOT'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'BIO'), (SELECT id FROM subjects WHERE name = 'CHE TOT'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'BIO'), (SELECT id FROM subjects WHERE name = 'BIO'), NOW(), NOW());

-- COM CS Group
INSERT INTO group_subject (group_id, subject_id, created_at, updated_at) VALUES
((SELECT id FROM groups WHERE name = 'COM CS'), (SELECT id FROM subjects WHERE name = 'ENGLISH'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'COM CS'), (SELECT id FROM subjects WHERE name = 'MATHS'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'COM CS'), (SELECT id FROM subjects WHERE name = 'ACCOUNTANCY'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'COM CS'), (SELECT id FROM subjects WHERE name = 'ECONOMICS'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'COM CS'), (SELECT id FROM subjects WHERE name = 'CS'), NOW(), NOW());

-- COM MAT Group
INSERT INTO group_subject (group_id, subject_id, created_at, updated_at) VALUES
((SELECT id FROM groups WHERE name = 'COM MAT'), (SELECT id FROM subjects WHERE name = 'ENGLISH'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'COM MAT'), (SELECT id FROM subjects WHERE name = 'MATHS'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'COM MAT'), (SELECT id FROM subjects WHERE name = 'ACCOUNTANCY'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'COM MAT'), (SELECT id FROM subjects WHERE name = 'ECONOMICS'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'COM MAT'), (SELECT id FROM subjects WHERE name = 'BS'), NOW(), NOW());

-- PS Group
INSERT INTO group_subject (group_id, subject_id, created_at, updated_at) VALUES
((SELECT id FROM groups WHERE name = 'PS'), (SELECT id FROM subjects WHERE name = 'ENGLISH'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'PS'), (SELECT id FROM subjects WHERE name = 'PHYS TOT'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'PS'), (SELECT id FROM subjects WHERE name = 'CHE TOT'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'PS'), (SELECT id FROM subjects WHERE name = 'BIO'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'PS'), (SELECT id FROM subjects WHERE name = 'CS'), NOW(), NOW());

-- FS Group
INSERT INTO group_subject (group_id, subject_id, created_at, updated_at) VALUES
((SELECT id FROM groups WHERE name = 'FS'), (SELECT id FROM subjects WHERE name = 'ENGLISH'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'FS'), (SELECT id FROM subjects WHERE name = 'BS'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'FS'), (SELECT id FROM subjects WHERE name = 'ENT'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'FS'), (SELECT id FROM subjects WHERE name = 'PAINT'), NOW(), NOW()),
((SELECT id FROM groups WHERE name = 'FS'), (SELECT id FROM subjects WHERE name = 'FASHION STUDIES'), NOW(), NOW());
```

Let me know if you want this logic in a Laravel Seeder too!
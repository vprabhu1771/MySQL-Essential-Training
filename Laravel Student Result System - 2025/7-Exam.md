Here is the **SQL equivalent** of your Laravel `exams` table migration:

```sql
CREATE TABLE exams (
    id BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    grade_id BIGINT UNSIGNED NOT NULL,
    group_id BIGINT UNSIGNED NULL,
    exam_date DATE NOT NULL,
    created_at TIMESTAMP NULL,
    updated_at TIMESTAMP NULL
);
```

### Optional: Add foreign key constraints (recommended for data integrity):

```sql
ALTER TABLE exams
    ADD CONSTRAINT fk_exams_grade FOREIGN KEY (grade_id) REFERENCES grades(id) ON DELETE CASCADE,
    ADD CONSTRAINT fk_exams_group FOREIGN KEY (group_id) REFERENCES groups(id) ON DELETE CASCADE;
```

Let me know if you'd like SQL to **insert sample data** or perform **select queries** as well.


----

Here's the **SQL insert statement** to populate your `exams` table with sample data for various exam types:

```sql
INSERT INTO exams (name, grade_id, group_id, exam_date, created_at, updated_at) VALUES
('Weekly Test', 10, 1, '2025-06-10', NOW(), NOW()),
('Monthly Test', 10, 1, '2025-06-20', NOW(), NOW()),
('Mid Term 1', 10, 1, '2025-07-10', NOW(), NOW()),
('Mid Term 2', 10, 1, '2025-08-10', NOW(), NOW()),
('Mid Term 3', 10, 1, '2025-09-10', NOW(), NOW()),
('Quarterly Exam', 10, 1, '2025-10-01', NOW(), NOW()),
('Halfly Exam', 10, 1, '2025-11-15', NOW(), NOW()),
('Annual Exam', 10, 1, '2026-03-20', NOW(), NOW());
```

### Notes:
- You can change `grade_id` and `group_id` as needed (currently using `10` and `1`).
- Dates are for demo; feel free to modify according to your academic calendar.

Want inserts for other grades or groups too?


---

Here are some useful `SELECT` queries for your `exams` table and related data:

---

### 🔹 **1. Get All Exams**
```sql
SELECT * FROM exams;
```

---

### 🔹 **2. Get All Exams for a Specific Grade (e.g., Grade 10)**
```sql
SELECT * FROM exams
WHERE grade_id = 10;
```

---

### 🔹 **3. Get All Exams for a Specific Group (e.g., Group ID 1)**
```sql
SELECT * FROM exams
WHERE group_id = 1;
```

---

### 🔹 **4. Get Exams Sorted by Exam Date**
```sql
SELECT * FROM exams
ORDER BY exam_date ASC;
```

---

### 🔹 **5. Get Exams with Grade and Group Names (Assuming Relationships Exist)**
```sql
SELECT exams.name AS exam_name,
       grades.name AS grade_name,
       groups.name AS group_name,
       exams.exam_date
FROM exams
JOIN grades ON exams.grade_id = grades.id
LEFT JOIN groups ON exams.group_id = groups.id
ORDER BY exams.exam_date;
```

---

### 🔹 **6. Get Upcoming Exams (Exam date in future)**
```sql
SELECT * FROM exams
WHERE exam_date > CURDATE()
ORDER BY exam_date;
```

Let me know if you'd like queries for a specific UI, like showing "Upcoming Exams this Week" or "Exams per Group and Grade".
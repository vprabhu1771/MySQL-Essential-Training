Here is the **SQL equivalent** of your Laravel migration for the `marks` table:

---

### 🔹 **Create Table SQL**
```sql
CREATE TABLE marks (
    id BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    student_id BIGINT UNSIGNED NOT NULL,
    exam_id BIGINT UNSIGNED NOT NULL,
    subject_id BIGINT UNSIGNED NOT NULL,
    score VARCHAR(255) NOT NULL,
    created_at TIMESTAMP NULL,
    updated_at TIMESTAMP NULL
);
```

---

### 🔹 **Add Foreign Key Constraints** (optional but recommended)
If you want to match the Laravel-style foreign key references (currently commented in your migration), you can run this after creating the table:

```sql
ALTER TABLE marks
    ADD CONSTRAINT fk_marks_student
        FOREIGN KEY (student_id) REFERENCES students(id) ON DELETE CASCADE,
    ADD CONSTRAINT fk_marks_exam
        FOREIGN KEY (exam_id) REFERENCES exams(id) ON DELETE CASCADE,
    ADD CONSTRAINT fk_marks_subject
        FOREIGN KEY (subject_id) REFERENCES subjects(id) ON DELETE CASCADE;
```

---

Let me know if you want:
- A sample `INSERT` query for this table.
- A `SELECT` query that joins marks with students, subjects, and exams.


----

Here's a sample **SQL `INSERT` query** for the `marks` table. This assumes you already have data in the `students`, `exams`, and `subjects` tables.

---

### 🔹 Example: Insert sample marks for a student

```sql
INSERT INTO marks (student_id, exam_id, subject_id, score, created_at, updated_at) VALUES
(1, 1, 1, '85', NOW(), NOW()),   -- Student 1, Exam 1, Subject 1
(1, 1, 2, '90', NOW(), NOW()),   -- Student 1, Exam 1, Subject 2
(1, 1, 3, '78', NOW(), NOW()),   -- Student 1, Exam 1, Subject 3
(2, 1, 1, '88', NOW(), NOW()),   -- Student 2, Exam 1, Subject 1
(2, 1, 2, '92', NOW(), NOW());   -- Student 2, Exam 1, Subject 2
```

---

### ⚠️ Notes:
- Replace `student_id`, `exam_id`, and `subject_id` with actual IDs from your tables.
- `score` is stored as a string (`VARCHAR`) — you can change it to `INTEGER` in the migration if you want numerical sorting/filtering.

Let me know if you want dynamic data or joins to fetch and verify marks across students, exams, and subjects!


---

Here are some useful `SELECT` queries for the `marks` table and related tables (`students`, `exams`, `subjects`):

---

### 🔹 1. **Basic: Get all marks**
```sql
SELECT * FROM marks;
```

---

### 🔹 2. **Get student marks with subject and exam names**
```sql
SELECT 
    students.name AS student_name,
    exams.name AS exam_name,
    subjects.name AS subject_name,
    marks.score
FROM marks
JOIN students ON marks.student_id = students.id
JOIN exams ON marks.exam_id = exams.id
JOIN subjects ON marks.subject_id = subjects.id;
```

---

### 🔹 3. **Marks of a particular student**
```sql
SELECT 
    exams.name AS exam_name,
    subjects.name AS subject_name,
    marks.score
FROM marks
JOIN students ON marks.student_id = students.id
JOIN exams ON marks.exam_id = exams.id
JOIN subjects ON marks.subject_id = subjects.id
WHERE students.name = 'John Doe';  -- Replace with actual student name
```

---

### 🔹 4. **Average score per subject**
```sql
SELECT 
    subjects.name AS subject_name,
    AVG(CAST(marks.score AS UNSIGNED)) AS average_score
FROM marks
JOIN subjects ON marks.subject_id = subjects.id
GROUP BY subjects.name;
```

---

### 🔹 5. **Total marks per student in a specific exam**
```sql
SELECT 
    students.name AS student_name,
    exams.name AS exam_name,
    SUM(CAST(marks.score AS UNSIGNED)) AS total_score
FROM marks
JOIN students ON marks.student_id = students.id
JOIN exams ON marks.exam_id = exams.id
WHERE exams.name = 'Mid Term 1'
GROUP BY students.name, exams.name;
```

---

Let me know if you want queries filtered by grade, group, or academic year!
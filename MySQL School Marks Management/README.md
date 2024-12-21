Here's an example of how you can create a MySQL database with stored procedures and triggers to manage a school marksheet system. 

### Database Structure
1. **`students` Table**: Stores student details.
2. **`marksheet` Table**: Stores marksheet details (subject name, subject mark, and status).

---

### SQL Script

```sql
-- Create students table
CREATE TABLE students (
    id INT AUTO_INCREMENT PRIMARY KEY,
    student_name VARCHAR(100) NOT NULL
);

-- Create marksheet table
CREATE TABLE marksheet (
    id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT,
    subject_name VARCHAR(50),
    subject_mark INT,
    status VARCHAR(10),
    FOREIGN KEY (student_id) REFERENCES students(id)
);

-- Insert example students
INSERT INTO students (student_name) VALUES ('John Doe'), ('Jane Smith');

-- Stored Procedure to Add Marks
DELIMITER $$

CREATE PROCEDURE AddMark(
    IN p_student_id INT,
    IN p_subject_name VARCHAR(50),
    IN p_subject_mark INT
)
BEGIN
    DECLARE p_status VARCHAR(10);

    -- Determine pass or fail status
    IF p_subject_mark >= 35 THEN
        SET p_status = 'Pass';
    ELSE
        SET p_status = 'Fail';
    END IF;

    -- Insert data into the marksheet table
    INSERT INTO marksheet (student_id, subject_name, subject_mark, status)
    VALUES (p_student_id, p_subject_name, p_subject_mark, p_status);
END $$

DELIMITER ;

-- Trigger to Automatically Update Status on Marks Update
DELIMITER $$

CREATE TRIGGER UpdateStatusAfterMarkUpdate
AFTER UPDATE ON marksheet
FOR EACH ROW
BEGIN
    DECLARE new_status VARCHAR(10);

    -- Determine pass or fail status
    IF NEW.subject_mark >= 35 THEN
        SET new_status = 'Pass';
    ELSE
        SET new_status = 'Fail';
    END IF;

    -- Update the status column
    UPDATE marksheet
    SET status = new_status
    WHERE marksheet_id = NEW.marksheet_id;
END $$

DELIMITER ;

-- Example Usage
-- Add marks for students using the stored procedure
CALL AddMark(1, 'Math', 40);
CALL AddMark(1, 'Science', 30);
CALL AddMark(2, 'Math', 50);
CALL AddMark(2, 'Science', 20);

-- Update a mark to see the trigger in action
UPDATE marksheet SET subject_mark = 45 WHERE id = 2;

-- View Results
SELECT * FROM students;
SELECT * FROM marksheet;
```

---

### Explanation

1. **Stored Procedure `AddMark`**:
   - Adds a record to the `marksheet` table.
   - Calculates the `status` (Pass/Fail) based on the mark.

2. **Trigger `UpdateStatusAfterMarkUpdate`**:
   - Automatically updates the `status` field if the `subject_mark` is updated.

3. **Example Usage**:
   - Add marks using the `AddMark` procedure.
   - Update a mark to see the `status` column updated automatically by the trigger.

### Output Example
Running the queries will populate the `students` and `marksheet` tables. The `status` column in `marksheet` will reflect the pass/fail result based on the marks.

| marksheet_id | student_id | subject_name | subject_mark | status |
|--------------|------------|--------------|--------------|--------|
| 1            | 1          | Math         | 40           | Pass   |
| 2            | 1          | Science      | 45           | Pass   |
| 3            | 2          | Math         | 50           | Pass   |
| 4            | 2          | Science      | 20           | Fail   |
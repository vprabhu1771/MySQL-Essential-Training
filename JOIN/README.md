```sql
CREATE TABLE `department` (
    `id` INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    `name` VARCHAR(255)
);
```

```sql
INSERT INTO `department` (`name`) VALUES
("Mathematics"),
("History"),
("Physics"),
("Computer Science");
```

```sql
CREATE TABLE `student` (
    `id` INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    `name` VARCHAR(255),
    `department_id` INT
);
```

```sql
INSERT INTO `student` (`name`) VALUES
("A", 1),
("B", 2),
("C", 3),
("D", 4);
```

```sql
SELECT 
    student.id,
    student.name AS student_name,
    department.name AS department_name
FROM student
INNER JOIN department 
ON student.department_id = department.id;
```

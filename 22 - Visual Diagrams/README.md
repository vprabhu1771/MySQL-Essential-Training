Here are **visual diagrams** for the four types of database relationships:

---

### 📘 1. **One-to-One (1:1)**

```
+---------+        +------------+
|  Users  |        |  Profiles  |
+---------+        +------------+
| id      |◄──────►| user_id    |
| name    |        | bio        |
+---------+        +------------+
```

- Each user has **exactly one** profile.
- The `Profiles` table holds a foreign key `user_id` referencing `Users.id`.

---

### 📗 2. **One-to-Many (1:N)**

```
+-------------+        +------------+
| Categories  |        |  Products  |
+-------------+        +------------+
| id          |◄───────┤ category_id|
| name        |        | name       |
+-------------+        +------------+
```

- One category can have **many products**.
- Each product belongs to **only one** category.

---

### 📙 3. **Many-to-One (N:1)** *(inverse of 1:N)*

```
+------------+         +-------------+
|  Orders    |         | Customers   |
+------------+         +-------------+
| id         | ───────►| id          |
| customer_id|         | name        |
+------------+         +-------------+
```

- Many orders belong to a **single customer**.

---

### 📕 4. **Many-to-Many (M:N)**

```
+-----------+     +-------------------+     +----------+
| Students  |     | student_course    |     | Courses  |
+-----------+     +-------------------+     +----------+
| id        |◄───►| student_id        |     | id       |
| name      |     | course_id         |◄───►| name     |
+-----------+     +-------------------+     +----------+
```

- The **join table** `student_course` connects `Students` and `Courses` using foreign keys.
- Each student can enroll in many courses, and each course can have many students.

---

Would you like these diagrams in **image format** or as part of a presentation/document?
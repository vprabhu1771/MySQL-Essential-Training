Sure! Here are **SQL code examples** for each of the four database relationship types:

---

### 📘 1. **One-to-One (1:1)**  
**Example**: `users` and `profiles`

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE profiles (
    id INT PRIMARY KEY,
    user_id INT UNIQUE,  -- ensures 1:1 relation
    bio TEXT,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

---

### 📗 2. **One-to-Many (1:N)**  
**Example**: `categories` and `products`

```sql
CREATE TABLE categories (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE products (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    category_id INT,
    FOREIGN KEY (category_id) REFERENCES categories(id)
);
```

---

### 📙 3. **Many-to-One (N:1)**  
**Example**: `orders` and `customers`

```sql
CREATE TABLE customers (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE orders (
    id INT PRIMARY KEY,
    customer_id INT,
    amount DECIMAL(10, 2),
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

> 💡 Many orders point to a single customer. This is the **inverse** of One-to-Many.

---

### 📕 4. **Many-to-Many (M:N)**  
**Example**: `students` and `courses` with a pivot table `student_course`

```sql
CREATE TABLE students (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE courses (
    id INT PRIMARY KEY,
    title VARCHAR(100)
);

-- Pivot (join) table for the many-to-many relationship
CREATE TABLE student_course (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students(id),
    FOREIGN KEY (course_id) REFERENCES courses(id)
);
```

---

Let me know if you want these examples adapted for **Laravel Eloquent**, **Django ORM**, or another framework!
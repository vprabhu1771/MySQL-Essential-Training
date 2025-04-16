There are **four main types of database relationships** commonly used in relational databases:

---

### 1. **One-to-One (1:1)**
- **Definition**: A record in **Table A** is related to only one record in **Table B**, and vice versa.
- **Example**:  
  - One **user** has one **profile**.  
  - Tables: `users` and `user_profiles`
- **Use case**: When splitting rarely-used fields into a separate table.

---

### 2. **One-to-Many (1:N)**
- **Definition**: A record in **Table A** can be related to **many** records in **Table B**, but a record in **Table B** belongs to only **one** record in **Table A**.
- **Example**:  
  - One **category** has many **products**.  
  - Tables: `categories` and `products`
- **Use case**: This is the **most common** relationship type in applications.

---

### 3. **Many-to-One (N:1)**
- **Definition**: The inverse of One-to-Many. Many records in **Table A** belong to one record in **Table B**.
- **Example**:  
  - Many **orders** belong to one **customer**.  
  - Tables: `orders` and `customers`
- **Use case**: Just the opposite view of 1:N.

---

### 4. **Many-to-Many (M:N)**
- **Definition**: Records in **Table A** can relate to **many** records in **Table B**, and vice versa.
- **Example**:  
  - Students and Courses:  
    - A student can enroll in many courses, and a course can have many students.
  - Tables: `students`, `courses`, and a **pivot/join table** like `student_course`
- **Use case**: When both sides of the relationship have multiple related entries.

---

### Summary Table:

| Relationship      | Description                             | Example                        |
|------------------|-----------------------------------------|--------------------------------|
| One-to-One       | 1 record ↔ 1 record                     | User ↔ Profile                 |
| One-to-Many      | 1 record ↔ Many records                 | Category ↔ Products            |
| Many-to-One      | Many records ↔ 1 record                 | Orders ↔ Customer              |
| Many-to-Many     | Many records ↔ Many records            | Students ↔ Courses             |

---

Let me know if you want visual diagrams or code examples in SQL, Laravel Eloquent, or Django ORM!

Here’s an **advanced MySQL trigger example** that logs changes from the `orders` table into an `order_history` table when an update occurs.

---

### **Scenario:**  
- We have an `orders` table with fields: `id`, `user_id`, `status`, `total_price`, `updated_at`.  
- We want to track status changes in an `order_history` table whenever an order is updated.

---

### **1. Create the `orders` Table**
```sql
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    status ENUM('pending', 'processing', 'shipped', 'delivered', 'cancelled') NOT NULL,
    total_price DECIMAL(10,2) NOT NULL,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

---

### **2. Create the `order_history` Table**
```sql
CREATE TABLE order_history (
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT NOT NULL,
    old_status ENUM('pending', 'processing', 'shipped', 'delivered', 'cancelled') NOT NULL,
    new_status ENUM('pending', 'processing', 'shipped', 'delivered', 'cancelled') NOT NULL,
    changed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    changed_by INT NOT NULL
);
```

---

### **3. Create an `AFTER UPDATE` Trigger**
```sql
DELIMITER //

CREATE TRIGGER after_order_update
AFTER UPDATE ON orders
FOR EACH ROW
BEGIN
    -- Only log changes if the status is updated
    IF OLD.status <> NEW.status THEN
        INSERT INTO order_history (order_id, old_status, new_status, changed_by)
        VALUES (OLD.id, OLD.status, NEW.status, NEW.user_id);
    END IF;
END;
//

DELIMITER ;
```

---

### **4. Testing the Trigger**
```sql
-- Insert a new order
INSERT INTO orders (user_id, status, total_price) 
VALUES (1, 'pending', 100.00);

-- Check the orders table
SELECT * FROM orders;

-- Update the order status
UPDATE orders SET status = 'processing' WHERE id = 1;

-- Check the order history table
SELECT * FROM order_history;
```

---

### **How This Works:**
1. When an order’s `status` changes, the trigger logs the change in the `order_history` table.
2. It captures the **old status**, **new status**, **timestamp**, and **user ID** of the person making the change.
3. The trigger only logs the change if the `status` field is actually modified.

---

🔥 **Use Case:**  
This trigger is useful in **e-commerce**, **logistics**, or any application where you need to track order status changes automatically.

Would you like another example, such as a **BEFORE DELETE trigger for soft deletes**? 🚀

Here's an example of a **MySQL trigger** that automatically updates the `updated_at` column of a `products` table whenever a row is updated.

### Example: AFTER UPDATE Trigger

```sql
DELIMITER //

CREATE TRIGGER update_product_timestamp
AFTER UPDATE ON products
FOR EACH ROW
BEGIN
    UPDATE products
    SET updated_at = NOW()
    WHERE id = OLD.id;
END;
//

DELIMITER ;
```

### Explanation:
- The **AFTER UPDATE** trigger executes **after** an update occurs on the `products` table.
- It updates the `updated_at` column to the current timestamp (`NOW()`) whenever a row is modified.

### Testing the Trigger:
```sql
UPDATE products SET price = 500 WHERE id = 1;
SELECT * FROM products WHERE id = 1;
```

Would you like an example of an **AFTER INSERT** or **BEFORE DELETE** trigger as well? 🚀

You can create a **trigger** in MySQL to automatically update the `total` in the `order` table whenever a new `order_item` is inserted, updated, or deleted.  

### **Step 1: Create a Trigger for Insert and Update**
```sql
DELIMITER $$

CREATE TRIGGER update_order_total_after_insert
AFTER INSERT ON `order_item`
FOR EACH ROW
BEGIN
    UPDATE `order`
    SET `total` = (SELECT SUM(sub_total) FROM `order_item` WHERE order_id = NEW.order_id)
    WHERE `id` = NEW.order_id;
END $$

DELIMITER ;
```

### **Step 2: Create a Trigger for Update on `order_item`**
```sql
DELIMITER $$

CREATE TRIGGER update_order_total_after_update
AFTER UPDATE ON `order_item`
FOR EACH ROW
BEGIN
    UPDATE `order`
    SET `total` = (SELECT SUM(sub_total) FROM `order_item` WHERE order_id = NEW.order_id)
    WHERE `id` = NEW.order_id;
END $$

DELIMITER ;
```

### **Step 3: Create a Trigger for Delete**
```sql
DELIMITER $$

CREATE TRIGGER update_order_total_after_delete
AFTER DELETE ON `order_item`
FOR EACH ROW
BEGIN
    UPDATE `order`
    SET `total` = (SELECT COALESCE(SUM(sub_total), 0) FROM `order_item` WHERE order_id = OLD.order_id)
    WHERE `id` = OLD.order_id;
END $$

DELIMITER ;
```

### **Explanation:**
- `AFTER INSERT` trigger updates the total when a new `order_item` is added.
- `AFTER UPDATE` trigger updates the total when an existing `order_item` is modified.
- `AFTER DELETE` trigger updates the total when an `order_item` is removed (handling cases where all items are deleted using `COALESCE` to prevent NULL values).

Now, whenever you insert, update, or delete items from `order_item`, the total in the `order` table will be automatically updated. 🚀  

Would you like any modifications, such as logging changes to a history table?

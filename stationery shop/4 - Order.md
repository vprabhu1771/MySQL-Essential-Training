```
CREATE TABLE `order` (
  `id` INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
  `order_number` VARCHAR(255),
  `order_date` VARCHAR(255),
  `total` VARCHAR(255)
);
```

Here’s how you can insert an order with two items (`A4 Paper` and `XO Pen Blue`) into your `order` and `order_item` tables:  

### **Step 1: Insert an Order**  
```sql
INSERT INTO `order` (`order_number`, `order_date`, `total`)  
VALUES ("ORD20240312", "2025-03-12", "500");  
```
> Replace `"500"` with the actual total amount based on item prices.

### **Step 2: Get the Last Inserted Order ID**  
```sql
SELECT LAST_INSERT_ID();
```
> Assume the returned `id` is `1` for this example.

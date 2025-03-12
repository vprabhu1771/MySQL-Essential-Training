```
CREATE TABLE `order_item` (
  `id` INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
  `order_id` INT,
  `name` VARCHAR(255),
  `qty` INT,
  `sub_total` INT
);
```



### **Step 3: Insert Order Items**  
```sql
INSERT INTO `order_item` (`order_id`, `name`, `qty`, `sub_total`)  
VALUES  
(1, "A4 Paper", 2, 200),  
(1, "XO Pen Blue", 2, 100);
```
> Replace `200` and `100` with actual prices multiplied by the quantity.  

To calculate the total for a specific `order_id`, you can use the following SQL query:  

```sql
SELECT SUM(sub_total) AS total 
FROM `order_item` 
WHERE order_id = 1;
```

This will return the total amount for `order_id = 1` by summing up the `sub_total` values of its associated items.  

If you also want to **update the total in the `order` table**, you can run:  

```sql
UPDATE `order` 
SET `total` = (SELECT SUM(sub_total) FROM `order_item` WHERE order_id = 1) 
WHERE `id` = 1;
```

Would you like to automate this update using a **trigger** in MySQL?

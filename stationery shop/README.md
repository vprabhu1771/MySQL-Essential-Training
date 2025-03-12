```
CREATE DATABASE `stationery_shop`;
```

```
DROP DATABASE `stationery_shop`;
```

```
CREATE TABLE `category` (
  `id` INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
  `name` VARCHAR(255)
);
```

```
INSERT INTO `category` (`name`) VALUES ("PAPER"), ("PEN"), ("pencil"). ("INK");
```

```
CREATE TABLE `sub_category` (
   `id` INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
   category_id INT NULL,
   `name` VARCHAR(255)
);
```

```
INSERT INTO `sub_category` (`category_id`, `name`) VALUES
(1, "A4 Paper"),
(1, "A3 Paper"),
(1, "Legal Paper"),
(1, "A4 White Bond Paper"),
(1, "A4 Green Bond Paper"),
(1, "Legal Green Bond Papaer")
(2, "XO Pen Blue"),
(2, "XO Pen Black"),
(2, "XO Pen RED");
```

```
CREATE TABLE `order` (
  `id` INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
  `order_number` VARCHAR(255),
  `order_date` VARCHAR(255),
  `total` VARCHAR(255)
);
```

```
CREATE TABLE `order_item` (
  `id` INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
  `order_id` INT,
  `name` VARCHAR(255),
  `qty` INT,
  `sub_total` INT
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

### **Step 3: Insert Order Items**  
```sql
INSERT INTO `order_item` (`order_id`, `name`, `qty`, `sub_total`)  
VALUES  
(1, "A4 Paper", 2, 200),  
(1, "XO Pen Blue", 2, 100);
```
> Replace `200` and `100` with actual prices multiplied by the quantity.  


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

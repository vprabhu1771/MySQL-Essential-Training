# 1 - Table Setup

```
CREATE TABLE `staff` (
    `id` INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    `name` VARCHAR(255),
    `age` INT,
    `address` TEXT,
    `salary` DECIMAL(10,2)
);
```

```
INSERT INTO `staff` (`name`, `age`, `address`, `salary`) VALUES
("Ramesh", 22, "Cuddalore", 4000),
("Kumar", 42, "Nellikuppam", 3000),
("Raja", 30, "Kurinjipadi", 4000),
("Jackson", 18, "Vandipalayam", 13000),
("Vigneshwaran", 18, "Thiruvanthipuram", 17500),
("Anu", 28, "Kullanchavadi",9000),
("Abi", 29, "Cuddalore-OT",20000),
("Suresh", 40, "Panruti", 750);
```

# 2 - BETWEEN

SQL statement selects all staff with a age between 30 and 50:

```
SELECT * FROM `staff` WHERE `age` BETWEEN 30 AND 50;
```

```
+----+--------+------+-------------+---------+
| id | name   | age  | address     | salary  |
+----+--------+------+-------------+---------+
|  2 | Kumar  |   42 | Nellikuppam | 3000.00 |
|  3 | Raja   |   30 | Kurinjipadi | 4000.00 |
|  8 | Suresh |   40 | Panruti     |  750.00 |
+----+--------+------+-------------+---------+
3 rows in set (0.011 sec)
```

SQL statement selects all staff with a salary between 10000 and 20000:

```
SELECT * FROM `staff` WHERE `salary` BETWEEN 10000 AND 20000;
```

```
+----+--------------+------+------------------+----------+
| id | name         | age  | address          | salary   |
+----+--------------+------+------------------+----------+
|  4 | Jackson      |   18 | Vandipalayam     | 13000.00 |
|  5 | Vigneshwaran |   18 | Thiruvanthipuram | 17500.00 |
|  7 | Abi          |   29 | Cuddalore-OT     | 20000.00 |
+----+--------------+------+------------------+----------+
3 rows in set (0.000 sec)
```
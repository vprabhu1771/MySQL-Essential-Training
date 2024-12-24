Here’s the SQL script to create the `ticket_type` table and populate it with the given data:

```sql
CREATE TABLE ticket_type (
    id INT PRIMARY KEY,
    type VARCHAR(50) NOT NULL,
    price DECIMAL(10, 2) NOT NULL
);

INSERT INTO ticket_type (id, type, price) VALUES
(1, 'Normal Adult', 200),
(2, 'Normal Child', 80),
(3, 'Foreigner Adult', 1100),
(4, 'Foreigner Child', 800);
```

### Explanation:
- `id`: An integer serving as the primary key.
- `type`: A string field for the ticket type.
- `price`: A decimal field for the price, with support for up to two decimal places.

Let me know if you need any modifications or additional functionality!


-----

Here’s the SQL script to create the `ticket` table with the specified columns:

To include `adult_unit_price` and `children_unit_price` when inserting a ticket based on the selected `ticket_type_id`, you can modify the `ticket` table schema to include these columns. Then, a `BEFORE INSERT` trigger can automatically fetch the unit prices from the `ticket_type` table and populate these columns.

### Updated `ticket` Table Schema
```sql
CREATE TABLE ticket (
    id INT PRIMARY KEY AUTO_INCREMENT,
    ticket_type_id INT NOT NULL,
    no_adult INT NOT NULL DEFAULT 0,
    no_children INT NOT NULL DEFAULT 0,
    adult_unit_price DECIMAL(10, 2) NOT NULL,
    children_unit_price DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (ticket_type_id) REFERENCES ticket_type(id)
);
```

### Adding a Trigger for Auto-Insertion of Prices
```sql
DELIMITER $$

CREATE TRIGGER set_ticket_prices
BEFORE INSERT ON ticket
FOR EACH ROW
BEGIN
    DECLARE ticket_price DECIMAL(10, 2);

    -- Fetch the ticket price from ticket_type
    SELECT price INTO ticket_price
    FROM ticket_type
    WHERE id = NEW.ticket_type_id;

    -- Set adult and children unit prices based on ticket_type
    IF NEW.ticket_type_id IN (1, 3) THEN
        -- For adult ticket types (Normal Adult, Foreigner Adult)
        SET NEW.adult_unit_price = ticket_price;
        SET NEW.children_unit_price = 0;
    ELSE
        -- For child ticket types (Normal Child, Foreigner Child)
        SET NEW.adult_unit_price = 0;
        SET NEW.children_unit_price = ticket_price;
    END IF;
END$$

DELIMITER ;
```

### Explanation of the Trigger:
1. **`BEFORE INSERT`**: The trigger runs before a new record is inserted into the `ticket` table.
2. **`DECLARE ticket_price`**: A variable to store the ticket price fetched from `ticket_type`.
3. **`SELECT price`**: Fetches the price from the `ticket_type` table based on the `ticket_type_id`.
4. **Condition on `ticket_type_id`**:
   - For adult ticket types (`id` 1 and 3), the `adult_unit_price` is set, and `children_unit_price` is 0.
   - For child ticket types (`id` 2 and 4), the `children_unit_price` is set, and `adult_unit_price` is 0.

### Insert Example
```sql
-- Insert data into the ticket table
INSERT INTO ticket (ticket_type_id, no_adult, no_children, adult_unit_price, children_unit_price) VALUES
(1, 2, 0, 200, 0),  -- Normal Adult: 2 adults, no children, adult price 200
(2, 0, 3, 0, 80),   -- Normal Child: no adults, 3 children, child price 80
(3, 1, 0, 1100, 0), -- Foreigner Adult: 1 adult, no children, adult price 1100
(4, 0, 4, 0, 800);  -- Foreigner Child: no adults, 4 children, child price 800
```

### Result
The `adult_unit_price` and `children_unit_price` fields will be automatically populated based on the `ticket_type`.

### Query to Verify
```sql
SELECT * FROM ticket;
```

Let me know if you need further assistance!



----

Here's an example of a query to fetch the ticket details, including the prices for adults and children, along with the total prices:

### Query to Fetch Ticket Details Including Unit Prices
```sql
SELECT 
    t.id AS ticket_id,
    tt.type AS ticket_type,
    t.no_adult AS number_of_adults,
    t.no_children AS number_of_children,
    t.adult_unit_price AS adult_unit_price,
    t.children_unit_price AS children_unit_price,
    (t.no_adult * t.adult_unit_price) AS total_adult_price,
    (t.no_children * t.children_unit_price) AS total_children_price,
    ((t.no_adult * t.adult_unit_price) + (t.no_children * t.children_unit_price)) AS total_price
FROM 
    ticket t
JOIN 
    ticket_type tt
ON 
    t.ticket_type_id = tt.id;
```

### Explanation:
1. **`t.id AS ticket_id`**: Fetches the ticket's unique ID.
2. **`tt.type AS ticket_type`**: Fetches the type of ticket from the `ticket_type` table.
3. **`t.no_adult AS number_of_adults`**: Fetches the number of adults for the ticket.
4. **`t.no_children AS number_of_children`**: Fetches the number of children for the ticket.
5. **`t.adult_unit_price AS adult_unit_price`**: Fetches the unit price for adults.
6. **`t.children_unit_price AS children_unit_price`**: Fetches the unit price for children.
7. **`total_adult_price` and `total_children_price`**: Calculates the total price for adults and children based on the respective unit prices.
8. **`total_price`**: Calculates the total ticket price by summing the total adult and children prices.

### Sample Output:
| ticket_id | ticket_type     | number_of_adults | number_of_children | adult_unit_price | children_unit_price | total_adult_price | total_children_price | total_price |
|-----------|-----------------|------------------|--------------------|------------------|---------------------|-------------------|----------------------|-------------|
| 1         | Normal Adult    | 2                | 0                  | 200              | 0                   | 400               | 0                    | 400         |
| 2         | Normal Child    | 0                | 3                  | 0                | 80                  | 0                 | 240                  | 240         |
| 3         | Foreigner Adult | 1                | 0                  | 1100             | 0                   | 1100              | 0                    | 1100        |
| 4         | Foreigner Child | 0                | 4                  | 0                | 800                 | 0                 | 3200                 | 3200        |

Let me know if you need any further adjustments or explanations!
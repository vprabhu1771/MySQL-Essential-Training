```sql
CREATE TABLE categories (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(150) NOT NULL,
    created_at TIMESTAMP NULL,
    updated_at TIMESTAMP NULL
);
CREATE TABLE brands (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(150) NOT NULL,
    created_at TIMESTAMP NULL,
    updated_at TIMESTAMP NULL
);
CREATE TABLE tax_slabs (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),                -- GST 5%, GST 12% etc
    percentage DECIMAL(5,2),         -- 5.00, 12.00, 18.00
    created_at TIMESTAMP NULL
);
CREATE TABLE products (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,

    category_id BIGINT NULL,
    brand_id BIGINT NULL,
    tax_slab_id BIGINT NULL,

    name VARCHAR(200) NOT NULL,
    barcode VARCHAR(100) UNIQUE,
    hsn_code VARCHAR(20),

    cost_price DECIMAL(12,2) DEFAULT 0,
    retail_price DECIMAL(12,2) DEFAULT 0,
    wholesale_price DECIMAL(12,2) DEFAULT 0,

    is_tax_inclusive BOOLEAN DEFAULT 1,

    is_active BOOLEAN DEFAULT 1,

    created_at TIMESTAMP NULL,
    updated_at TIMESTAMP NULL,

    FOREIGN KEY (category_id) REFERENCES categories(id) ON DELETE SET NULL,
    FOREIGN KEY (brand_id) REFERENCES brands(id) ON DELETE SET NULL,
    FOREIGN KEY (tax_slab_id) REFERENCES tax_slabs(id) ON DELETE SET NULL
);
CREATE TABLE product_stocks (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    product_id BIGINT NOT NULL,
    current_stock DECIMAL(12,2) DEFAULT 0,
    minimum_stock DECIMAL(12,2) DEFAULT 0,
    created_at TIMESTAMP NULL,
    updated_at TIMESTAMP NULL,

    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE
);
CREATE TABLE stock_movements (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    product_id BIGINT NOT NULL,
    type ENUM('opening','purchase','sale','adjustment','return'),
    quantity DECIMAL(12,2),
    reference_id BIGINT NULL,
    notes TEXT NULL,
    created_at TIMESTAMP NULL,

    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE
);
CREATE TABLE sales (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,

    invoice_no VARCHAR(50),
    customer_name VARCHAR(200) NULL,
    customer_gstin VARCHAR(20) NULL,

    sub_total DECIMAL(12,2),
    cgst_total DECIMAL(12,2),
    sgst_total DECIMAL(12,2),
    igst_total DECIMAL(12,2),

    discount_total DECIMAL(12,2),
    grand_total DECIMAL(12,2),

    payment_mode ENUM('cash','upi','card','mixed'),

    created_at TIMESTAMP NULL,
    updated_at TIMESTAMP NULL
);
CREATE TABLE sale_items (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,

    sale_id BIGINT NOT NULL,
    product_id BIGINT NOT NULL,

    quantity DECIMAL(12,2),
    unit_price DECIMAL(12,2),

    tax_percent DECIMAL(5,2),
    cgst DECIMAL(12,2),
    sgst DECIMAL(12,2),
    igst DECIMAL(12,2),

    total DECIMAL(12,2),

    created_at TIMESTAMP NULL,

    FOREIGN KEY (sale_id) REFERENCES sales(id) ON DELETE CASCADE,
    FOREIGN KEY (product_id) REFERENCES products(id)
);

CREATE TABLE purchases (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    supplier_name VARCHAR(200),
    supplier_gstin VARCHAR(20),
    total_taxable DECIMAL(12,2),
    cgst_total DECIMAL(12,2),
    sgst_total DECIMAL(12,2),
    igst_total DECIMAL(12,2),
    grand_total DECIMAL(12,2),
    created_at TIMESTAMP NULL
);

CREATE TABLE purchase_items (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    purchase_id BIGINT,
    product_id BIGINT,
    quantity DECIMAL(12,2),
    unit_price DECIMAL(12,2),
    tax_percent DECIMAL(5,2),
    cgst DECIMAL(12,2),
    sgst DECIMAL(12,2),
    igst DECIMAL(12,2),
    total DECIMAL(12,2),
    FOREIGN KEY (purchase_id) REFERENCES purchases(id) ON DELETE CASCADE
);

INSERT INTO purchases 
(supplier_name, supplier_gstin, total_taxable, cgst_total, sgst_total, igst_total, grand_total, created_at)
VALUES
('ABC Suppliers', '33AAAAA0000A1Z5', 100000.00, 6000.00, 6000.00, 0.00, 112000.00, NOW());

INSERT INTO purchase_items
(purchase_id, product_id, quantity, unit_price, tax_percent, cgst, sgst, igst, total)
VALUES
(1, 1, 20, 5000.00, 12.00, 6000.00, 6000.00, 0.00, 112000.00);

INSERT INTO stock_movements
(product_id, type, quantity, reference_id, notes, created_at)
VALUES
(1, 'purchase', 20, 1, 'Purchase Stock Entry', NOW());

UPDATE product_stocks
SET current_stock = current_stock + 20
WHERE product_id = 1;

INSERT INTO sales
(invoice_no, customer_name, customer_gstin, sub_total, cgst_total, sgst_total, igst_total, discount_total, grand_total, payment_mode, created_at)
VALUES
('INV001', 'Ravi Kumar', NULL, 17000.00, 1020.00, 1020.00, 0.00, 0.00, 19040.00, 'cash', NOW());

INSERT INTO sale_items
(sale_id, product_id, quantity, unit_price, tax_percent, cgst, sgst, igst, total, created_at)
VALUES
(1, 1, 2, 8500.00, 12.00, 1020.00, 1020.00, 0.00, 19040.00, NOW());

INSERT INTO stock_movements
(product_id, type, quantity, reference_id, notes, created_at)
VALUES
(1, 'sale', -2, 1, 'Sale Stock Deduction INV001', NOW());

INSERT INTO purchases 
(supplier_name, supplier_gstin, total_taxable, cgst_total, sgst_total, igst_total, grand_total, created_at)
VALUES
('Mumbai Electronics', '27BBBBB0000B1Z6', 50000.00, 0.00, 0.00, 2500.00, 52500.00, NOW());

INSERT INTO purchase_items
(purchase_id, product_id, quantity, unit_price, tax_percent, cgst, sgst, igst, total)
VALUES
(2, 1, 10, 5000.00, 5.00, 0.00, 0.00, 2500.00, 52500.00);

INSERT INTO stock_movements
(product_id, type, quantity, reference_id, notes, created_at)
VALUES
(1, 'purchase', 10, 2, 'Interstate Purchase', NOW());

UPDATE product_stocks
SET current_stock = current_stock + 10
WHERE product_id = 1;

INSERT INTO sales
(invoice_no, customer_name, customer_gstin, sub_total, cgst_total, sgst_total, igst_total, discount_total, grand_total, payment_mode, created_at)
VALUES
('INV002', 'Tech Solutions Pvt Ltd', '33ABCDE1234F1Z5', 25500.00, 1530.00, 1530.00, 0.00, 0.00, 28560.00, 'upi', NOW());

INSERT INTO sale_items
(sale_id, product_id, quantity, unit_price, tax_percent, cgst, sgst, igst, total, created_at)
VALUES
(2, 1, 3, 8500.00, 12.00, 1530.00, 1530.00, 0.00, 28560.00, NOW());

INSERT INTO stock_movements
(product_id, type, quantity, reference_id, notes, created_at)
VALUES
(1, 'sale', -3, 2, 'B2B Sale INV002', NOW());

UPDATE product_stocks
SET current_stock = current_stock - 3
WHERE product_id = 1;
```
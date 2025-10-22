# hello_world
-- Drop tables if they exist
DROP TABLE IF EXISTS orders;
DROP TABLE IF EXISTS products;
DROP TABLE IF EXISTS customers;

-- Customers Table
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100) UNIQUE NOT NULL,
    phone VARCHAR(20),
    created_at TIMESTAMP DEFAULT NOW()
);

-- Products Table
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    category VARCHAR(50),
    price DECIMAL(10,2),
    stock_quantity INT,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Orders Table
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers(customer_id),
    product_id INT REFERENCES products(product_id),
    order_date TIMESTAMP DEFAULT NOW(),
    quantity INT CHECK (quantity > 0),
    total_amount DECIMAL(10,2)
);

-- Insert sample data

-- Customers
INSERT INTO customers (first_name, last_name, email, phone) VALUES
('Alice', 'Wanjiku', 'alice@example.com', '0722000001'),
('Brian', 'Otieno', 'brian@example.com', '0722000002'),
('Clara', 'Mutua', 'clara@example.com', '0722000003'),
('Daniel', 'Mwangi', 'daniel@example.com', '0722000004'),
('Eva', 'Kariuki', 'eva@example.com', '0722000005');

-- Products
INSERT INTO products (product_name, category, price, stock_quantity) VALUES
('Laptop', 'Electronics', 85000.00, 10),
('Wireless Mouse', 'Accessories', 1500.00, 50),
('Smartphone', 'Electronics', 60000.00, 20),
('Headphones', 'Accessories', 3500.00, 30),
('Office Chair', 'Furniture', 12000.00, 15);

-- Orders
INSERT INTO orders (customer_id, product_id, quantity, total_amount) VALUES
(1, 1, 1, 85000.00),
(2, 2, 2, 3000.00),
(3, 3, 1, 60000.00),
(4, 5, 1, 12000.00),
(5, 4, 3, 10500.00);

SELECT * FROM customers;

SELECT o.order_id, c.first_name, p.product_name, o.quantity, o.total_amount
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
JOIN products p ON o.product_id = p.product_id;


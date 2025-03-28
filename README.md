# Retail Store SQL Database

## 📌 Overview
This repository contains SQL scripts to set up and manage a **Retail Store Database**. The database includes tables for managing customers, products, orders, and order details, ensuring smooth inventory and sales tracking.

## 📂 Folder Structure
```
Retail-Store-SQL/
│── schema.sql          # SQL script for creating tables
│── insert_data.sql     # SQL script for inserting sample data
│── queries.sql         # SQL queries for retrieving and analyzing data
│── README.md           # Project documentation
```

## 🚀 Getting Started
### **Step 1: Clone the Repository**
```bash
git clone https://github.com/your-username/Retail-Store-SQL.git
cd Retail-Store-SQL
```

### **Step 2: Create the Database**
```sql
CREATE DATABASE RetailStore;
USE RetailStore;
```

### **Step 3: Run SQL Scripts**
1. Execute `schema.sql` to create tables:
   ```sql
   SOURCE schema.sql;
   ```
2. Insert sample data:
   ```sql
   SOURCE insert_data.sql;
   ```
3. Run queries from `queries.sql` to analyze data.

## 📊 Database Schema
### **Tables:**
1. `Customers` - Stores customer details.
2. `Products` - Stores product details (name, category, price, stock).
3. `Orders` - Stores order details placed by customers.
4. `OrderDetails` - Stores product-wise details of each order.

## 🔍 Sample Queries
- Get all customers:
  ```sql
  SELECT * FROM Customers;
  ```
- Retrieve product stock details:
  ```sql
  SELECT name, category, price, stock_quantity FROM Products WHERE stock_quantity > 0;
  ```
- Fetch all orders with customer details:
  ```sql
  SELECT Orders.order_id, Customers.name, Orders.order_date, Orders.total_amount
  FROM Orders
  JOIN Customers ON Orders.customer_id = Customers.customer_id;
  ``
---
🚀 **Happy Coding!** 🎯
-------------------------------------------------------------------------------------------------------------------------------------------------------------------
# SQL.queries
CREATE DATABASE RetailStore;
USE RetailStore;

CREATE TABLE Customers (
    customer_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone VARCHAR(15),
    address TEXT
);

CREATE TABLE Products (
    product_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    category VARCHAR(50),
    price DECIMAL(10,2) NOT NULL,
    stock_quantity INT NOT NULL
);

CREATE TABLE Orders (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT,
    order_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    total_amount DECIMAL(10,2),
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id) ON DELETE CASCADE
);

CREATE TABLE OrderDetails (
    order_detail_id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT NOT NULL,
    price DECIMAL(10,2),
    FOREIGN KEY (order_id) REFERENCES Orders(order_id) ON DELETE CASCADE,
    FOREIGN KEY (product_id) REFERENCES Products(product_id) ON DELETE CASCADE
);
-- Insert customers
INSERT INTO Customers (name, email, phone, address)
VALUES 
('John Doe', 'john@example.com', '9876543210', '123 Main St, NY'),
('Jane Smith', 'jane@example.com', '8765432109', '456 Park Ave, CA');

-- Insert products
INSERT INTO Products (name, category, price, stock_quantity)
VALUES 
('Laptop', 'Electronics', 750.00, 10),
('Phone', 'Electronics', 500.00, 20),
('Shoes', 'Fashion', 50.00, 30);

-- Insert orders
INSERT INTO Orders (customer_id, total_amount)
VALUES 
(1, 1250.00),
(2, 500.00);

-- Insert order details
INSERT INTO OrderDetails (order_id, product_id, quantity, price)
VALUES 
(1, 1, 1, 750.00),
(1, 2, 1, 500.00),
(2, 2, 1, 500.00);
-- Get all customers
SELECT * FROM Customers;

-- Get all products with stock availability
SELECT name, category, price, stock_quantity FROM Products WHERE stock_quantity > 0;

-- Get all orders with customer details
SELECT Orders.order_id, Customers.name, Orders.order_date, Orders.total_amount
FROM Orders
JOIN Customers ON Orders.customer_id = Customers.customer_id;

-- Get order details for a specific order
SELECT Orders.order_id, Products.name, OrderDetails.quantity, OrderDetails.price
FROM OrderDetails
JOIN Orders ON OrderDetails.order_id = Orders.order_id
JOIN Products ON OrderDetails.product_id = Products.product_id
WHERE Orders.order_id = 1;
-- Update product stock after a sale
UPDATE Products SET stock_quantity = stock_quantity - 1 WHERE product_id = 1;

-- Delete a customer and their orders (CASCADE DELETE)
DELETE FROM Customers WHERE customer_id = 2;

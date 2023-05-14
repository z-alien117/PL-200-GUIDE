# Relationships

## Customers table

```sql
CREATE TABLE Customers (
  CustomerID INT AUTO_INCREMENT PRIMARY KEY,
  Name VARCHAR(255),
  Address VARCHAR(255),
  CreditRating VARCHAR(10)
);

INSERT INTO Customers (Name, Address, CreditRating) VALUES
  ('John Doe', '123 Main St.', 'A'),
  ('Jane Smith', '456 Oak Ave.', 'B');
```

## Products table

```sql
CREATE TABLE Products (
  ProductID INT AUTO_INCREMENT PRIMARY KEY,
  Name VARCHAR(255),
  Quantity INT,
  Cost DECIMAL(10, 2),
  Price DECIMAL(10, 2)
);

INSERT INTO Products (Name, Quantity, Cost, Price) VALUES
  ('Product A', 100, 10.00, 20.00),
  ('Product B', 50, 15.00, 30.00);
```

## Orders table

```sql
CREATE TABLE Orders (
  OrderID INT AUTO_INCREMENT PRIMARY KEY,
  Date DATE,
  InvoiceNumber VARCHAR(20),
  Salesperson VARCHAR(255),
  CustomerID INT,
  FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);

INSERT INTO Orders (Date, InvoiceNumber, Salesperson, CustomerID) VALUES
  ('2023-05-01', 'INV-001', 'Mark Johnson', 1),
  ('2023-05-02', 'INV-002', 'Sarah Smith', 2);
```

## Line Items table

```sql
CREATE TABLE LineItems (
  LineItemID INT AUTO_INCREMENT PRIMARY KEY,
  OrderID INT,
  ProductID INT,
  Quantity INT,
  FOREIGN KEY (OrderID) REFERENCES Orders(OrderID),
  FOREIGN KEY (ProductID) REFERENCES Products(ProductID)
);

INSERT INTO LineItems (OrderID, ProductID, Quantity) VALUES
  (1, 1, 2),
  (1, 2, 1);
```

## Relationships query

```sql
SELECT o.OrderID, o.Date, o.InvoiceNumber, o.Salesperson,
       c.CustomerID, c.Name AS CustomerName, c.Address, c.CreditRating,
       li.LineItemID, li.Quantity,
       p.ProductID, p.Name AS ProductName, p.Quantity AS ProductQuantity, p.Cost, p.Price
FROM Orders o
JOIN Customers c ON o.CustomerID = c.CustomerID
JOIN LineItems li ON o.OrderID = li.OrderID
JOIN Products p ON li.ProductID = p.ProductID;
```
**Explanation:**

> This query performs JOIN operations between the tables to fetch related data. It retrieves information from the Orders, Customers, LineItems, and Products tables, connecting them based on the specified relationships.

The result of this query will include the following columns:

- **OrderID**: The ID of the order.
- **Date**: The date of the order.
- **InvoiceNumber**: The invoice number of the order.
- **Salesperson**: The name of the salesperson associated with the order.
- **CustomerID**: The ID of the customer associated with the order.
- **CustomerName**: The name of the customer associated with the order.
- **Address**: The address of the customer.
- **CreditRating**: The credit rating of the customer.
- **LineItemID**: The ID of the line item.
- **Quantity**: The quantity of the product in the line item.
- **ProductID**: The ID of the product associated with the line item.
- **ProductName**: The name of the product associated with the line item.
- **ProductQuantity**: The quantity of the product available.
- **Cost**: The cost of the product.
- **Price**: The selling price of the product.

> This query combines the data from multiple tables into a single result set, showcasing the relationships between the tables and providing a comprehensive view of the connected information.


## What are the Primary and Foreign keys?

> **Primary Key**: A primary key is a unique identifier for each record in a table. It is a column or set of columns that uniquely identifies each row in the table. Think of it as a "special ID" that is unique to each record. It helps to uniquely identify and distinguish one record from another. For example, in a table of customers, the Customer ID column can be designated as the primary key. Each customer will have a unique Customer ID assigned to them.

> **Foreign Key**: A foreign key is a column or set of columns in one table that refers to the primary key in another table. It establishes a relationship between two tables by linking the records in one table to the records in another table. It acts as a reference to a primary key in a different table. For example, in a table of orders, the Customer ID column can be a foreign key that references the Customer ID primary key in the customers table. It allows you to associate each order with the corresponding customer.
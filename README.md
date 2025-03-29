# SQL Mastery Project: Enterprise E-Commerce Database Roadmap

## 🌟 Project Overview

This project provides a comprehensive roadmap to master SQL programming using Microsoft SQL Server through the development of a full-scale, real-world enterprise **E-Commerce Database System**. The project will cover everything from schema design to advanced optimization, reporting, ETL, and internal SQL concepts. All features are introduced with practical examples, syntax, usage, and rationale.

---

## 🏦 Domain Structure

### ✂️ Core Modules:
- **Catalog**: Products, Categories, ProductVariants, Inventory
- **Customers**: Users, Addresses, Preferences, Wishlist
- **Orders**: Orders, OrderItems, Discounts, Coupons
- **Payments**: PaymentMethods, Transactions, Refunds
- **Deliveries**: DeliveryAddress, Shipments, Carriers
- **Reviews**: ProductRatings, Comments
- **Support**: Tickets, Chats
- **Logging**: SystemLogs, UserActions
- **Admin**: Roles, Permissions, StaffLogs
- **Audit**: ChangeTracking, History, DeletedRecords

---

## 🚀 SQL Mastery Roadmap

### ✅ Stage 1: Foundation – Core Tables and Data Modeling
- Skills: Data Types, Constraints, PK/FK, Normalization
- Deliverables:
  - Tables: Customers, Products, Categories, Orders, OrderItems
  - ERD (Entity Relationship Diagram)
  - Sample Insert Scripts

### 📊 Stage 2: Reporting & Aggregation Basics
- Skills: `GROUP BY`, `HAVING`, Aggregates, Filtering
- Reports:
  - Daily Sales
  - Revenue by Category
  - Low Stock Alerts
  - Orders Missing Payments

### 🔗 Stage 3: Intermediate Queries & Joins
- Skills: `JOIN`, Views, Subqueries
- Features:
  - `vw_CustomerPurchaseHistory`
  - `vw_ProductInventoryOverview`
  - Products Never Purchased

### 🧰 Stage 4: Window Functions & Analytics
- Skills: `ROW_NUMBER()`, `RANK()`, `LAG()`, Running Totals
- Use Cases:
  - Category-wise Sales Rank
  - Rolling Revenue
  - Repeat Buyers

### 🔄 Stage 5: CTEs, Recursion & Pagination
- Skills: CTEs, Recursive Queries, `OFFSET/FETCH`
- Examples:
  - Category Hierarchy
  - Paginated Products & Orders
  - Drill-down Queries

### ⚙️ Stage 6: Stored Procedures, Functions, Triggers
- Skills: T-SQL, Triggers, UDFs, Transactions
- Features:
  - `usp_PlaceOrder`
  - Trigger for Stock Update
  - Refund Handler
  - Delivery Estimate Function

### 📈 Stage 7: Advanced Reporting & Dashboards
- Skills: Materialized Views, Snapshots, Pivoting
- Dashboards:
  - Daily/Weekly/Monthly Trends
  - Product Performance
  - Abandoned Carts
  - Regional Revenue

### 🚚 Stage 8: Deliveries & Payment Modules
- Skills: Advanced Joins, Modular Schema
- Features:
  - `Shipments`, `Carriers`, `Tracking`
  - `Transactions`, `Refunds`, `GiftCards`
  - Delivery Failure Reports

### 🛠️ Stage 9: SQL Optimization & Indexing
- Skills: Indexing, SARGability, Execution Plans
- Topics:
  - Index Design (Clustered/Non-Clustered)
  - Query Rewrite Strategies
  - Monitoring Query Performance

### 💼 Stage 10: ETL & Warehousing
- Skills: ETL with T-SQL, Staging, Snapshots
- Features:
  - Daily Sales Fact Table
  - Rolling Aggregates
  - ETL Audit Tables

### 🔐 Stage 11: Security, Roles & Row-Level Access
- Skills: `GRANT`, `DENY`, RLS
- Use Cases:
  - Region-Based Row-Level Security
  - Role-Based Views
  - Secure Reporting Access

### 🔧 Stage 12: SQL Internals & Concurrency
- Skills: Transactions, Locks, Isolation Levels
- Topics:
  - Deadlocks
  - TempDB Usage
  - DMV Monitoring
  - Fill Factor, Page Splits

### ⛨️ Stage 13: Auditing & Logging
- Skills: Triggers, Change Data Capture
- Features:
  - Change Tracking Tables
  - Insert/Update/Delete Logs
  - Login Audit Trails

### 🛠️ Stage 14: Maintenance & DevOps
- Skills: CI/CD, Backups, Migrations
- Topics:
  - Script Deployment Pipelines
  - GitHub Actions / Azure DevOps for SQL
  - Scheduled Index Maintenance

---

## 📆 Final Output
A complete **SQL Enterprise e-Commerce Engine** including:
- Modular Schema
- Smart Reports
- Dashboards
- ETL Pipelines
- Materialized Views
- Paginated Queries
- Full SQL Internals Insight
- Optimized and Secure Architecture

---


### ✅ Stage 1: Foundation – Core Tables and Data Modeling

#### 🔬 Skills Covered:
- SQL Syntax: `CREATE TABLE`, `PRIMARY KEY`, `FOREIGN KEY`, `NOT NULL`, `UNIQUE`, `CHECK`
- Understanding data types (e.g., `INT`, `VARCHAR`, `DECIMAL`, `DATETIME`)
- Entity Relationship Design (ERD)

#### 💼 Objectives:
- Design and create the foundational schema for the e-commerce platform.
- Normalize the schema to remove redundancy and enforce data integrity.
- Insert sample data for querying.

#### 📄 Tables to Create:
1. `Customers`
2. `Categories`
3. `Products`
4. `ProductVariants`
5. `Orders`
6. `OrderItems`

#### ⚖️ Table Definitions:

```sql
CREATE TABLE Customers (
    CustomerId INT PRIMARY KEY IDENTITY,
    FullName NVARCHAR(100) NOT NULL,
    Email NVARCHAR(100) UNIQUE NOT NULL,
    Phone NVARCHAR(20),
    CreatedAt DATETIME DEFAULT GETDATE()
);

CREATE TABLE Categories (
    CategoryId INT PRIMARY KEY IDENTITY,
    Name NVARCHAR(100) NOT NULL,
    Description NVARCHAR(255)
);

CREATE TABLE Products (
    ProductId INT PRIMARY KEY IDENTITY,
    CategoryId INT NOT NULL,
    Name NVARCHAR(100) NOT NULL,
    Description NVARCHAR(255),
    Price DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (CategoryId) REFERENCES Categories(CategoryId)
);

CREATE TABLE ProductVariants (
    VariantId INT PRIMARY KEY IDENTITY,
    ProductId INT NOT NULL,
    SKU NVARCHAR(50) UNIQUE NOT NULL,
    Color NVARCHAR(50),
    Size NVARCHAR(20),
    StockQuantity INT NOT NULL,
    FOREIGN KEY (ProductId) REFERENCES Products(ProductId)
);

CREATE TABLE Orders (
    OrderId INT PRIMARY KEY IDENTITY,
    CustomerId INT NOT NULL,
    OrderDate DATETIME DEFAULT GETDATE(),
    Status NVARCHAR(50) DEFAULT 'Pending',
    FOREIGN KEY (CustomerId) REFERENCES Customers(CustomerId)
);

CREATE TABLE OrderItems (
    OrderItemId INT PRIMARY KEY IDENTITY,
    OrderId INT NOT NULL,
    VariantId INT NOT NULL,
    Quantity INT NOT NULL,
    PriceAtPurchase DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (OrderId) REFERENCES Orders(OrderId),
    FOREIGN KEY (VariantId) REFERENCES ProductVariants(VariantId)
);
```

#### 🧵 What, Why, How
- **What**: `Products`, `Customers`, `Orders` are fundamental entities in any e-commerce.
- **Why**: Normalized design improves data integrity, reduces redundancy.
- **How**: Use `FOREIGN KEY` to link related entities and ensure relational consistency.

#### 🔍 Sample Queries:

```sql
-- Get all customers
SELECT * FROM Customers;

-- Get all products under a specific category
SELECT p.* FROM Products p
JOIN Categories c ON p.CategoryId = c.CategoryId
WHERE c.Name = 'Electronics';

-- Get customer orders
SELECT o.* FROM Orders o
JOIN Customers c ON o.CustomerId = c.CustomerId
WHERE c.FullName = 'John Doe';

-- Get all variants for a product
SELECT v.* FROM ProductVariants v
JOIN Products p ON v.ProductId = p.ProductId
WHERE p.Name = 'Smartphone';

-- List all order items for a given customer
SELECT oi.*, p.Name AS ProductName, v.Color, v.Size
FROM OrderItems oi
JOIN Orders o ON oi.OrderId = o.OrderId
JOIN ProductVariants v ON oi.VariantId = v.VariantId
JOIN Products p ON v.ProductId = p.ProductId
WHERE o.CustomerId = 1;

-- Check stock availability for all products
SELECT p.Name, v.Color, v.Size, v.StockQuantity
FROM ProductVariants v
JOIN Products p ON v.ProductId = p.ProductId
WHERE v.StockQuantity > 0;

-- Orders placed within the last 30 days
SELECT * FROM Orders
WHERE OrderDate >= DATEADD(DAY, -30, GETDATE());

-- Products with no stock left
SELECT p.Name, v.Color, v.Size
FROM ProductVariants v
JOIN Products p ON v.ProductId = p.ProductId
WHERE v.StockQuantity = 0;
```

---

### 📊 Stage 2: Reporting & Aggregation Basics

#### 🔬 Skills Covered:
- Aggregate functions: `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`
- Grouping and Filtering with `GROUP BY`, `HAVING`
- Filtering data with `WHERE`, `IN`, `BETWEEN`, `LIKE`
- Sorting results with `ORDER BY`
- Using `DISTINCT` to eliminate duplicates
- Subqueries (Scalar, Correlated, `EXISTS`, `IN`, `ANY`, `ALL`)

#### 💼 Objectives:
- Generate insights and summaries from operational data
- Create reusable queries for dashboards and analytics
- Practice advanced filtering and data comparison logic

#### 🔍 Example Queries:

```sql
-- Total number of customers
SELECT COUNT(*) AS TotalCustomers FROM Customers;

-- Total orders per customer
SELECT CustomerId, COUNT(*) AS TotalOrders
FROM Orders
GROUP BY CustomerId
ORDER BY TotalOrders DESC;

-- Average order value
SELECT AVG(PriceAtPurchase * Quantity) AS AvgOrderValue
FROM OrderItems;

-- Daily sales revenue
SELECT CAST(OrderDate AS DATE) AS SaleDate,
       SUM(oi.PriceAtPurchase * oi.Quantity) AS DailyRevenue
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
GROUP BY CAST(OrderDate AS DATE)
ORDER BY SaleDate DESC;

-- Revenue per category
SELECT c.Name AS Category, 
       SUM(oi.PriceAtPurchase * oi.Quantity) AS Revenue
FROM OrderItems oi
JOIN ProductVariants v ON oi.VariantId = v.VariantId
JOIN Products p ON v.ProductId = p.ProductId
JOIN Categories c ON p.CategoryId = c.CategoryId
GROUP BY c.Name
ORDER BY Revenue DESC;

-- Top 5 best-selling products
SELECT TOP 5 p.Name, SUM(oi.Quantity) AS TotalSold
FROM OrderItems oi
JOIN ProductVariants v ON oi.VariantId = v.VariantId
JOIN Products p ON v.ProductId = p.ProductId
GROUP BY p.Name
ORDER BY TotalSold DESC;

-- Low stock alert (< 10 units)
SELECT p.Name, v.SKU, v.StockQuantity
FROM ProductVariants v
JOIN Products p ON v.ProductId = p.ProductId
WHERE v.StockQuantity < 10
ORDER BY v.StockQuantity ASC;

-- Customers with no orders (using NOT IN)
SELECT * FROM Customers
WHERE CustomerId NOT IN (
    SELECT DISTINCT CustomerId FROM Orders
);

-- Customers with no orders (using NOT EXISTS)
SELECT * FROM Customers c
WHERE NOT EXISTS (
    SELECT 1 FROM Orders o WHERE o.CustomerId = c.CustomerId
);

-- Products that have ever been ordered (using EXISTS)
SELECT * FROM Products p
WHERE EXISTS (
    SELECT 1 FROM OrderItems oi
    JOIN ProductVariants v ON oi.VariantId = v.VariantId
    WHERE v.ProductId = p.ProductId
);

-- Customers who have placed more than 1 order and ordered high value items (using subquery and HAVING)
SELECT CustomerId
FROM Orders o
WHERE o.OrderId IN (
    SELECT OrderId FROM OrderItems
    WHERE PriceAtPurchase > 1000
)
GROUP BY CustomerId
HAVING COUNT(*) > 1;

-- Customers with orders in ANY status: Delivered, Shipped
SELECT * FROM Customers
WHERE CustomerId = ANY (
    SELECT CustomerId FROM Orders WHERE Status IN ('Delivered', 'Shipped')
);
```

#### 🧵 What, Why, How
- **What**: Advanced filters allow comparison and precise data slicing.
- **Why**: Business logic often requires checking conditions like existence, membership, or value comparison across subqueries.
- **How**: Use `EXISTS` for correlated checks, `IN` for set membership, and scalar subqueries for dynamic filtering.

---

### 🔗 Stage 3: Intermediate Queries & Joins

#### 🔬 Skills Covered:
- Join types: `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `FULL OUTER JOIN`
- Multi-level joins
- Subqueries inside joins
- Creating reusable views
- Handling NULLs and unmatched rows

#### 💼 Objectives:
- Extract meaningful combined data across multiple entities
- Create reusable query layers using views
- Explore different join patterns for data completeness

#### 🔍 Example Queries:

```sql
-- List all customers and their orders (even if they haven’t placed any)
SELECT c.FullName, o.OrderId, o.OrderDate
FROM Customers c
LEFT JOIN Orders o ON c.CustomerId = o.CustomerId;

-- Products and their total ordered quantity
SELECT p.Name, SUM(oi.Quantity) AS TotalOrdered
FROM Products p
LEFT JOIN ProductVariants v ON p.ProductId = v.ProductId
LEFT JOIN OrderItems oi ON v.VariantId = oi.VariantId
GROUP BY p.Name;

-- Orders that have no items
SELECT o.OrderId, o.OrderDate
FROM Orders o
LEFT JOIN OrderItems oi ON o.OrderId = oi.OrderId
WHERE oi.OrderItemId IS NULL;

-- Full join to compare customers and support tickets
SELECT c.FullName, s.TicketId
FROM Customers c
FULL OUTER JOIN SupportTickets s ON c.CustomerId = s.CustomerId;

-- Products that are never ordered
SELECT p.Name
FROM Products p
LEFT JOIN ProductVariants v ON p.ProductId = v.ProductId
LEFT JOIN OrderItems oi ON v.VariantId = oi.VariantId
WHERE oi.OrderItemId IS NULL;

-- Orders and matching shipments (view for dispatch tracking)
CREATE VIEW vw_OrderDispatchStatus AS
SELECT o.OrderId, o.OrderDate, d.ShipmentDate, d.Status
FROM Orders o
LEFT JOIN Deliveries d ON o.OrderId = d.OrderId;

-- View: customer purchase history
CREATE VIEW vw_CustomerPurchaseHistory AS
SELECT c.FullName, p.Name AS ProductName, oi.Quantity, o.OrderDate
FROM Customers c
JOIN Orders o ON c.CustomerId = o.CustomerId
JOIN OrderItems oi ON o.OrderId = oi.OrderId
JOIN ProductVariants v ON oi.VariantId = v.VariantId
JOIN Products p ON v.ProductId = p.ProductId;

-- View: product inventory overview
CREATE VIEW vw_ProductInventoryOverview AS
SELECT p.Name, v.SKU, v.Color, v.Size, v.StockQuantity
FROM ProductVariants v
JOIN Products p ON v.ProductId = p.ProductId;
```

#### 🧵 What, Why, How
- **What**: Joins let us connect related data across normalized tables.
- **Why**: Business decisions often require data from multiple sources.
- **How**: Use `LEFT JOIN` for inclusiveness, `INNER JOIN` for strict matching, and views to encapsulate logic.

---

### 🧰 Stage 4: Window Functions & Analytics

#### 🔬 Skills Covered:
- `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`
- `LEAD()`, `LAG()`
- `NTILE()`
- Running totals & cumulative aggregates
- Partitioning vs ordering

#### 💼 Objectives:
- Perform advanced row-by-row analysis
- Rank and compare data within groups
- Calculate trends using historical comparisons

#### 🔍 Example Queries:

```sql
-- Rank products by total sales within each category
SELECT 
    c.Name AS Category, 
    p.Name AS ProductName,
    SUM(oi.Quantity) AS TotalSold,
    RANK() OVER (
        PARTITION BY c.Name        -- Restart ranking for each category
        ORDER BY SUM(oi.Quantity) DESC -- Rank products by total quantity sold
    ) AS SalesRank
FROM OrderItems oi
JOIN ProductVariants v ON oi.VariantId = v.VariantId
JOIN Products p ON v.ProductId = p.ProductId
JOIN Categories c ON p.CategoryId = c.CategoryId
GROUP BY c.Name, p.Name;

-- Assign a row number to each customer's orders (latest first)
SELECT 
    CustomerId, 
    OrderId, 
    OrderDate,
    ROW_NUMBER() OVER (
        PARTITION BY CustomerId         -- Restart numbering for each customer
        ORDER BY OrderDate DESC         -- Newest order gets row number 1
    ) AS OrderSequence
FROM Orders;

-- Calculate daily revenue and running total revenue
SELECT 
    CAST(o.OrderDate AS DATE) AS OrderDay,                             -- Extract date from datetime
    SUM(oi.PriceAtPurchase * oi.Quantity) AS DailyRevenue,            -- Revenue for that day
    SUM(SUM(oi.PriceAtPurchase * oi.Quantity)) OVER (
        ORDER BY CAST(o.OrderDate AS DATE)                            -- Calculate cumulative total by date
    ) AS RunningRevenue
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
GROUP BY CAST(o.OrderDate AS DATE);

-- Compare each customer's current and previous order totals
SELECT 
    o.CustomerId, 
    o.OrderId, 
    o.OrderDate,
    SUM(oi.PriceAtPurchase * oi.Quantity) AS OrderTotal,
    LAG(SUM(oi.PriceAtPurchase * oi.Quantity)) OVER (
        PARTITION BY o.CustomerId        -- Get previous order's value for same customer
        ORDER BY o.OrderDate             -- Order their history by date
    ) AS PreviousOrderTotal
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
GROUP BY o.CustomerId, o.OrderId, o.OrderDate;

-- Split customers into 4 groups based on their order count (quartiles)
SELECT 
    CustomerId, 
    COUNT(*) AS TotalOrders,
    NTILE(4) OVER (
        ORDER BY COUNT(*) DESC           -- Rank customers by order volume, highest first
    ) AS Quartile
FROM Orders
GROUP BY CustomerId;
```

#### 🧵 What, Why, How
- **What**: Window functions let you calculate values over a group of rows without collapsing them.
- **Why**: Crucial for analytics like rankings, trends, and comparisons that require both row-level detail and group-level calculations.
- **How**: Use `OVER()` with `PARTITION BY` and `ORDER BY` to define the scope and sequence of the calculation.

---
## 🪟 SQL Window Functions: Plain English Guide with Analogies

SQL Window Functions perform calculations across a set of table rows that are somehow related to the current row. Unlike GROUP BY, they do not reduce the number of rows. Instead, they "look across" rows and add extra columns with insights like rankings, previous values, and cumulative totals.

---

### 🔢 1. Cumulative Functions (Running Totals, Averages)

**Analogy**: Tracking your daily spending, summing as you go.

**SQL**: `SUM(Sales) OVER (ORDER BY Date)`

| Day       | Expense | Running Total |
|-----------|---------|----------------|
| Monday    | $10     | $10            |
| Tuesday   | $15     | $25            |
| Wednesday | $5      | $30            |

You can also use `AVG()`, `MAX()`, `MIN()` similarly.

---

### 📍 2. ROW_NUMBER() – Unique Row Numbers per Group

**Analogy**: Assigning roll numbers to students in each class.

**SQL**: `ROW_NUMBER() OVER (PARTITION BY ClassId ORDER BY Marks DESC)`

Each group (Class) starts from 1.

---

### 🏆 3. RANK() and DENSE_RANK() – Ranking with Ties

**Analogy**: Two students with same marks? Same rank.

- `RANK()` skips the next number (1,2,2,4)
- `DENSE_RANK()` doesn’t skip (1,2,2,3)

**SQL**:
```sql
RANK() OVER (PARTITION BY ClassId ORDER BY Marks DESC)
DENSE_RANK() OVER (PARTITION BY ClassId ORDER BY Marks DESC)
```

---

### 🔁 4. LAG() and LEAD() – Accessing Previous/Next Rows

**Analogy**: Looking at yesterday’s or tomorrow’s price.

**SQL**:
```sql
LAG(Salary) OVER (ORDER BY JoinDate)
LEAD(Salary) OVER (ORDER BY JoinDate)
```

---

### 🪜 5. NTILE(n) – Dividing Rows into Buckets

**Analogy**: Ranking players into 4 tiers based on performance.

**SQL**:
```sql
NTILE(4) OVER (ORDER BY Score DESC)
```

Creates 4 equally-sized groups.

---

### 🧠 6. FIRST_VALUE() and LAST_VALUE()

**Analogy**:
- `FIRST_VALUE()`: Topper’s score shown in all rows.
- `LAST_VALUE()`: Last person’s score shown in all rows.

**SQL**:
```sql
FIRST_VALUE(Salary) OVER (PARTITION BY Dept ORDER BY Salary DESC)
```

**⚠️ LAST_VALUE** often requires window frame:
```sql
LAST_VALUE(Salary) OVER (PARTITION BY Dept ORDER BY Salary ASC ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING)
```

---

### 🧾 Summary Table

| Function         | Purpose                                     | Analogy                                    |
|------------------|---------------------------------------------|--------------------------------------------|
| `SUM()`          | Running total                               | Daily money spent so far                   |
| `ROW_NUMBER()`   | Unique row number in group                  | Roll number in each class                  |
| `RANK()`         | Rank with gaps for ties                     | Student ranks with skipped numbers         |
| `DENSE_RANK()`   | Rank without gaps                           | Tied students get same rank (no skips)     |
| `LAG()`          | Previous row value                          | What was yesterday’s price?                |
| `LEAD()`         | Next row value                              | What’s tomorrow’s price?                   |
| `NTILE(4)`       | Divide into 4 groups                        | Performance tiers                          |
| `FIRST_VALUE()`  | First value in partition                    | Top scorer in your class                   |
| `LAST_VALUE()`   | Last value in partition                     | Most recent entry in group                 |

---

### 🔄 Stage 5: CTEs, Recursion & Pagination

#### 🔬 Skills Covered:
- Common Table Expressions (CTEs)
- Recursive CTEs
- Drill-down and hierarchy processing
- Pagination using `OFFSET` / `FETCH`

#### 💼 Objectives:
- Create readable multi-step queries using CTEs
- Handle hierarchical data like category trees
- Implement pagination for large result sets

#### 🔍 Example Queries:

```sql
-- 1. Basic CTE: Calculate total order amount per order
WITH OrderTotals AS (
    SELECT 
        o.OrderId,
        o.CustomerId,
        SUM(oi.PriceAtPurchase * oi.Quantity) AS TotalAmount
    FROM Orders o
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    GROUP BY o.OrderId, o.CustomerId
)
SELECT * FROM OrderTotals;

-- 2. Recursive CTE: Build category tree structure
WITH CategoryHierarchy AS (
    -- Base level (root categories with no parent)
    SELECT CategoryId, Name, ParentCategoryId, 0 AS Level
    FROM Categories
    WHERE ParentCategoryId IS NULL

    UNION ALL

    -- Recursive part (children of each category)
    SELECT c.CategoryId, c.Name, c.ParentCategoryId, ch.Level + 1
    FROM Categories c
    JOIN CategoryHierarchy ch ON c.ParentCategoryId = ch.CategoryId
)
SELECT * FROM CategoryHierarchy
ORDER BY Level, Name;

-- 3. Simple pagination (Page 1: First 10 products)
SELECT ProductId, Name, Price
FROM Products
ORDER BY Name
OFFSET 0 ROWS FETCH NEXT 10 ROWS ONLY;  -- Skip 0 rows, return next 10

-- 4. Pagination (Page 2: Rows 11-20)
SELECT ProductId, Name, Price
FROM Products
ORDER BY Name
OFFSET 10 ROWS FETCH NEXT 10 ROWS ONLY;  -- Skip first 10 rows

-- 5. Pagination with filters and total count using CTE
WITH FilteredProducts AS (
    SELECT ProductId, Name, Price
    FROM Products
    WHERE Price BETWEEN 100 AND 500   -- Filter by price range
)
SELECT *
FROM FilteredProducts
ORDER BY Name
OFFSET 0 ROWS FETCH NEXT 5 ROWS ONLY;  -- Apply pagination to filtered results

-- 6. Customer order history - paginated with search by name
DECLARE @CustomerName NVARCHAR(100) = 'John';
DECLARE @PageNumber INT = 2;
DECLARE @PageSize INT = 5;

SELECT o.OrderId, o.OrderDate, c.FullName
FROM Orders o
JOIN Customers c ON o.CustomerId = c.CustomerId
WHERE c.FullName LIKE @CustomerName + '%'
ORDER BY o.OrderDate DESC
OFFSET (@PageNumber - 1) * @PageSize ROWS FETCH NEXT @PageSize ROWS ONLY;
-- Page 2: skips first 5 rows, shows rows 6-10

-- 7. Paginated view of best-selling products
WITH ProductSales AS (
    SELECT 
        p.ProductId, 
        p.Name, 
        SUM(oi.Quantity) AS TotalSold
    FROM OrderItems oi
    JOIN ProductVariants v ON oi.VariantId = v.VariantId
    JOIN Products p ON v.ProductId = p.ProductId
    GROUP BY p.ProductId, p.Name
)
SELECT *
FROM ProductSales
ORDER BY TotalSold DESC
OFFSET 0 ROWS FETCH NEXT 10 ROWS ONLY;  -- Top 10 sellers, page 1
```

#### 🧵 What, Why, How
- **What**: CTEs simplify logic for multi-step or recursive problems.
- **Why**: Improves readability and reuse; pagination avoids data overload on UI.
- **How**: Use `WITH` to define reusable subqueries, and `OFFSET/FETCH` to implement pagination logic for any query result.

---

### ⚙️ Stage 6: Stored Procedures, Functions, Triggers

#### 🔬 Skills Covered:
- T-SQL procedural programming
- Input/output parameters
- Transactions and error handling
- User-defined functions (scalar and table-valued)
- DML Triggers (AFTER INSERT, UPDATE, DELETE)

#### 💼 Objectives:
- Encapsulate business logic using procedures
- Automate data manipulation using triggers
- Reuse logic through modular functions
- Build detailed reports combining multiple SQL skills

#### 🔍 Example Procedures, Functions, Triggers:

```sql
-- Stored Procedure: Place a new order (expanded with comments)
CREATE PROCEDURE usp_PlaceOrder
    @CustomerId INT,
    @OrderItems OrderItemType READONLY, -- Table-Valued Parameter: Contains multiple order items
    @NewOrderId INT OUTPUT               -- Output parameter to return the new OrderId
AS
BEGIN
    SET NOCOUNT ON;
    BEGIN TRY
        BEGIN TRANSACTION; -- Start a transaction for atomicity

        -- Insert new order into Orders table
        INSERT INTO Orders (CustomerId, OrderDate, Status)
        VALUES (@CustomerId, GETDATE(), 'Pending');

        -- Capture the generated OrderId
        SET @NewOrderId = SCOPE_IDENTITY();

        -- Insert all order items from the TVP
        INSERT INTO OrderItems (OrderId, VariantId, Quantity, PriceAtPurchase)
        SELECT @NewOrderId, VariantId, Quantity, PriceAtPurchase
        FROM @OrderItems;

        COMMIT TRANSACTION; -- Commit only if all inserts succeed
    END TRY
    BEGIN CATCH
        ROLLBACK TRANSACTION; -- Rollback on any failure
        THROW; -- Re-throw the error for visibility
    END CATCH
END;

-- Scalar Function: Calculate discount by tier
CREATE FUNCTION fn_GetCustomerDiscount (@TotalAmount DECIMAL(10,2))
RETURNS DECIMAL(5,2)
AS
BEGIN
    DECLARE @DiscountRate DECIMAL(5,2);
    IF @TotalAmount >= 1000 SET @DiscountRate = 0.10;
    ELSE IF @TotalAmount >= 500 SET @DiscountRate = 0.05;
    ELSE SET @DiscountRate = 0.00;
    RETURN @DiscountRate;
END;

-- AFTER INSERT Trigger: Auto-update stock levels
CREATE TRIGGER trg_UpdateStockAfterOrder
ON OrderItems
AFTER INSERT
AS
BEGIN
    -- Reduce stock for each product variant in the order
    UPDATE v
    SET v.StockQuantity = v.StockQuantity - i.Quantity
    FROM ProductVariants v
    JOIN inserted i ON v.VariantId = i.VariantId;
END;

-- Trigger: Log user deletes to an audit table
CREATE TRIGGER trg_LogDeletedUsers
ON Customers
AFTER DELETE
AS
BEGIN
    INSERT INTO AuditDeletedCustomers (CustomerId, FullName, Email, DeletedAt)
    SELECT CustomerId, FullName, Email, GETDATE()
    FROM deleted;
END;

-- Inline Table-Valued Function: Return customer’s active orders
CREATE FUNCTION fn_GetOpenOrders (@CustomerId INT)
RETURNS TABLE
AS
RETURN (
    SELECT OrderId, OrderDate, Status
    FROM Orders
    WHERE CustomerId = @CustomerId AND Status IN ('Pending', 'Processing')
);
```

---

#### 🔢 Advanced Reporting Using All Skills So Far

```sql
-- Generate a customer purchase report with:
-- - Total orders
-- - Total revenue
-- - Average order value
-- - Rank by revenue
-- - Include only customers who made purchases in the last 90 days
WITH CustomerOrderStats AS (
    SELECT 
        c.CustomerId,
        c.FullName,
        COUNT(DISTINCT o.OrderId) AS TotalOrders,
        SUM(oi.PriceAtPurchase * oi.Quantity) AS TotalRevenue,
        AVG(oi.PriceAtPurchase * oi.Quantity) AS AvgOrderValue
    FROM Customers c
    JOIN Orders o ON c.CustomerId = o.CustomerId
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    WHERE o.OrderDate >= DATEADD(DAY, -90, GETDATE())
    GROUP BY c.CustomerId, c.FullName
),
RankedStats AS (
    SELECT *,
           RANK() OVER (ORDER BY TotalRevenue DESC) AS RevenueRank
    FROM CustomerOrderStats
)
SELECT *
FROM RankedStats
WHERE TotalRevenue > 100; -- Filter low revenue accounts
```

#### 🧵 What, Why, How
- **What**: Encapsulate logic (SPs), automate workflows (Triggers), reuse calculations (Functions), and produce strategic reports.
- **Why**: Real-world systems require atomic operations, audit logs, and clean business logic separation.
- **How**: Use transactions and error handling in SPs, triggers for auto-updates/logging, and modularize logic using UDFs and views.

---

### 📈 Stage 7: Advanced Reporting & Dashboards

#### 🔬 Skills Covered:
- Materialized Views
- Daily/Weekly/Monthly rollups
- Pivot tables and cross-tab reports
- View-based dashboard aggregation
- Data transformation for analytics tools

#### 💼 Objectives:
- Create ready-to-use reporting layers
- Support BI dashboards and export tools
- Track time-based business trends

#### 🔍 Example Reporting Queries (with comments):

```sql
-- Daily sales snapshot: revenue and total orders per day
SELECT 
    CAST(o.OrderDate AS DATE) AS OrderDate,                     -- Extract only the date part from datetime
    COUNT(DISTINCT o.OrderId) AS TotalOrders,                   -- Count unique orders placed that day
    SUM(oi.Quantity * oi.PriceAtPurchase) AS TotalRevenue       -- Sum of revenue for that day
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId                   -- Join to get order details
GROUP BY CAST(o.OrderDate AS DATE)                             -- Grouping daily
ORDER BY OrderDate DESC;                                       -- Most recent days first

-- Monthly revenue breakdown by product category
SELECT 
    FORMAT(o.OrderDate, 'yyyy-MM') AS Month,                   -- Format date into Year-Month for grouping
    c.Name AS Category,                                        -- Category name from Products
    SUM(oi.PriceAtPurchase * oi.Quantity) AS Revenue           -- Total revenue for that month and category
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
JOIN ProductVariants v ON oi.VariantId = v.VariantId
JOIN Products p ON v.ProductId = p.ProductId
JOIN Categories c ON p.CategoryId = c.CategoryId
GROUP BY FORMAT(o.OrderDate, 'yyyy-MM'), c.Name                -- Group by month and category
ORDER BY Month, Revenue DESC;                                  -- Show highest revenue categories first each month

-- Pivot Table: Count of order statuses over months
SELECT *
FROM (
    SELECT 
        FORMAT(OrderDate, 'yyyy-MM') AS Month,                  -- Convert to month format
        Status                                                  -- Order status (e.g. Delivered, Pending)
    FROM Orders
) AS SourceTable
PIVOT (
    COUNT(Status)                                               -- Count how many of each status
    FOR Status IN ([Pending], [Processing], [Shipped], [Delivered], [Cancelled])
) AS PivotTable
ORDER BY Month;

-- Abandoned cart report: customers with no orders
SELECT c.CustomerId, c.FullName, c.Email
FROM Customers c
LEFT JOIN Orders o ON c.CustomerId = o.CustomerId              -- LEFT JOIN to include customers even if no order
WHERE o.OrderId IS NULL;                                       -- Keep only those with no matching order

-- Regional revenue (assumes region column exists in Customers)
SELECT c.Region, SUM(oi.PriceAtPurchase * oi.Quantity) AS Revenue
FROM Customers c
JOIN Orders o ON c.CustomerId = o.CustomerId
JOIN OrderItems oi ON o.OrderId = oi.OrderId
GROUP BY c.Region
ORDER BY Revenue DESC;                                         -- Highest earning regions first
```

#### 📅 Materialized View Example (Indexed View)

```sql
-- Create a materialized view (indexed view) for daily product sales
-- Used to speed up frequent reporting by pre-aggregating data
CREATE VIEW vw_DailyProductSales
WITH SCHEMABINDING                       -- Required for indexed views
AS
SELECT 
    CAST(o.OrderDate AS DATE) AS SaleDate,     -- Daily grouping
    p.ProductId,                               -- Product identifier
    SUM(oi.Quantity * oi.PriceAtPurchase) AS Revenue, -- Daily revenue
    COUNT_BIG(*) AS RecordCount               -- Required for indexed views
FROM dbo.Orders o
JOIN dbo.OrderItems oi ON o.OrderId = oi.OrderId
JOIN dbo.ProductVariants v ON oi.VariantId = v.VariantId
JOIN dbo.Products p ON v.ProductId = p.ProductId
GROUP BY CAST(o.OrderDate AS DATE), p.ProductId; -- Must GROUP BY all columns not aggregated

-- Materialize the view by creating a clustered index
CREATE UNIQUE CLUSTERED INDEX IX_DailyProductSales
ON vw_DailyProductSales (SaleDate, ProductId); -- Makes the view physically stored and faster to query
```

#### 🎯 Bonus Example: Weekly Active Customers Report
```sql
-- Count distinct customers placing orders each week
SELECT 
    DATEPART(YEAR, OrderDate) AS Year,                -- Extract year
    DATEPART(WEEK, OrderDate) AS Week,                -- Extract week number
    COUNT(DISTINCT CustomerId) AS ActiveCustomers     -- Unique customers placing orders that week
FROM Orders
GROUP BY DATEPART(YEAR, OrderDate), DATEPART(WEEK, OrderDate)
ORDER BY Year DESC, Week DESC;
```

#### 🧵 What, Why, How
- **What**: We are transforming transactional data into trends, summaries, and performance metrics.
- **Why**: These insights help decision-makers evaluate KPIs like revenue, order volume, abandoned carts, etc.
- **How**:
  - Use `GROUP BY`, `FORMAT`, `PIVOT` to reshape data for trend analysis.
  - Materialized views with indexed storage improve dashboard performance.
  - LEFT JOIN + NULL check helps identify missing data (e.g., abandoned carts).

---

### 🚚 Stage 8: Deliveries & Payment Modules

#### 🔬 Skills Covered:
- Complex joins with shipments and payments
- Status tracking (Delivered, Delayed, Refunded)
- Nested queries for refund verification
- Transaction and refund reporting
- Delivery failure analysis

#### 💼 Objectives:
- Track order delivery from dispatch to delivery
- Monitor payment statuses and refund processes
- Analyze logistics and payment efficiency

#### 🔍 Schema Elements Introduced:
- `Shipments`: ShipmentId, OrderId, CarrierId, ShipmentDate, DeliveryDate, Status
- `Carriers`: CarrierId, Name, ContactInfo
- `Payments`: PaymentId, OrderId, Amount, PaymentMethod, Status, PaidOn
- `Refunds`: RefundId, PaymentId, Reason, Amount, RefundedOn

#### 🌐 Example Queries:

```sql
-- Join Orders with shipment status
SELECT 
    o.OrderId, o.OrderDate, s.ShipmentDate, s.DeliveryDate, s.Status AS ShipmentStatus
FROM Orders o
LEFT JOIN Shipments s ON o.OrderId = s.OrderId;

-- Orders delivered more than 10 days after shipment
SELECT 
    o.OrderId, s.ShipmentDate, s.DeliveryDate,
    DATEDIFF(DAY, s.ShipmentDate, s.DeliveryDate) AS DeliveryDelay
FROM Orders o
JOIN Shipments s ON o.OrderId = s.OrderId
WHERE DATEDIFF(DAY, s.ShipmentDate, s.DeliveryDate) > 10;

-- Total refunds issued per customer
SELECT 
    c.FullName,
    SUM(r.Amount) AS TotalRefunded
FROM Refunds r
JOIN Payments p ON r.PaymentId = p.PaymentId
JOIN Orders o ON p.OrderId = o.OrderId
JOIN Customers c ON o.CustomerId = c.CustomerId
GROUP BY c.FullName;

-- Refund ratio per carrier
SELECT 
    cr.Name AS Carrier,
    COUNT(DISTINCT r.RefundId) * 1.0 / COUNT(DISTINCT s.ShipmentId) AS RefundRate
FROM Shipments s
JOIN Carriers cr ON s.CarrierId = cr.CarrierId
JOIN Orders o ON s.OrderId = o.OrderId
JOIN Payments p ON o.OrderId = p.OrderId
LEFT JOIN Refunds r ON p.PaymentId = r.PaymentId
GROUP BY cr.Name;

-- Payment status summary
SELECT Status, COUNT(*) AS Count
FROM Payments
GROUP BY Status;
```

#### 🧵 What, Why, How
- **What**: These modules track physical and financial fulfillment.
- **Why**: Delivery delays and refund trends help optimize logistics and customer satisfaction.
- **How**: Use joins across orders, shipments, carriers, and payments; apply conditions and aggregations to build operational KPIs.

---

### 🛠️ Stage 9: SQL Optimization & Indexing (Deep Dive)

#### 🔬 Skills Covered:
- Index types and strategies
- Query tuning and execution plan analysis
- SARGability and operator behavior
- SQL anti-patterns (what *not* to do)
- Statistics, cardinality, and row estimation
- Parameter sniffing
- TempDB and memory grants
- System views and DMVs for diagnostics

#### 💼 Objectives:
- Understand SQL Server internals that affect performance
- Learn how to structure fast and scalable queries
- Avoid common traps that lead to slow performance

#### ⚖️ Optimization Fundamentals

**1. Indexing Types and Use Cases**
- **Clustered Index**: Only one per table. Defines the physical sort order of the table (typically the `Id` column).
- **Non-Clustered Index**: Multiple allowed. Pointers to actual table rows.
- **Covering Index**: Includes all columns used by the query — avoids extra lookups.
- **Filtered Index**: Indexes a subset of rows (e.g. `WHERE IsActive = 1`).
- **Included Columns**: Columns added to index to cover queries but not used for ordering.

**2. SARGability (Search ARGument Able)**
- SARGable WHERE clause allows SQL Server to use indexes.
- **Avoid:**
  - Functions on columns (`YEAR(OrderDate)`, `ISNULL(Column, 0)`)
  - Inequality on leading index columns
  - `OR` statements without re-indexing each filter

**3. Query Execution Plan Operators**
- **Clustered Index Seek** ✅ Best case
- **Index Seek** ✅ Efficient
- **Index Scan** ⚠️ Entire index scanned
- **Key Lookup** ⚠️ Data pulled from clustered index
- **RID Lookup** ⚠️ When table has no clustered index (heap)
- **Hash Match / Merge Join / Nested Loops** — depends on table size and join strategy

---

#### 🪖 Diagnostic Tools

```sql
-- Show missing indexes (potential recommendations)
SELECT TOP 10
    migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) AS IndexBenefit,
    mid.statement AS TableName,
    mid.equality_columns,
    mid.inequality_columns,
    mid.included_columns
FROM sys.dm_db_missing_index_group_stats migs
JOIN sys.dm_db_missing_index_groups mig ON migs.group_handle = mig.index_group_handle
JOIN sys.dm_db_missing_index_details mid ON mig.index_handle = mid.index_handle
ORDER BY IndexBenefit DESC;

-- Get execution plan for a query
SET SHOWPLAN_XML ON;
GO
-- <Place your query here>
GO
SET SHOWPLAN_XML OFF;
```

---

#### 🔍 Query Tuning Examples (Detailed with Comments)

```sql
-- ❌ BAD: Uses function on indexed column (non-SARGable)
SELECT *
FROM Orders
WHERE YEAR(OrderDate) = 2024;  -- Forces table scan

-- ✅ GOOD: Use date range (SARGable)
SELECT *
FROM Orders
WHERE OrderDate >= '2024-01-01' AND OrderDate < '2025-01-01';  -- Uses index on OrderDate
```

```sql
-- ❌ BAD: SELECT * + unnecessary joins
SELECT *
FROM Orders o
JOIN Customers c ON o.CustomerId = c.CustomerId
WHERE o.Status = 'Pending';

-- ✅ GOOD: Only select needed columns, reduce data
SELECT o.OrderId, o.OrderDate, c.FullName
FROM Orders o
JOIN Customers c ON o.CustomerId = c.CustomerId
WHERE o.Status = 'Pending';
```

```sql
-- Creating a covering index
-- Use when the same query runs repeatedly and involves SELECT + WHERE on specific fields
CREATE NONCLUSTERED INDEX IX_Order_Search
ON Orders (OrderDate)
INCLUDE (OrderId, CustomerId, Status);
```

---

#### 💡 Advanced Concepts

**1. Parameter Sniffing**
- SQL Server creates a plan based on first parameter value it sees.
- May not work well for other values.
- Solution: Use `OPTION (RECOMPILE)` or dynamic SQL if needed.

**2. Memory Grants & TempDB Spills**
- Query too big for memory? Spills to TempDB (slow).
- Monitor via `sys.dm_exec_query_stats` and `sys.dm_exec_query_memory_grants`

**3. Fill Factor**
- Controls page fullness in indexes.
- Use 90–95% for write-heavy workloads (to reduce page splits).

---

#### ✅ Optimization Summary

**DOs**:
- Use SARGable filters with ranges
- Choose selective indexed columns for WHERE, JOIN, ORDER BY
- Analyze execution plans regularly
- Use filtered/indexed views for repeated aggregations
- Regularly update stats & rebuild/reorganize indexes

**DON'Ts**:
- Avoid `SELECT *` unless truly necessary
- Avoid functions on filter/join columns
- Avoid unnecessary joins and large scans
- Don’t rely solely on guessed performance — validate with execution plans

---


### 📄 Stage 10: ETL & Warehousing (Master Class)

#### 🔬 Concepts Introduced:
- **ETL**: Extract, Transform, Load — move and prep data from OLTP systems for analytics
- **Staging**: Temporary raw data tables (landing zone)
- **Snapshots**: Time-based reports (daily sales, monthly inventory)
- **Fact Tables**: Core measurable business events (e.g. `FactSales`)
- **Dimension Tables**: Lookup attributes (e.g. `DimCustomer`, `DimDate`)
- **Incremental Loads**: Only pull new/changed records
- **ETL Orchestration**: Schedule and log ETL jobs

---

#### 💼 Objectives:
- Automate accurate reporting layers
- Separate transaction from analytical processing
- Design a scalable, auditable data warehouse

---

### 🧰 STAR SCHEMA OVERVIEW

- `FactSales` → Transaction data (grain: Product + Day)
- `DimCustomer`, `DimProduct`, `DimDate`, `DimRegion` → Lookup data

```sql
-- Sample DimDate Table
CREATE TABLE DimDate (
    DateId INT PRIMARY KEY,        -- Surrogate Key
    FullDate DATE,
    DayName VARCHAR(10),
    MonthName VARCHAR(10),
    Year INT,
    Quarter INT
);
```

```sql
-- Sample Fact Table
CREATE TABLE FactSales (
    DateId INT,
    ProductId INT,
    CustomerId INT,
    Quantity INT,
    Revenue DECIMAL(18, 2),
    FOREIGN KEY (DateId) REFERENCES DimDate(DateId),
    FOREIGN KEY (ProductId) REFERENCES Products(ProductId),
    FOREIGN KEY (CustomerId) REFERENCES Customers(CustomerId)
);
```

---

### 🔍 DAILY SALES SNAPSHOT EXAMPLE (WITH COMMENTS)

```sql
-- Capture daily revenue snapshot by product
INSERT INTO FactSales (DateId, ProductId, CustomerId, Quantity, Revenue)
SELECT 
    CONVERT(INT, FORMAT(o.OrderDate, 'yyyyMMdd')) AS DateId,  -- Convert date to surrogate key format
    p.ProductId,
    o.CustomerId,
    SUM(oi.Quantity),
    SUM(oi.Quantity * oi.PriceAtPurchase)
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
JOIN ProductVariants v ON oi.VariantId = v.VariantId
JOIN Products p ON v.ProductId = p.ProductId
WHERE o.OrderDate >= CAST(GETDATE() AS DATE)  -- Only today's data
GROUP BY FORMAT(o.OrderDate, 'yyyyMMdd'), p.ProductId, o.CustomerId;
```

---

### 🪧 ETL PIPELINE DESIGN (WITH STAGING & LOGGING)

```sql
-- 1. Create staging table (same structure as target)
CREATE TABLE Staging_Orders (
    OrderId INT,
    CustomerId INT,
    OrderDate DATETIME,
    Status NVARCHAR(50)
);

-- 2. Load raw data from flat file or source
-- Example: CSV Import with BULK INSERT
BULK INSERT Staging_Orders
FROM 'C:\Data\orders.csv'
WITH (
    FIELDTERMINATOR = ',',
    ROWTERMINATOR = '\n',
    FIRSTROW = 2
);

-- 3. Insert new records only into target
INSERT INTO Orders (OrderId, CustomerId, OrderDate, Status)
SELECT s.*
FROM Staging_Orders s
LEFT JOIN Orders o ON s.OrderId = o.OrderId
WHERE o.OrderId IS NULL;

-- 4. Clear staging
TRUNCATE TABLE Staging_Orders;
```

---

### ⏰ ETL LOGGING TABLES

```sql
-- Track ETL batch executions
CREATE TABLE ETL_BatchLog (
    BatchId INT IDENTITY PRIMARY KEY,
    JobName VARCHAR(100),
    StartTime DATETIME DEFAULT GETDATE(),
    EndTime DATETIME,
    Status VARCHAR(20),
    RowsProcessed INT,
    Notes VARCHAR(255)
);
```

```sql
-- Example usage (wrapped in stored procedure)
DECLARE @Start DATETIME = GETDATE();
-- ETL logic here...
INSERT INTO ETL_BatchLog (JobName, EndTime, Status, RowsProcessed, Notes)
VALUES ('DailySalesLoad', GETDATE(), 'Success', 1200, 'Snapshot loaded');
```

---

### 🧵 INCREMENTAL LOADS

```sql
-- Load only new/modified records (Change Tracking or audit column)
INSERT INTO FactSales (DateId, ProductId, CustomerId, Quantity, Revenue)
SELECT 
    DateId, ProductId, CustomerId, Quantity, Revenue
FROM Staging_FactSales
WHERE ModifiedAt > (SELECT MAX(ModifiedAt) FROM FactSales);
```

---

### 🎯 ETL BEST PRACTICES
- Use **TRY...CATCH** blocks and log exceptions
- Avoid locking production tables — use staging
- Always validate before final insert (e.g., CHECKSUM or row count)
- Test incremental vs full load timings

---


### 🔐 Stage 11: Security, Roles & Row-Level Access (Comprehensive Guide)

#### 🔬 Core Concepts

**Authentication**: Identifies who is accessing the system (e.g., SQL Login, Windows Auth).

**Authorization**: Defines what resources a user can access or modify (`GRANT`, `DENY`, `REVOKE`).

**Principals**: Security identities such as `LOGIN`, `USER`, `ROLE`.

**Securables**: Objects like tables, views, schemas, or procedures.

**Roles**: Groups of permissions to simplify access control (`db_datareader`, custom roles).

**Row-Level Security (RLS)**: Filter rows dynamically based on the logged-in user.

---

### 💼 Best Practices for SQL Server Security
- Never grant access directly to users — use roles.
- Use least privilege: only give the minimum required permission.
- Separate users by role: Readers, Editors, Admins, Auditors.
- Use views/stored procedures to hide sensitive logic or columns.
- Always audit who can read or write to sensitive data (email, address, salary).

---

### 🔍 Security Examples with Full Comments

```sql
-- 1. Create a SQL login (at server level)
CREATE LOGIN readonly_user WITH PASSWORD = 'StrongP@ssw0rd';

-- 2. Create a database user mapped to that login
USE EcommerceDB;
CREATE USER readonly_user FOR LOGIN readonly_user;

-- 3. Create a role to manage permissions
CREATE ROLE ReportingReaders;

-- 4. Grant read-only access to all tables in dbo schema
GRANT SELECT ON SCHEMA::dbo TO ReportingReaders;

-- 5. Add user to role
EXEC sp_addrolemember 'ReportingReaders', 'readonly_user';
```

```sql
-- 6. Revoke sensitive column or table access from user directly
DENY SELECT ON dbo.Customers(Email) TO readonly_user;
```

```sql
-- 7. Secure access using views only
REVOKE SELECT ON dbo.Orders TO readonly_user;
GRANT SELECT ON dbo.vw_OrdersPublic TO readonly_user;
```

---

### 🔓 Row-Level Security (RLS) Explained with Region Example

```sql
-- 1. Setup a mapping table for users and their region access
CREATE TABLE SecurityRegionMap (
    UserName SYSNAME PRIMARY KEY,   -- System user name
    Region NVARCHAR(100)            -- Allowed region
);

-- 2. Insert user-specific region access
INSERT INTO SecurityRegionMap VALUES
('maria_reader', 'South'),
('john_reader', 'North');
```

```sql
-- 3. Create inline table-valued function for filtering rows
CREATE FUNCTION dbo.fn_FilterCustomersByRegion(@Region NVARCHAR(100))
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN
    SELECT 1 AS Allowed
    FROM dbo.SecurityRegionMap
    WHERE Region = @Region AND UserName = USER_NAME();  -- USER_NAME() = current user
```

```sql
-- 4. Apply RLS policy using security function above
CREATE SECURITY POLICY CustomerRegionPolicy
ADD FILTER PREDICATE dbo.fn_FilterCustomersByRegion(Region)
ON dbo.Customers
WITH (STATE = ON);
```

-- ✅ Result: Users only see customers from their assigned region.

---

### 🧵 Auditing Permissions

```sql
-- View all permissions granted
SELECT pr.name AS Principal, pe.permission_name, pe.state_desc, obj.name AS Object
FROM sys.database_permissions pe
JOIN sys.database_principals pr ON pe.grantee_principal_id = pr.principal_id
LEFT JOIN sys.objects obj ON pe.major_id = obj.object_id
ORDER BY pr.name, obj.name;
```

```sql
-- Check user role memberships
EXEC sp_helprolemember 'ReportingReaders';
```

---

### 🕵️ Advanced Use Cases
- Apply **RLS by Department, Region, or StoreId**
- Combine RLS with **secure views** to protect both rows and columns
- Use stored procedures with **EXECUTE AS** to delegate secure access
- Implement **DDL triggers** to prevent unauthorized object changes

---

### 🏰 Stage 12: Logging, Auditing & Soft Deletes

#### 🔬 Concepts Introduced:
- **Soft Deletes**: Flag records instead of deleting (logical deletion)
- **Audit Trails**: Log who, what, and when of data changes
- **Change Tracking**: Lightweight row-change system
- **Temporal Tables**: Automatically keep data history
- **CDC (Change Data Capture)**: Advanced row-level change capture

---

#### 💼 Objectives:
- Track changes without losing historical data
- Provide full traceability of user actions
- Implement safe deletes with restore capability

---

### ❌ Soft Delete Implementation

```sql
-- 1. Add IsDeleted column to the table
ALTER TABLE Products ADD IsDeleted BIT DEFAULT 0;

-- 2. Replace DELETE with UPDATE
UPDATE Products
SET IsDeleted = 1
WHERE ProductId = 101;

-- 3. Filter out deleted products in all queries
SELECT *
FROM Products
WHERE IsDeleted = 0;

-- 4. Optionally, create a view for active records
CREATE VIEW vw_Products_Active AS
SELECT * FROM Products WHERE IsDeleted = 0;
```

---

### 🌐 Audit Log Table Example

```sql
-- 1. Create audit table
CREATE TABLE Audit_Orders (
    AuditId INT IDENTITY(1,1),
    OrderId INT,
    ActionType VARCHAR(10),
    PerformedBy NVARCHAR(100),
    PerformedOn DATETIME DEFAULT GETDATE(),
    OldStatus NVARCHAR(50),
    NewStatus NVARCHAR(50)
);

-- 2. Create trigger to track status changes
CREATE TRIGGER trg_AuditOrderStatus
ON Orders
AFTER UPDATE
AS
BEGIN
    INSERT INTO Audit_Orders (OrderId, ActionType, PerformedBy, OldStatus, NewStatus)
    SELECT d.OrderId, 'UPDATE', SYSTEM_USER, d.Status, i.Status
    FROM deleted d
    JOIN inserted i ON d.OrderId = i.OrderId
    WHERE d.Status <> i.Status;
END;
```

---

### 🔮 Temporal Tables (System-Versioned History)

```sql
-- 1. Add system versioning
CREATE TABLE Customers_History
(
    CustomerId INT PRIMARY KEY,
    FullName NVARCHAR(100),
    Email NVARCHAR(100),
    ValidFrom DATETIME2 GENERATED ALWAYS AS ROW START,
    ValidTo DATETIME2 GENERATED ALWAYS AS ROW END,
    PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
)
WITH (SYSTEM_VERSIONING = ON);
```

```sql
-- 2. Query past state (e.g., how it looked last month)
SELECT * FROM Customers_History
FOR SYSTEM_TIME AS OF '2024-02-15T00:00:00';
```

---

### 🤖 Change Tracking

```sql
-- 1. Enable change tracking at database level
ALTER DATABASE EcommerceDB
SET CHANGE_TRACKING = ON
(CHANGE_RETENTION = 7 DAYS, AUTO_CLEANUP = ON);

-- 2. Enable for specific tables
ALTER TABLE Orders
ENABLE CHANGE_TRACKING
WITH (TRACK_COLUMNS_UPDATED = ON);

-- 3. Query changed rows since last sync
SELECT *
FROM CHANGETABLE(CHANGES Orders, 101) AS CT; -- 101 = last sync version
```

---

### 🧵 Best Practices
- Use **views** to hide deleted rows
- Log changes using triggers or temporal tables
- Consider **Change Tracking** for sync, **CDC** for full row change details
- Always store `User`, `Action`, and `Timestamp` in audit logs
- Encrypt or mask PII in audit records when needed

---

### ⚖️ Summary
- **Soft Deletes**: Safer than physical delete; allows recovery
- **Auditing**: Proves compliance and accountability
- **System Versioning**: Built-in way to track full table history

---

### 🤖 Stage 13: Monitoring & Performance Insights (Comprehensive)

#### 🔬 Concepts Introduced:
- **Query Store**: Tracks execution plans and performance stats for T-SQL queries
- **Wait Stats**: Analyze resource waits like CPU, I/O, memory, locks
- **DMVs (Dynamic Management Views)**: Real-time internal metrics from SQL Server
- **Blocking & Deadlocks**: Identifying queries that are stuck or waiting
- **Performance Baselines**: Log and compare KPIs over time

---

### 💼 Why This Matters
Performance bottlenecks can cripple business operations. Mastering these tools allows you to:
- Detect slow queries before users report them
- Track down which processes are blocking others
- Build custom performance dashboards for developers and DBAs

---

### 🔍 Part 1: Enabling and Using Query Store

```sql
-- Enable Query Store (if not already)
ALTER DATABASE EcommerceDB SET QUERY_STORE = ON;
ALTER DATABASE EcommerceDB SET QUERY_STORE (OPERATION_MODE = READ_WRITE);
```

```sql
-- Top resource-heavy queries
SELECT
    qt.query_sql_text,
    rs.avg_duration AS AvgDuration,
    rs.avg_cpu_time AS AvgCPU,
    rs.avg_logical_io_reads AS AvgReads
FROM sys.query_store_query_text qt
JOIN sys.query_store_query q ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan p ON q.query_id = p.query_id
JOIN sys.query_store_runtime_stats rs ON p.plan_id = rs.plan_id
ORDER BY rs.avg_duration DESC;
```

---

### ⌛ Part 2: Wait Stats (Resource Wait Analysis)

```sql
-- Common wait types and time spent waiting (excluding idle waits)
SELECT TOP 10
    wait_type,
    wait_time_ms / 1000.0 AS SecondsWaited,
    signal_wait_time_ms / 1000.0 AS CPU_QueueTime,
    waiting_tasks_count AS WaitCount
FROM sys.dm_os_wait_stats
WHERE wait_type NOT LIKE '%SLEEP%'
ORDER BY wait_time_ms DESC;
```

- **What it tells you**: What your server is waiting on the most
- **Use it when**: Server is slow and you need to know why (e.g., locking, I/O, memory bottlenecks)

---

### 🚗 Part 3: Find Blocking Sessions

```sql
-- Shows who is blocking whom
SELECT
    r.session_id AS BlockedSession,
    r.blocking_session_id AS BlockingSession,
    r.wait_type,
    r.wait_time,
    r.status,
    t.text AS SQL
FROM sys.dm_exec_requests r
JOIN sys.dm_exec_sessions s ON r.session_id = s.session_id
CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) t
WHERE r.blocking_session_id <> 0;
```

```sql
-- Identify blocking chains for long-running waits
-- Can be visualized with tools like SQL Server Management Studio or manually
```

---

### 📊 Part 4: DMV Dashboards (Real-Time Monitoring)

```sql
-- Top CPU Consumers
SELECT TOP 5
    text AS QueryText,
    total_worker_time / execution_count AS AvgCPU,
    execution_count,
    total_elapsed_time / execution_count AS AvgDuration
FROM sys.dm_exec_query_stats
CROSS APPLY sys.dm_exec_sql_text(sql_handle)
ORDER BY AvgCPU DESC;
```

```sql
-- Top I/O Consumers
SELECT TOP 5
    text AS QueryText,
    total_logical_reads / execution_count AS AvgReads,
    execution_count
FROM sys.dm_exec_query_stats
CROSS APPLY sys.dm_exec_sql_text(sql_handle)
ORDER BY AvgReads DESC;
```

---

### 📅 Part 5: Scheduled Performance Snapshot Logger

```sql
-- Create snapshot table
CREATE TABLE dbo.PerformanceSnapshots (
    SnapshotTime DATETIME DEFAULT GETDATE(),
    QueryText NVARCHAR(MAX),
    AvgCPU BIGINT,
    AvgDuration BIGINT,
    ExecutionCount INT
);

-- Insert snapshot for high CPU queries (can schedule via SQL Agent)
INSERT INTO dbo.PerformanceSnapshots (QueryText, AvgCPU, AvgDuration, ExecutionCount)
SELECT TOP 10
    text,
    total_worker_time / execution_count,
    total_elapsed_time / execution_count,
    execution_count
FROM sys.dm_exec_query_stats
CROSS APPLY sys.dm_exec_sql_text(sql_handle)
ORDER BY total_worker_time DESC;
```

---

### 🥵 Part 6: Live Monitoring Checklist
- ✅ Enable Query Store
- ✅ Review `sys.dm_exec_query_stats` for hot spots
- ✅ Check `sys.dm_os_wait_stats` for external pressure (I/O, memory, locks)
- ✅ Detect blocking via `sys.dm_exec_requests`
- ✅ Log top queries to historical table daily
- ✅ Use SQL Server Agent to automate ETL from DMVs

---

### 📆 Stage 14: Advanced SQL Patterns & Final Project Integration

#### 🔬 Concepts Introduced:
- **Dynamic SQL**: Build and execute SQL dynamically
- **PIVOT / UNPIVOT**: Rotate rows to columns for reporting
- **JSON / XML Handling**: Store and extract structured data
- **TVPs (Table-Valued Parameters)**: Pass table data into procedures
- **Final Dashboard Queries**: Integrate reporting logic from all modules

---

#### 💼 Objectives:
- Master advanced SQL programming techniques
- Finalize reusable reporting modules and components
- Prepare query layers for integration with front-end tools (e.g., Power BI)

---

### 🔍 Dynamic SQL Example

```sql
-- Build query based on a condition
DECLARE @ColumnName NVARCHAR(50) = 'Region';
DECLARE @SQL NVARCHAR(MAX);

SET @SQL = 'SELECT ' + QUOTENAME(@ColumnName) + ', COUNT(*) AS CustomerCount FROM Customers GROUP BY ' + QUOTENAME(@ColumnName);
EXEC sp_executesql @SQL;
```

---

### 📊 PIVOT for Sales Reporting

```sql
-- Sales by product category across months
SELECT * FROM (
    SELECT FORMAT(OrderDate, 'yyyy-MM') AS OrderMonth, c.Name AS Category, oi.Quantity
    FROM OrderItems oi
    JOIN Orders o ON oi.OrderId = o.OrderId
    JOIN ProductVariants v ON oi.VariantId = v.VariantId
    JOIN Products p ON v.ProductId = p.ProductId
    JOIN Categories c ON p.CategoryId = c.CategoryId
) AS SourceTable
PIVOT (
    SUM(Quantity) FOR Category IN ([Shoes], [Bags], [Shirts])
) AS PivotTable;
```

---

### 🔎 JSON Support

```sql
-- Return customer data as JSON
SELECT CustomerId, FullName, Email
FROM Customers
FOR JSON AUTO;

-- Parse JSON input
DECLARE @json NVARCHAR(MAX) = '{"ProductId": 1, "Quantity": 5}';
SELECT *
FROM OPENJSON(@json)
WITH (ProductId INT, Quantity INT);
```

---

### 📰 XML Support

```sql
-- Export order data as XML
SELECT OrderId, OrderDate, Status
FROM Orders
FOR XML AUTO, ROOT('Orders');
```

---

### 🚪 Table-Valued Parameter Example

```sql
-- Step 1: Create a TVP type
CREATE TYPE OrderItemTVP AS TABLE (
    VariantId INT,
    Quantity INT,
    PriceAtPurchase DECIMAL(10,2)
);

-- Step 2: Use it in a stored procedure
CREATE PROCEDURE usp_InsertOrderItems
    @OrderId INT,
    @Items OrderItemTVP READONLY
AS
BEGIN
    INSERT INTO OrderItems (OrderId, VariantId, Quantity, PriceAtPurchase)
    SELECT @OrderId, VariantId, Quantity, PriceAtPurchase FROM @Items;
END;
```

---

### 🎯 Final Dashboard Query Integration Example

```sql
-- Daily KPIs: Total Sales, Orders, Revenue, Top Category
WITH DailyData AS (
    SELECT 
        CAST(OrderDate AS DATE) AS Day,
        COUNT(DISTINCT o.OrderId) AS Orders,
        SUM(oi.Quantity) AS ItemsSold,
        SUM(oi.Quantity * oi.PriceAtPurchase) AS Revenue,
        c.Name AS Category
    FROM Orders o
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    JOIN ProductVariants v ON oi.VariantId = v.VariantId
    JOIN Products p ON v.ProductId = p.ProductId
    JOIN Categories c ON p.CategoryId = c.CategoryId
    GROUP BY CAST(OrderDate AS DATE), c.Name
)
SELECT Day, SUM(Orders) AS TotalOrders, SUM(ItemsSold) AS TotalItems, SUM(Revenue) AS TotalRevenue
FROM DailyData
GROUP BY Day
ORDER BY Day DESC;
```

---

### 🧵 Best Practices
- Use **dynamic SQL** only when necessary and sanitize input
- Use **TVPs** for batch inserts instead of looping
- **Pivot/unpivot** only after filtering and cleaning data
- Prefer **JSON** or **XML** for app-to-database data contracts

---

### 📍 CONGRATULATIONS!
You've completed the full SQL Mastery Journey — from beginner data modeling to enterprise-grade dashboards, monitoring, ETL, auditing, and beyond.

You now have:
- A full-featured SQL database architecture
- Modular reporting patterns
- Admin, logging, and security layers
- Real-time and historical tracking tools



# 🧠 SQL Interview & Certification Preparation Pack – Full Code + What, Why, How

This guide contains **detailed SQL interview questions and answers**, categorized by topic, with full code examples and explanations to help you master both **SQL certification exams** and **real-world technical interviews**.

Each section below corresponds to stages of learning, starting from beginner to expert-level SQL.

---

## ✅ Stage 1: SELECT, JOIN, GROUP BY – Basic Foundations

### 1. Get total number of orders per customer
```sql
SELECT CustomerId, COUNT(*) AS TotalOrders
FROM Orders
GROUP BY CustomerId;
```
**What**: Count of rows per customer.
**Why**: Measures customer engagement.
**How**: `GROUP BY` aggregates data per unique CustomerId.

---

### 2. List customers with their total spending
```sql
SELECT o.CustomerId, SUM(oi.Quantity * oi.PriceAtPurchase) AS TotalSpent
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
GROUP BY o.CustomerId;
```
**What**: Total amount each customer has spent.
**Why**: Used in customer lifetime value analysis.
**How**: `JOIN` and aggregate function `SUM` calculate total.

---

### 3. Find products with no sales
```sql
SELECT p.ProductId, p.ProductName
FROM Products p
LEFT JOIN ProductVariants v ON p.ProductId = v.ProductId
LEFT JOIN OrderItems oi ON v.VariantId = oi.VariantId
WHERE oi.OrderItemId IS NULL;
```
**What**: Products never purchased.
**Why**: Useful for inventory clean-up or promotions.
**How**: `LEFT JOIN` followed by `NULL` filter.

---

### 4. Count total variants per product
```sql
SELECT ProductId, COUNT(*) AS VariantCount
FROM ProductVariants
GROUP BY ProductId;
```
**What**: Shows product diversity.
**Why**: Useful for category management.
**How**: `COUNT(*)` grouped by ProductId.

---

### 5. Get average order value
```sql
SELECT AVG(OrderTotal) AS AvgOrderValue
FROM (
    SELECT o.OrderId, SUM(oi.Quantity * oi.PriceAtPurchase) AS OrderTotal
    FROM Orders o
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    GROUP BY o.OrderId
) AS OrderTotals;
```
**What**: Average of all order totals.
**Why**: Key KPI in e-commerce analytics.
**How**: Nested query calculates each order total.

---

### 6. Retrieve customers who made more than 3 orders
```sql
SELECT CustomerId, COUNT(*) AS OrderCount
FROM Orders
GROUP BY CustomerId
HAVING COUNT(*) > 3;
```
**What**: High-engagement customers.
**Why**: Potential for loyalty programs.
**How**: `HAVING` filters after grouping.

---

### 7. List customers who have not ordered anything
```sql
SELECT c.CustomerId, c.FullName
FROM Customers c
LEFT JOIN Orders o ON c.CustomerId = o.CustomerId
WHERE o.OrderId IS NULL;
```
**What**: Detects inactive users.
**Why**: Useful for re-marketing campaigns.
**How**: `LEFT JOIN` + `NULL` filter.

---

### 8. List products ordered in January 2024
```sql
SELECT DISTINCT p.ProductId, p.ProductName
FROM Products p
JOIN ProductVariants v ON p.ProductId = v.ProductId
JOIN OrderItems oi ON v.VariantId = oi.VariantId
JOIN Orders o ON oi.OrderId = o.OrderId
WHERE o.OrderDate BETWEEN '2024-01-01' AND '2024-01-31';
```
**What**: Products purchased during a date range.
**Why**: Useful for seasonal trend analysis.
**How**: `JOINs` and date filtering.

---

### 9. Top 5 best-selling product variants
```sql
SELECT TOP 5 VariantId, SUM(Quantity) AS TotalSold
FROM OrderItems
GROUP BY VariantId
ORDER BY TotalSold DESC;
```
**What**: Most popular products.
**Why**: Stock forecasting.
**How**: Use `TOP` and `ORDER BY`.

---

### 10. Most recent orders per customer
```sql
SELECT CustomerId, MAX(OrderDate) AS LastOrderDate
FROM Orders
GROUP BY CustomerId;
```
**What**: Customer recency.
**Why**: Used in RFM segmentation.
**How**: `MAX` finds latest date per group.

## ✅ Stage 2: Window Functions – Ranking & Time-Series Analysis

### 1. Rank customers by total spending
```sql
SELECT 
    CustomerId,
    SUM(oi.Quantity * oi.PriceAtPurchase) AS TotalSpent,
    RANK() OVER (ORDER BY SUM(oi.Quantity * oi.PriceAtPurchase) DESC) AS SpendingRank
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
GROUP BY CustomerId;
```
**What**: Ranks customers based on spending.
**Why**: For loyalty or reward tiers.
**How**: `RANK()` function partitions results and orders by spend.

---

### 2. Get the latest order per customer
```sql
SELECT *
FROM (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY CustomerId ORDER BY OrderDate DESC) AS rn
    FROM Orders
) AS ranked
WHERE rn = 1;
```
**What**: Identifies the most recent order.
**Why**: Track customer activity.
**How**: `ROW_NUMBER()` resets per customer.

---

### 3. Compare each customer's order value to previous
```sql
SELECT 
    CustomerId,
    OrderId,
    OrderDate,
    SUM(oi.Quantity * oi.PriceAtPurchase) AS OrderValue,
    LAG(SUM(oi.Quantity * oi.PriceAtPurchase)) OVER (PARTITION BY CustomerId ORDER BY o.OrderDate) AS PrevOrderValue
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
GROUP BY CustomerId, OrderId, OrderDate;
```
**What**: Compares sequential purchases.
**Why**: Detect increasing/decreasing spend patterns.
**How**: `LAG()` brings prior row value into current row.

---

### 4. Cumulative revenue per customer
```sql
SELECT 
    CustomerId,
    OrderDate,
    SUM(oi.Quantity * oi.PriceAtPurchase) AS Revenue,
    SUM(SUM(oi.Quantity * oi.PriceAtPurchase)) OVER (PARTITION BY CustomerId ORDER BY OrderDate) AS RunningTotal
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
GROUP BY CustomerId, OrderDate;
```
**What**: Shows how much customer has spent over time.
**Why**: Visualize financial engagement.
**How**: Nested SUM with OVER clause.

---

### 5. Find the first order of every customer
```sql
SELECT *
FROM (
    SELECT *, RANK() OVER (PARTITION BY CustomerId ORDER BY OrderDate ASC) AS rnk
    FROM Orders
) AS RankedOrders
WHERE rnk = 1;
```
**What**: Extracts first purchase.
**Why**: Entry point for onboarding campaigns.
**How**: `RANK()` ordered by ascending order date.

---

### 6. Find customers whose last order was their highest-value order
```sql
WITH OrderStats AS (
    SELECT 
        CustomerId,
        OrderId,
        OrderDate,
        SUM(oi.Quantity * oi.PriceAtPurchase) AS OrderValue,
        ROW_NUMBER() OVER (PARTITION BY CustomerId ORDER BY OrderDate DESC) AS rn
    FROM Orders o
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    GROUP BY CustomerId, OrderId, OrderDate
)
SELECT *
FROM OrderStats
WHERE rn = 1 AND OrderValue = (
    SELECT MAX(SUM(oi2.Quantity * oi2.PriceAtPurchase))
    FROM Orders o2
    JOIN OrderItems oi2 ON o2.OrderId = oi2.OrderId
    WHERE o2.CustomerId = OrderStats.CustomerId
    GROUP BY o2.OrderId
);
```
**What**: Checks if last order is max order.
**Why**: Signals growth or loyalty.
**How**: Combines `ROW_NUMBER()` with correlated subquery.

---

### 7. Bucket customers into quartiles based on total spending
```sql
SELECT CustomerId, TotalSpent,
       NTILE(4) OVER (ORDER BY TotalSpent DESC) AS SpendingQuartile
FROM (
    SELECT CustomerId, SUM(oi.Quantity * oi.PriceAtPurchase) AS TotalSpent
    FROM Orders o
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    GROUP BY CustomerId
) AS Spending;
```
**What**: Segments customers into 4 buckets.
**Why**: Targeting by segment.
**How**: `NTILE(4)` divides rows into 4 evenly.

---

### 8. Add order count per customer next to each order
```sql
SELECT 
    OrderId,
    CustomerId,
    COUNT(*) OVER (PARTITION BY CustomerId) AS TotalOrdersForCustomer
FROM Orders;
```
**What**: Adds context to each order.
**Why**: Profile density of purchases.
**How**: `COUNT(*) OVER` scans partition.

---

### 9. Find most recent and second-most-recent orders
```sql
SELECT *
FROM (
    SELECT *, DENSE_RANK() OVER (PARTITION BY CustomerId ORDER BY OrderDate DESC) AS rnk
    FROM Orders
) AS Ranked
WHERE rnk <= 2;
```
**What**: History snapshot.
**Why**: Useful for retention analysis.
**How**: `DENSE_RANK()` allows ties.

---

### 10. Add difference between current and previous order date
```sql
SELECT 
    CustomerId,
    OrderDate,
    DATEDIFF(DAY, LAG(OrderDate) OVER (PARTITION BY CustomerId ORDER BY OrderDate), OrderDate) AS DaysSinceLast
FROM Orders;
```
**What**: Time gaps between customer orders.
**Why**: Churn prediction.
**How**: Use `LAG()` and `DATEDIFF()`.

---

### 11. Compare order values with average customer order value
```sql
SELECT 
    OrderId,
    CustomerId,
    SUM(oi.Quantity * oi.PriceAtPurchase) AS OrderValue,
    AVG(SUM(oi.Quantity * oi.PriceAtPurchase)) OVER (PARTITION BY o.CustomerId) AS AvgOrderValue
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
GROUP BY OrderId, CustomerId;
```
**What**: Compares a specific order value against that customer's average.
**Why**: Find outliers or unusual purchases.
**How**: `AVG(...) OVER` computes per partition.

---

### 12. Percentage of total spend per order
```sql
WITH CustomerSpending AS (
    SELECT o.OrderId, o.CustomerId,
           SUM(oi.Quantity * oi.PriceAtPurchase) AS OrderValue
    FROM Orders o
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    GROUP BY o.OrderId, o.CustomerId
)
SELECT *,
       100.0 * OrderValue / SUM(OrderValue) OVER (PARTITION BY CustomerId) AS PercentOfTotal
FROM CustomerSpending;
```
**What**: Contribution of each order to total customer spend.
**Why**: Identify large or small orders.
**How**: Ratio over window total.

---

### 13. Customer activity streaks: number of days since last order
```sql
SELECT 
    CustomerId,
    OrderDate,
    DATEDIFF(DAY, LAG(OrderDate) OVER (PARTITION BY CustomerId ORDER BY OrderDate), OrderDate) AS GapFromLast
FROM Orders;
```
**What**: Time between purchases.
**Why**: Customer churn signal.
**How**: `LAG()` gives previous row.

---

### 14. Show running rank of customers by total sales over time
```sql
WITH SalesByDate AS (
    SELECT CustomerId, OrderDate, 
           SUM(oi.Quantity * oi.PriceAtPurchase) AS DailySpend
    FROM Orders o
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    GROUP BY CustomerId, OrderDate
)
SELECT *,
       RANK() OVER (PARTITION BY OrderDate ORDER BY DailySpend DESC) AS RankThatDay
FROM SalesByDate;
```
**What**: Daily sales leaderboard.
**Why**: Gamification, promotions.
**How**: Partition by OrderDate.

---

### 15. Add revenue difference from previous order
```sql
SELECT 
    OrderId,
    CustomerId,
    OrderDate,
    Revenue,
    Revenue - LAG(Revenue) OVER (PARTITION BY CustomerId ORDER BY OrderDate) AS RevenueDelta
FROM (
    SELECT o.OrderId, o.CustomerId, o.OrderDate, 
           SUM(oi.Quantity * oi.PriceAtPurchase) AS Revenue
    FROM Orders o
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    GROUP BY o.OrderId, o.CustomerId, o.OrderDate
) AS RevenueTable;
```
**What**: Order-to-order revenue changes.
**Why**: Spot sudden spending surges or drops.
**How**: `LAG()` + math on outer query.

---

### 16. Detect first-time and repeat buyers
```sql
SELECT 
    OrderId,
    CustomerId,
    CASE WHEN ROW_NUMBER() OVER (PARTITION BY CustomerId ORDER BY OrderDate) = 1 THEN 'First Time' ELSE 'Repeat' END AS BuyerType
FROM Orders;
```
**What**: Labels customers.
**Why**: Tailor welcome vs. returner flows.
**How**: `ROW_NUMBER()` logic in CASE.

---

### 17. Running average revenue per customer
```sql
WITH DailyRevenue AS (
    SELECT o.CustomerId, o.OrderDate, SUM(oi.Quantity * oi.PriceAtPurchase) AS DailyTotal
    FROM Orders o
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    GROUP BY o.CustomerId, o.OrderDate
)
SELECT *,
       AVG(DailyTotal) OVER (PARTITION BY CustomerId ORDER BY OrderDate ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS RunningAvg
FROM DailyRevenue;
```
**What**: See how average spend evolves.
**Why**: Track customer trend over time.
**How**: Moving average window.

---

### 18. Show revenue for last 3 orders only
```sql
SELECT *
FROM (
    SELECT o.OrderId, o.CustomerId, o.OrderDate,
           SUM(oi.Quantity * oi.PriceAtPurchase) AS Revenue,
           ROW_NUMBER() OVER (PARTITION BY o.CustomerId ORDER BY o.OrderDate DESC) AS rn
    FROM Orders o
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    GROUP BY o.OrderId, o.CustomerId, o.OrderDate
) AS Recent
WHERE rn <= 3;
```
**What**: Limits to most recent 3.
**Why**: Trend snapshots.
**How**: `ROW_NUMBER()` usage.

---

### 19. Show revenue rank per category per day
```sql
WITH DailyCategorySales AS (
    SELECT FORMAT(OrderDate, 'yyyy-MM-dd') AS SalesDate,
           c.CategoryId,
           SUM(oi.Quantity * oi.PriceAtPurchase) AS Revenue
    FROM Orders o
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    JOIN ProductVariants v ON oi.VariantId = v.VariantId
    JOIN Products p ON v.ProductId = p.ProductId
    JOIN Categories c ON p.CategoryId = c.CategoryId
    GROUP BY FORMAT(OrderDate, 'yyyy-MM-dd'), c.CategoryId
)
SELECT *,
       RANK() OVER (PARTITION BY SalesDate ORDER BY Revenue DESC) AS DailyRank
FROM DailyCategorySales;
```
**What**: Leaderboard of top product categories.
**Why**: Measure category performance.
**How**: Partition by SalesDate.

---

### 20. Count days since first purchase for each order
```sql
SELECT OrderId, CustomerId, OrderDate,
       DATEDIFF(DAY, MIN(OrderDate) OVER (PARTITION BY CustomerId), OrderDate) AS DaysSinceFirstOrder
FROM Orders;
```
**What**: Time into customer lifecycle.
**Why**: Segmentation.
**How**: `MIN(...) OVER`.

---

### 21. Percentile rank of customers based on revenue
```sql
WITH CustomerSpend AS (
    SELECT CustomerId, SUM(oi.Quantity * oi.PriceAtPurchase) AS Revenue
    FROM Orders o
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    GROUP BY CustomerId
)
SELECT *,
       PERCENT_RANK() OVER (ORDER BY Revenue DESC) AS SpendPercentile
FROM CustomerSpend;
```
**What**: Rank normalized to 0–1 scale.
**Why**: For percentile-based targeting.
**How**: `PERCENT_RANK()`.

---

### 22. Rank orders within each region by revenue
```sql
WITH OrderRevenue AS (
    SELECT o.OrderId, c.Region,
           SUM(oi.Quantity * oi.PriceAtPurchase) AS Revenue
    FROM Orders o
    JOIN Customers c ON o.CustomerId = c.CustomerId
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    GROUP BY o.OrderId, c.Region
)
SELECT *,
       RANK() OVER (PARTITION BY Region ORDER BY Revenue DESC) AS RankInRegion
FROM OrderRevenue;
```
**What**: Localized top orders.
**Why**: Regional leaderboards.
**How**: `PARTITION BY Region`.

---

### 23. Cumulative order count per customer
```sql
SELECT CustomerId, OrderDate,
       COUNT(*) OVER (PARTITION BY CustomerId ORDER BY OrderDate ROWS UNBOUNDED PRECEDING) AS CumulativeOrders
FROM Orders;
```
**What**: Tracks total order history over time.
**Why**: Power churn dashboards.
**How**: `ROWS UNBOUNDED PRECEDING`.

---

### 24. Compute average gap between orders per customer
```sql
WITH OrderGaps AS (
    SELECT CustomerId, OrderDate,
           DATEDIFF(DAY, LAG(OrderDate) OVER (PARTITION BY CustomerId ORDER BY OrderDate), OrderDate) AS Gap
    FROM Orders
)
SELECT CustomerId, AVG(Gap * 1.0) AS AvgGap
FROM OrderGaps
WHERE Gap IS NOT NULL
GROUP BY CustomerId;
```
**What**: Time between orders.
**Why**: Churn or loyalty indicator.
**How**: LAG() + aggregate.

---

### 25. Highlight orders significantly above customer average
```sql
WITH OrderWithAvg AS (
    SELECT o.OrderId, o.CustomerId, o.OrderDate,
           SUM(oi.Quantity * oi.PriceAtPurchase) AS OrderValue,
           AVG(SUM(oi.Quantity * oi.PriceAtPurchase)) OVER (PARTITION BY o.CustomerId) AS AvgValue
    FROM Orders o
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    GROUP BY o.OrderId, o.CustomerId, o.OrderDate
)
SELECT *
FROM OrderWithAvg
WHERE OrderValue > 1.5 * AvgValue;
```
**What**: Identify large spikes.
**Why**: Upsell/cross-sell triggers.
**How**: Compare current row to window average.

---

# ✅ Stage 3: CTEs, Recursion & Pagination – Advanced Data Structuring

This stage explores Common Table Expressions (CTEs), recursive CTEs for hierarchies, and pagination techniques — essential for scalable applications and modular SQL.

Each question includes:
- ✅ The problem scenario
- 💡 Why it matters in real systems
- 🧠 Step-by-step SQL with explanations

---

### 1. Find top 5 most recent orders using a CTE
```sql
WITH RecentOrders AS (
  SELECT * FROM Orders ORDER BY OrderDate DESC
)
SELECT TOP 5 * FROM RecentOrders;
```
**What**: Filters recent orders.
**Why**: Often used for dashboard widgets.
**How**: CTE simplifies chaining filters.

---

### 2. List products that have never been ordered
```sql
WITH UnorderedProducts AS (
  SELECT p.ProductId, p.ProductName
  FROM Products p
  LEFT JOIN ProductVariants v ON p.ProductId = v.ProductId
  LEFT JOIN OrderItems oi ON v.VariantId = oi.VariantId
  WHERE oi.OrderItemId IS NULL
)
SELECT * FROM UnorderedProducts;
```
**What**: Inventory cleanup list.
**Why**: Useful for marketing unpurchased products.
**How**: LEFT JOIN + NULL filter inside CTE.

---

### 3. Build category hierarchy using recursion
```sql
WITH CategoryTree AS (
  SELECT CategoryId, ParentCategoryId, CategoryName, 0 AS Level
  FROM Categories
  WHERE ParentCategoryId IS NULL

  UNION ALL

  SELECT c.CategoryId, c.ParentCategoryId, c.CategoryName, Level + 1
  FROM Categories c
  JOIN CategoryTree ct ON c.ParentCategoryId = ct.CategoryId
)
SELECT * FROM CategoryTree ORDER BY Level;
```
**What**: Parent-child tree traversal.
**Why**: Visualize nested categories.
**How**: Recursive self-join.

---

### 4. Flatten order summary into a single CTE pipeline
```sql
WITH OrderSummary AS (
  SELECT o.OrderId, o.CustomerId, SUM(oi.Quantity * oi.PriceAtPurchase) AS TotalAmount
  FROM Orders o
  JOIN OrderItems oi ON o.OrderId = oi.OrderId
  GROUP BY o.OrderId, o.CustomerId
)
SELECT * FROM OrderSummary WHERE TotalAmount > 100;
```
**What**: Filter high-value orders.
**Why**: Used in loyalty and fraud detection.
**How**: Aggregate inside CTE.

---

### 5. Paginate orders with OFFSET-FETCH
```sql
SELECT OrderId, OrderDate
FROM Orders
ORDER BY OrderDate DESC
OFFSET 10 ROWS FETCH NEXT 10 ROWS ONLY;
```
**What**: Returns page 2 (rows 11–20).
**Why**: Required for paging UIs.
**How**: OFFSET/FETCH used with ORDER BY.

---

### 6. Retrieve full product breadcrumb path
```sql
WITH Breadcrumbs AS (
  SELECT CategoryId, CategoryName, ParentCategoryId,
         CAST(CategoryName AS VARCHAR(MAX)) AS Path
  FROM Categories
  WHERE ParentCategoryId IS NULL

  UNION ALL

  SELECT c.CategoryId, c.CategoryName, c.ParentCategoryId,
         CAST(b.Path + ' > ' + c.CategoryName AS VARCHAR(MAX))
  FROM Categories c
  JOIN Breadcrumbs b ON c.ParentCategoryId = b.CategoryId
)
SELECT * FROM Breadcrumbs;
```
**What**: Builds readable paths.
**Why**: Used in site navigation.
**How**: Recursive concat string path.

---

### 7. Paginate customers ordered by signup date
```sql
SELECT CustomerId, FullName, CreatedAt
FROM Customers
ORDER BY CreatedAt
OFFSET 0 ROWS FETCH NEXT 25 ROWS ONLY;
```
**What**: First 25 customers.
**Why**: Used in CRM and data export.
**How**: OFFSET-FETCH with sort.

---

### 8. Find all descendants of a specific category
```sql
WITH Descendants AS (
  SELECT CategoryId FROM Categories WHERE CategoryId = 5

  UNION ALL

  SELECT c.CategoryId
  FROM Categories c
  JOIN Descendants d ON c.ParentCategoryId = d.CategoryId
)
SELECT * FROM Descendants;
```
**What**: Drill-down subcategory finder.
**Why**: Organize or delete nested sets.
**How**: Recursive hierarchy.

---

### 9. Find most expensive product per category
```sql
WITH RankedProducts AS (
  SELECT p.ProductId, p.ProductName, c.CategoryId, p.Price,
         ROW_NUMBER() OVER (PARTITION BY c.CategoryId ORDER BY p.Price DESC) AS rn
  FROM Products p
  JOIN Categories c ON p.CategoryId = c.CategoryId
)
SELECT * FROM RankedProducts WHERE rn = 1;
```
**What**: Find top priced product.
**Why**: Use in merchandising strategy.
**How**: Use window + CTE.

---

### 10. Build a recursive calendar date range
```sql
WITH Calendar AS (
  SELECT CAST('2024-01-01' AS DATE) AS DateVal

  UNION ALL

  SELECT DATEADD(DAY, 1, DateVal)
  FROM Calendar
  WHERE DateVal < '2024-01-31'
)
SELECT * FROM Calendar;
```
**What**: Creates a date dimension.
**Why**: Use in time series reports.
**How**: Simple date recursion.

---

### 11. Generate monthly intervals between two dates
```sql
WITH MonthSeries AS (
  SELECT CAST('2023-01-01' AS DATE) AS MonthStart
  UNION ALL
  SELECT DATEADD(MONTH, 1, MonthStart)
  FROM MonthSeries
  WHERE MonthStart < '2023-12-01'
)
SELECT * FROM MonthSeries;
```
**What**: Generates months for reports.
**Why**: Use in financial monthly reports.
**How**: Recursive month increment.

---

### 12. Top 3 orders per customer using CTE + ranking
```sql
WITH RankedOrders AS (
  SELECT o.*, ROW_NUMBER() OVER (PARTITION BY CustomerId ORDER BY OrderDate DESC) AS rn
  FROM Orders o
)
SELECT * FROM RankedOrders WHERE rn <= 3;
```
**What**: Retrieve latest orders.
**Why**: For summary emails.
**How**: Use ROW_NUMBER in CTE.

---

### 13. Count total orders and value per customer
```sql
WITH CustomerOrders AS (
  SELECT CustomerId, COUNT(*) AS TotalOrders,
         SUM(oi.Quantity * oi.PriceAtPurchase) AS TotalSpent
  FROM Orders o
  JOIN OrderItems oi ON o.OrderId = oi.OrderId
  GROUP BY CustomerId
)
SELECT * FROM CustomerOrders;
```
**What**: Summarizes customer activity.
**Why**: For CRM dashboards.
**How**: Aggregate in CTE.

---

### 14. Generate product combinations for testing
```sql
WITH Base AS (
  SELECT ProductId FROM Products WHERE CategoryId = 1
)
SELECT a.ProductId AS Product1, b.ProductId AS Product2
FROM Base a CROSS JOIN Base b
WHERE a.ProductId < b.ProductId;
```
**What**: All pairings in category.
**Why**: Bundle product suggestions.
**How**: CROSS JOIN with filter.

---

### 15. Identify users with no recent orders
```sql
WITH LastOrders AS (
  SELECT CustomerId, MAX(OrderDate) AS LastOrderDate
  FROM Orders GROUP BY CustomerId
)
SELECT * FROM LastOrders WHERE LastOrderDate < DATEADD(MONTH, -6, GETDATE());
```
**What**: Identify dormant customers.
**Why**: Trigger reactivation campaign.
**How**: MAX(OrderDate) filter.

---

### 16. Flatten multilevel category tree to single line path
```sql
WITH Recurse AS (
  SELECT CategoryId, CategoryName, ParentCategoryId,
         CAST(CategoryName AS VARCHAR(MAX)) AS FullPath
  FROM Categories WHERE ParentCategoryId IS NULL

  UNION ALL

  SELECT c.CategoryId, c.CategoryName, c.ParentCategoryId,
         r.FullPath + ' > ' + c.CategoryName
  FROM Categories c
  JOIN Recurse r ON c.ParentCategoryId = r.CategoryId
)
SELECT * FROM Recurse;
```
**What**: Breadcrumb path.
**Why**: Frontend menus.
**How**: Recursive concat.

---

### 17. Show first product variant per product
```sql
WITH RankedVariants AS (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY ProductId ORDER BY CreatedAt) AS rn
  FROM ProductVariants
)
SELECT * FROM RankedVariants WHERE rn = 1;
```
**What**: Base variant.
**Why**: Product previews.
**How**: Partition by ProductId.

---

### 18. Calculate weekly sales aggregates
```sql
WITH WeeklySales AS (
  SELECT DATEPART(WEEK, OrderDate) AS WeekNum,
         SUM(oi.Quantity * oi.PriceAtPurchase) AS TotalRevenue
  FROM Orders o
  JOIN OrderItems oi ON o.OrderId = oi.OrderId
  GROUP BY DATEPART(WEEK, OrderDate)
)
SELECT * FROM WeeklySales;
```
**What**: Weekly revenue.
**Why**: Sales tracking.
**How**: Date grouping.

---

### 19. Rank categories by overall sales
```sql
WITH CategorySales AS (
  SELECT c.CategoryId, c.CategoryName,
         SUM(oi.Quantity * oi.PriceAtPurchase) AS Revenue
  FROM OrderItems oi
  JOIN ProductVariants v ON oi.VariantId = v.VariantId
  JOIN Products p ON v.ProductId = p.ProductId
  JOIN Categories c ON p.CategoryId = c.CategoryId
  GROUP BY c.CategoryId, c.CategoryName
)
SELECT *, RANK() OVER (ORDER BY Revenue DESC) AS RankInRevenue
FROM CategorySales;
```
**What**: Leaderboard by sales.
**Why**: Inventory planning.
**How**: CTE + RANK().

---

### 20. Compare customer order value to category average
```sql
WITH AvgPerCategory AS (
  SELECT c.CategoryId,
         AVG(oi.Quantity * oi.PriceAtPurchase) AS AvgValue
  FROM OrderItems oi
  JOIN ProductVariants v ON oi.VariantId = v.VariantId
  JOIN Products p ON v.ProductId = p.ProductId
  JOIN Categories c ON p.CategoryId = c.CategoryId
  GROUP BY c.CategoryId
)
SELECT * FROM AvgPerCategory;
```
**What**: Benchmarks by category.
**Why**: Product value alignment.
**How**: AVG() aggregation.

---

### 21. Return top 3 categories by average review score
```sql
WITH AvgRatings AS (
  SELECT c.CategoryName, AVG(r.Rating) AS AvgRating
  FROM ProductRatings r
  JOIN Products p ON r.ProductId = p.ProductId
  JOIN Categories c ON p.CategoryId = c.CategoryId
  GROUP BY c.CategoryName
)
SELECT TOP 3 * FROM AvgRatings ORDER BY AvgRating DESC;
```
**What**: Customer perception.
**Why**: Quality control.
**How**: AVG() + ranking.

---

### 22. Paginate product reviews in batches of 20
```sql
SELECT * FROM ProductRatings
ORDER BY CreatedAt DESC
OFFSET 0 ROWS FETCH NEXT 20 ROWS ONLY;
```
**What**: Fetch 1st page.
**Why**: Frontend pagination.
**How**: OFFSET/FETCH.

---

### 23. Generate day-of-week report from order dates
```sql
WITH DayStats AS (
  SELECT DATENAME(WEEKDAY, OrderDate) AS DayName,
         COUNT(*) AS Orders
  FROM Orders
  GROUP BY DATENAME(WEEKDAY, OrderDate)
)
SELECT * FROM DayStats;
```
**What**: Peak day insight.
**Why**: Optimize ads.
**How**: DATENAME + GROUP.

---

### 24. Paginate categories alphabetically, page 2 (rows 26–50)
```sql
SELECT * FROM Categories
ORDER BY CategoryName
OFFSET 25 ROWS FETCH NEXT 25 ROWS ONLY;
```
**What**: Page 2 display.
**Why**: UI/UX.
**How**: OFFSET.

---

### 25. Recursive depth tracker for category tree
```sql
WITH TreeDepth AS (
  SELECT CategoryId, ParentCategoryId, 0 AS Depth
  FROM Categories
  WHERE ParentCategoryId IS NULL

  UNION ALL

  SELECT c.CategoryId, c.ParentCategoryId, td.Depth + 1
  FROM Categories c
  JOIN TreeDepth td ON c.ParentCategoryId = td.CategoryId
)
SELECT * FROM TreeDepth ORDER BY Depth;
```
**What**: Nesting level.
**Why**: UI menu indentation.
**How**: Recursive depth.

---

# ✅ Stage 4: Stored Procedures, Triggers & Functions – SQL Logic and Automation

In this stage, we cover:
- ✅ Stored Procedures
- ✅ User-Defined Functions (Scalar & Table-Valued)
- ✅ Triggers (AFTER/INSTEAD OF)

Each of the 25 questions includes:
- Practical use case
- What, Why, and How explanation
- Full SQL with commentary

---

### 1. Create a stored procedure to insert a new customer
```sql
CREATE PROCEDURE AddCustomer
    @FullName NVARCHAR(100),
    @Email NVARCHAR(100)
AS
BEGIN
    INSERT INTO Customers (FullName, Email)
    VALUES (@FullName, @Email);
END;
```
**What**: Insert procedure.
**Why**: Encapsulates logic.
**How**: Parameters passed into insert.

---

### 2. Create a scalar function to calculate discount
```sql
CREATE FUNCTION fn_GetDiscount (@Total DECIMAL(10,2))
RETURNS DECIMAL(10,2)
AS
BEGIN
    RETURN CASE 
        WHEN @Total > 100 THEN @Total * 0.10
        ELSE 0
    END;
END;
```
**What**: Function returns a discount value.
**Why**: Used in invoices.
**How**: Conditional logic with RETURN.

---

### 3. Create AFTER INSERT trigger to log order creation
```sql
CREATE TRIGGER trg_LogOrderInsert
ON Orders
AFTER INSERT
AS
BEGIN
    INSERT INTO SystemLogs (ActionType, EntityName, EntityId, Timestamp)
    SELECT 'INSERT', 'Orders', OrderId, GETDATE()
    FROM inserted;
END;
```
**What**: Logs order creation.
**Why**: Tracks inserts for audits.
**How**: Uses inserted pseudo-table.

---

### 4. Function to calculate order total
```sql
CREATE FUNCTION fn_GetOrderTotal (@OrderId INT)
RETURNS DECIMAL(10,2)
AS
BEGIN
    DECLARE @Total DECIMAL(10,2);
    SELECT @Total = SUM(Quantity * PriceAtPurchase)
    FROM OrderItems WHERE OrderId = @OrderId;
    RETURN @Total;
END;
```
**What**: Order total calculator.
**Why**: Reusable for reports.
**How**: Aggregate with variable.

---

### 5. INSTEAD OF DELETE trigger for soft deletes
```sql
CREATE TRIGGER trg_SoftDeleteProduct
ON Products
INSTEAD OF DELETE
AS
BEGIN
    UPDATE Products
    SET IsDeleted = 1
    WHERE ProductId IN (SELECT ProductId FROM deleted);
END;
```
**What**: Prevents physical delete.
**Why**: Enables undo/delete recovery.
**How**: Converts DELETE into UPDATE.

---

### 6. Create a stored procedure to update order status
```sql
CREATE PROCEDURE UpdateOrderStatus
    @OrderId INT,
    @NewStatus NVARCHAR(50)
AS
BEGIN
    UPDATE Orders
    SET Status = @NewStatus, UpdatedAt = GETDATE()
    WHERE OrderId = @OrderId;
END;
```
**What**: Updates order status.
**Why**: Controlled updates.
**How**: Parameter-driven update.

---

### 7. Table-valued function for top N customers
```sql
CREATE FUNCTION fn_TopCustomers (@N INT)
RETURNS TABLE
AS
RETURN (
    SELECT TOP (@N) CustomerId, SUM(oi.Quantity * oi.PriceAtPurchase) AS TotalSpent
    FROM Orders o
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    GROUP BY CustomerId
    ORDER BY TotalSpent DESC
);
```
**What**: Returns top spenders.
**Why**: Leaderboards.
**How**: TVF using table RETURN.

---

### 8. Trigger to update inventory on order item insert
```sql
CREATE TRIGGER trg_UpdateInventory
ON OrderItems
AFTER INSERT
AS
BEGIN
    UPDATE Inventory
    SET QuantityAvailable = QuantityAvailable - i.Quantity
    FROM Inventory inv
    JOIN inserted i ON inv.VariantId = i.VariantId;
END;
```
**What**: Auto-adjusts inventory.
**Why**: Real-time tracking.
**How**: INSERT trigger updates Inventory.

---

### 9. Stored procedure for paginated product list
```sql
CREATE PROCEDURE GetProductsPaged
    @Page INT,
    @PageSize INT
AS
BEGIN
    DECLARE @Offset INT = (@Page - 1) * @PageSize;

    SELECT *
    FROM Products
    ORDER BY ProductName
    OFFSET @Offset ROWS FETCH NEXT @PageSize ROWS ONLY;
END;
```
**What**: Retrieves paginated product list.
**Why**: UI/API pagination.
**How**: OFFSET FETCH inside SP.

---

### 10. Scalar function to compute tax
```sql
CREATE FUNCTION fn_ComputeTax (@Amount DECIMAL(10,2))
RETURNS DECIMAL(10,2)
AS
BEGIN
    RETURN @Amount * 0.15;
END;
```
**What**: Calculates tax.
**Why**: Standard in e-commerce.
**How**: Simple multiplier function.

---

### 11. Stored procedure to apply bulk discount
```sql
CREATE PROCEDURE ApplyBulkDiscount
    @CategoryId INT, @DiscountRate DECIMAL(5,2)
AS
BEGIN
    UPDATE Products
    SET Price = Price * (1 - @DiscountRate)
    WHERE CategoryId = @CategoryId;
END;
```
**What**: Bulk price update.
**Why**: Promotions.
**How**: Category-based UPDATE.

---

### 12. Trigger to prevent deleting active customers
```sql
CREATE TRIGGER trg_PreventCustomerDelete
ON Customers
INSTEAD OF DELETE
AS
BEGIN
    IF EXISTS (
        SELECT 1 FROM Orders o
        JOIN deleted d ON o.CustomerId = d.CustomerId
    )
    BEGIN
        RAISERROR('Cannot delete customers with orders', 16, 1);
        ROLLBACK;
    END
    ELSE
    BEGIN
        DELETE FROM Customers WHERE CustomerId IN (SELECT CustomerId FROM deleted);
    END
END;
```
**What**: Safeguard for deletion.
**Why**: Preserve order integrity.
**How**: Conditional check in trigger.

---

### 13. TVF for products below inventory threshold
```sql
CREATE FUNCTION fn_LowStockProducts (@Threshold INT)
RETURNS TABLE
AS
RETURN (
    SELECT VariantId, QuantityAvailable
    FROM Inventory
    WHERE QuantityAvailable < @Threshold
);
```
**What**: Low stock list.
**Why**: Alerting systems.
**How**: Table-returning filter.

---

### 14. Scalar function for estimated delivery
```sql
CREATE FUNCTION fn_EstimatedDelivery (@Days INT)
RETURNS DATE
AS
BEGIN
    RETURN DATEADD(DAY, @Days, GETDATE());
END;
```
**What**: Adds days to today.
**Why**: Show expected dates.
**How**: DATEADD logic.

---

### 15. Trigger to audit price changes
```sql
CREATE TRIGGER trg_AuditPriceChange
ON Products
AFTER UPDATE
AS
BEGIN
    INSERT INTO PriceChangeLog (ProductId, OldPrice, NewPrice, ChangedAt)
    SELECT d.ProductId, d.Price, i.Price, GETDATE()
    FROM deleted d
    JOIN inserted i ON d.ProductId = i.ProductId
    WHERE d.Price <> i.Price;
END;
```
**What**: Tracks price updates.
**Why**: For audit/compliance.
**How**: Compare inserted/deleted.

---

### 16. Stored procedure to cancel an order
```sql
CREATE PROCEDURE CancelOrder
    @OrderId INT
AS
BEGIN
    UPDATE Orders
    SET Status = 'Cancelled'
    WHERE OrderId = @OrderId;

    DELETE FROM OrderItems WHERE OrderId = @OrderId;
END;
```
**What**: Full cancellation.
**Why**: Order management.
**How**: Two-step transaction.

---

### 17. Scalar function for full customer name
```sql
CREATE FUNCTION fn_FullName (@First NVARCHAR(50), @Last NVARCHAR(50))
RETURNS NVARCHAR(101)
AS
BEGIN
    RETURN @First + ' ' + @Last;
END;
```
**What**: Combines names.
**Why**: Clean display.
**How**: String concat.

---

### 18. Trigger to log failed logins
```sql
-- Hypothetical table for login events
CREATE TRIGGER trg_LogFailedLogin
ON FailedLoginEvents
AFTER INSERT
AS
BEGIN
    INSERT INTO SystemLogs (ActionType, EntityName, EntityId, Timestamp)
    SELECT 'LOGIN_FAIL', 'User', UserId, GETDATE()
    FROM inserted;
END;
```
**What**: Login failure tracking.
**Why**: Security.
**How**: Inserted audit.

---

### 19. Procedure to get total customers by region
```sql
CREATE PROCEDURE GetCustomerRegionTotals
AS
BEGIN
    SELECT Region, COUNT(*) AS TotalCustomers
    FROM Customers
    GROUP BY Region;
END;
```
**What**: Region breakdown.
**Why**: Analytics.
**How**: GROUP BY inside SP.

---

### 20. Function to get last order date
```sql
CREATE FUNCTION fn_LastOrderDate (@CustomerId INT)
RETURNS DATE
AS
BEGIN
    RETURN (SELECT MAX(OrderDate) FROM Orders WHERE CustomerId = @CustomerId);
END;
```
**What**: Last activity.
**Why**: Re-engagement logic.
**How**: MAX with filter.

---

### 21. Trigger to prevent negative inventory
```sql
CREATE TRIGGER trg_PreventNegativeInventory
ON OrderItems
AFTER INSERT
AS
BEGIN
    IF EXISTS (
        SELECT 1 FROM inserted i
        JOIN Inventory inv ON i.VariantId = inv.VariantId
        WHERE inv.QuantityAvailable - i.Quantity < 0
    )
    BEGIN
        RAISERROR('Inventory cannot go below zero', 16, 1);
        ROLLBACK;
    END
END;
```
**What**: Validation on insert.
**Why**: Prevent overselling.
**How**: Quantity check trigger.

---

### 22. Procedure to create daily sales snapshot
```sql
CREATE PROCEDURE SnapshotDailySales
AS
BEGIN
    INSERT INTO SalesSnapshot (SnapshotDate, TotalRevenue)
    SELECT CAST(GETDATE() AS DATE), SUM(oi.Quantity * oi.PriceAtPurchase)
    FROM Orders o
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    WHERE CAST(OrderDate AS DATE) = CAST(GETDATE() AS DATE);
END;
```
**What**: Daily snapshot.
**Why**: Historical tracking.
**How**: Aggregation + timestamp.

---

### 23. Scalar function to check if weekend
```sql
CREATE FUNCTION fn_IsWeekend (@Date DATE)
RETURNS BIT
AS
BEGIN
    RETURN CASE WHEN DATENAME(WEEKDAY, @Date) IN ('Saturday', 'Sunday') THEN 1 ELSE 0 END;
END;
```
**What**: Weekend check.
**Why**: Used in delivery SLA.
**How**: DATENAME logic.

---

### 24. Trigger to enforce product naming rules
```sql
CREATE TRIGGER trg_ValidateProductName
ON Products
INSTEAD OF INSERT
AS
BEGIN
    IF EXISTS (SELECT 1 FROM inserted WHERE LEN(ProductName) < 3)
    BEGIN
        RAISERROR('Product name too short', 16, 1);
        ROLLBACK;
    END
    ELSE
    BEGIN
        INSERT INTO Products SELECT * FROM inserted;
    END
END;
```
**What**: Naming enforcement.
**Why**: Clean data.
**How**: Conditional insert.

---

### 25. Procedure to archive old orders
```sql
CREATE PROCEDURE ArchiveOldOrders
    @CutoffDate DATE
AS
BEGIN
    INSERT INTO ArchivedOrders
    SELECT * FROM Orders WHERE OrderDate < @CutoffDate;

    DELETE FROM Orders WHERE OrderDate < @CutoffDate;
END;
```
**What**: Move old data.
**Why**: Performance cleanup.
**How**: Insert then delete.

---

# ✅ Stage 5: Views, Materialized Views & Modular Queries – Reusable & Performance-Driven SQL

In this stage, we focus on:
- ✅ Simple and complex **Views**
- ✅ Indexed/Materialized Views
- ✅ Reusable modular query patterns
- ✅ Encapsulation of logic for BI and performance

Each question includes:
- Use case
- What/Why/How explanation
- Code example

---

### 1. Create a simple view for active customers
```sql
CREATE VIEW vw_ActiveCustomers AS
SELECT CustomerId, FullName, Email
FROM Customers
WHERE IsActive = 1;
```
**What**: Filters active users.
**Why**: Avoid repeating WHERE clause.
**How**: Simple SELECT with filter.

---

### 2. Create a view for product inventory with threshold
```sql
CREATE VIEW vw_LowInventory AS
SELECT VariantId, QuantityAvailable
FROM Inventory
WHERE QuantityAvailable < 10;
```
**What**: Inventory alerts.
**Why**: Support restock workflows.
**How**: SELECT + filter.

---

### 3. View combining order with customer name
```sql
CREATE VIEW vw_OrderSummary AS
SELECT o.OrderId, o.OrderDate, c.FullName
FROM Orders o
JOIN Customers c ON o.CustomerId = c.CustomerId;
```
**What**: Order overview.
**Why**: Useful in BI tools.
**How**: JOIN inside view.

---

### 4. Parameter-like filtering using inline views
```sql
-- Use inline view inside FROM
SELECT *
FROM (
  SELECT * FROM Orders WHERE Status = 'Delivered'
) AS DeliveredOrders
WHERE OrderDate >= '2024-01-01';
```
**What**: Composable logic.
**Why**: Stackable queries.
**How**: Subquery alias.

---

### 5. Create a view for average order value per customer
```sql
CREATE VIEW vw_AvgOrderValue AS
SELECT o.CustomerId,
       AVG(oi.Quantity * oi.PriceAtPurchase) AS AvgValue
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
GROUP BY o.CustomerId;
```
**What**: Customer metric.
**Why**: Reporting.
**How**: Aggregation in view.

---

### 6. Materialized view (indexed) for fast sales totals
```sql
-- SQL Server: create indexed view for total revenue
CREATE VIEW vw_TotalSales
WITH SCHEMABINDING AS
SELECT CustomerId,
       COUNT_BIG(*) AS OrderCount,
       SUM(CONVERT(DECIMAL(10,2), oi.Quantity * oi.PriceAtPurchase)) AS Revenue
FROM dbo.Orders o
JOIN dbo.OrderItems oi ON o.OrderId = oi.OrderId
GROUP BY CustomerId;

CREATE UNIQUE CLUSTERED INDEX idx_TotalSales
ON vw_TotalSales (CustomerId);
```
**What**: Fast snapshot.
**Why**: Precomputed aggregation.
**How**: Indexed view.

---

### 7. View to extract pending shipment details
```sql
CREATE VIEW vw_PendingShipments AS
SELECT s.ShipmentId, o.OrderId, o.OrderDate, s.Status
FROM Shipments s
JOIN Orders o ON s.OrderId = o.OrderId
WHERE s.Status = 'Pending';
```
**What**: Ready-to-ship items.
**Why**: Logistics dashboard.
**How**: JOIN + WHERE filter.

---

### 8. View with computed column for profit
```sql
CREATE VIEW vw_ProductProfit AS
SELECT p.ProductId, p.ProductName, p.CostPrice, p.SalePrice,
       (p.SalePrice - p.CostPrice) AS ProfitMargin
FROM Products p;
```
**What**: Tracks profit margin.
**Why**: Margin analysis.
**How**: Computed column.

---

### 9. Combine view with window function for ranks
```sql
CREATE VIEW vw_CustomerSpendingRank AS
SELECT CustomerId, 
       SUM(oi.Quantity * oi.PriceAtPurchase) AS TotalSpent,
       RANK() OVER (ORDER BY SUM(oi.Quantity * oi.PriceAtPurchase) DESC) AS SpendRank
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
GROUP BY CustomerId;
```
**What**: Leaderboard.
**Why**: Loyalty tiers.
**How**: Window inside view.

---

### 10. View for average delivery time
```sql
CREATE VIEW vw_AvgDeliveryTime AS
SELECT CarrierId,
       AVG(DATEDIFF(DAY, ShippedDate, DeliveredDate)) AS AvgDays
FROM Shipments
WHERE DeliveredDate IS NOT NULL
GROUP BY CarrierId;
```
**What**: Delivery SLA tracker.
**Why**: Logistics QA.
**How**: DATEDIFF + AVG.

---

### 11. Use a view to simplify a complex join
```sql
CREATE VIEW vw_OrderDetailFull AS
SELECT o.OrderId, o.OrderDate, c.FullName, p.ProductName, oi.Quantity
FROM Orders o
JOIN Customers c ON o.CustomerId = c.CustomerId
JOIN OrderItems oi ON o.OrderId = oi.OrderId
JOIN ProductVariants v ON oi.VariantId = v.VariantId
JOIN Products p ON v.ProductId = p.ProductId;
```
**What**: Complex view.
**Why**: Reduces join code.
**How**: Multi-join logic.

---

### 12. View for monthly sales per category
```sql
CREATE VIEW vw_MonthlyCategorySales AS
SELECT FORMAT(o.OrderDate, 'yyyy-MM') AS Month, c.CategoryName,
       SUM(oi.Quantity * oi.PriceAtPurchase) AS Revenue
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
JOIN ProductVariants v ON oi.VariantId = v.VariantId
JOIN Products p ON v.ProductId = p.ProductId
JOIN Categories c ON p.CategoryId = c.CategoryId
GROUP BY FORMAT(o.OrderDate, 'yyyy-MM'), c.CategoryName;
```
**What**: Time-series sales.
**Why**: BI trends.
**How**: FORMAT + GROUP BY.

---

### 13. View for top 5 categories (ranking filter outside)
```sql
CREATE VIEW vw_CategoryRevenue AS
SELECT c.CategoryName,
       SUM(oi.Quantity * oi.PriceAtPurchase) AS Revenue
FROM OrderItems oi
JOIN ProductVariants v ON oi.VariantId = v.VariantId
JOIN Products p ON v.ProductId = p.ProductId
JOIN Categories c ON p.CategoryId = c.CategoryId
GROUP BY c.CategoryName;
```
**What**: Top categories.
**Why**: Inventory strategy.
**How**: Aggregate logic in view.

---

### 14. Reusable view for shipment delays
```sql
CREATE VIEW vw_DelayedShipments AS
SELECT ShipmentId, OrderId, DATEDIFF(DAY, ShippedDate, DeliveredDate) AS Delay
FROM Shipments
WHERE DeliveredDate > ExpectedDeliveryDate;
```
**What**: Track SLA failures.
**Why**: Performance audits.
**How**: DATEDIFF.

---

### 15. Indexed view for most sold products
```sql
CREATE VIEW vw_MostSoldProducts
WITH SCHEMABINDING AS
SELECT v.VariantId, COUNT_BIG(*) AS SoldCount
FROM dbo.OrderItems oi
JOIN dbo.ProductVariants v ON oi.VariantId = v.VariantId
GROUP BY v.VariantId;

CREATE UNIQUE CLUSTERED INDEX idx_SoldCount ON vw_MostSoldProducts (VariantId);
```
**What**: For fast lookup.
**Why**: Analytics performance.
**How**: Indexed view setup.

---

### 16. Create view with CASE statement for customer tier
```sql
CREATE VIEW vw_CustomerTier AS
SELECT CustomerId,
       SUM(oi.Quantity * oi.PriceAtPurchase) AS TotalSpend,
       CASE
           WHEN SUM(oi.Quantity * oi.PriceAtPurchase) >= 1000 THEN 'Gold'
           WHEN SUM(oi.Quantity * oi.PriceAtPurchase) >= 500 THEN 'Silver'
           ELSE 'Bronze'
       END AS Tier
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
GROUP BY CustomerId;
```
**What**: Customer segmentation.
**Why**: Loyalty program.
**How**: CASE logic in view.

---

### 17. Use view in report: category sales by day
```sql
SELECT * FROM vw_MonthlyCategorySales WHERE Month = '2024-01';
```
**What**: Reporting usage.
**Why**: Reusability.
**How**: Querying view.

---

### 18. View for payment summary per method
```sql
CREATE VIEW vw_PaymentSummary AS
SELECT PaymentMethod, COUNT(*) AS PaymentCount,
       SUM(Amount) AS TotalAmount
FROM Payments
GROUP BY PaymentMethod;
```
**What**: Finance view.
**Why**: Gateway analysis.
**How**: GROUP BY logic.

---

### 19. View with COALESCE for null-safe output
```sql
CREATE VIEW vw_CustomerContacts AS
SELECT CustomerId, FullName,
       COALESCE(PhoneNumber, 'N/A') AS Phone,
       COALESCE(Email, 'Unknown') AS Email
FROM Customers;
```
**What**: Null-safe display.
**Why**: Consistent output.
**How**: COALESCE for fallback.

---

### 20. Create dynamic view filtered by month (to be parameterized)
```sql
-- Not truly dynamic in SQL Server, use with a filtered wrapper
CREATE VIEW vw_JanOrders AS
SELECT * FROM Orders
WHERE MONTH(OrderDate) = 1;
```
**What**: Time slice.
**Why**: Snapshot for static views.
**How**: Static WHERE.

---

### 21. Combine sales and refunds in one view
```sql
CREATE VIEW vw_NetSales AS
SELECT o.OrderId, o.OrderDate, SUM(oi.Quantity * oi.PriceAtPurchase) AS GrossSales,
       ISNULL(r.RefundAmount, 0) AS Refunds,
       SUM(oi.Quantity * oi.PriceAtPurchase) - ISNULL(r.RefundAmount, 0) AS NetSales
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
LEFT JOIN Refunds r ON o.OrderId = r.OrderId
GROUP BY o.OrderId, o.OrderDate, r.RefundAmount;
```
**What**: Net sales view.
**Why**: Reconcile returns.
**How**: Join + subtraction.

---

### 22. View to support real-time dashboard
```sql
CREATE VIEW vw_LiveKPIs AS
SELECT COUNT(*) AS TotalOrders,
       SUM(oi.Quantity * oi.PriceAtPurchase) AS RevenueToday
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
WHERE CAST(OrderDate AS DATE) = CAST(GETDATE() AS DATE);
```
**What**: Daily stats.
**Why**: Live dashboard tiles.
**How**: CAST for today.

---

### 23. View with UNION for multi-table reporting
```sql
CREATE VIEW vw_UserActions AS
SELECT 'Order' AS ActionType, CustomerId, CreatedAt FROM Orders
UNION
SELECT 'Refund', CustomerId, CreatedAt FROM Refunds;
```
**What**: Unified activity stream.
**Why**: Behavioral logs.
**How**: UNION logic.

---

### 24. View for abandoned carts
```sql
CREATE VIEW vw_AbandonedCarts AS
SELECT c.CustomerId, c.CartCreatedAt, DATEDIFF(HOUR, c.CartCreatedAt, GETDATE()) AS HoursSinceLastActive
FROM Carts c
WHERE c.IsCheckedOut = 0;
```
**What**: Marketing re-targeting.
**Why**: Cart abandonment.
**How**: Timer logic.

---

### 25. Indexed view for fast lookup of available stock
```sql
CREATE VIEW vw_StockAvailability
WITH SCHEMABINDING AS
SELECT VariantId, QuantityAvailable
FROM dbo.Inventory
WHERE QuantityAvailable > 0;

CREATE UNIQUE CLUSTERED INDEX idx_StockAvailability
ON vw_StockAvailability (VariantId);
```
**What**: Optimize reads.
**Why**: Real-time checks.
**How**: Materialized + filtered.

---

# ✅ Stage 6: Advanced Reporting, PIVOT, and Dashboard Views – Analytical SQL

This stage focuses on:
- ✅ PIVOT / UNPIVOT
- ✅ Advanced reports and KPIs
- ✅ Temporal trends and snapshot reporting
- ✅ Visual-ready dashboard data

Each question includes:
- Clear use case
- What/Why/How format
- Full SQL code with best practices

---

### 1. PIVOT: Sales quantity by product per month
```sql
SELECT * FROM (
    SELECT FORMAT(OrderDate, 'yyyy-MM') AS OrderMonth,
           p.ProductName,
           oi.Quantity
    FROM OrderItems oi
    JOIN ProductVariants v ON oi.VariantId = v.VariantId
    JOIN Products p ON v.ProductId = p.ProductId
    JOIN Orders o ON oi.OrderId = o.OrderId
) AS SourceTable
PIVOT (
    SUM(Quantity) FOR ProductName IN ([Shoes], [Bags], [Shirts])
) AS PivotTable;
```
**What**: Show monthly sales in columns.
**Why**: User-friendly dashboard layout.
**How**: Use PIVOT to turn rows into columns.

---

### 2. UNPIVOT: Flatten inventory status
```sql
SELECT VariantId, StockType, Quantity
FROM (
  SELECT VariantId, QuantityAvailable, QuantityReserved
  FROM Inventory
) AS Inv
UNPIVOT (
  Quantity FOR StockType IN (QuantityAvailable, QuantityReserved)
) AS FlatStock;
```
**What**: Normalize wide stock table.
**Why**: Simpler aggregation.
**How**: Use UNPIVOT.

---

### 3. KPI: Total orders, revenue, average order value
```sql
SELECT
    COUNT(DISTINCT o.OrderId) AS TotalOrders,
    SUM(oi.Quantity * oi.PriceAtPurchase) AS TotalRevenue,
    AVG(oi.Quantity * oi.PriceAtPurchase) AS AvgOrderValue
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId;
```
**What**: Executive summary.
**Why**: Core dashboard metrics.
**How**: Aggregates.

---

### 4. KPI: Daily revenue trend (last 7 days)
```sql
SELECT CAST(OrderDate AS DATE) AS Day,
       SUM(oi.Quantity * oi.PriceAtPurchase) AS Revenue
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
WHERE OrderDate >= DATEADD(DAY, -7, GETDATE())
GROUP BY CAST(OrderDate AS DATE)
ORDER BY Day;
```
**What**: 7-day trend line.
**Why**: Performance monitoring.
**How**: GROUP BY day.

---

### 5. Top-selling products per month
```sql
WITH MonthlyProductSales AS (
    SELECT FORMAT(o.OrderDate, 'yyyy-MM') AS Month,
           p.ProductName,
           SUM(oi.Quantity) AS TotalSold
    FROM OrderItems oi
    JOIN ProductVariants v ON oi.VariantId = v.VariantId
    JOIN Products p ON v.ProductId = p.ProductId
    JOIN Orders o ON oi.OrderId = o.OrderId
    GROUP BY FORMAT(o.OrderDate, 'yyyy-MM'), p.ProductName
)
SELECT * FROM (
    SELECT *, RANK() OVER (PARTITION BY Month ORDER BY TotalSold DESC) AS rn
    FROM MonthlyProductSales
) Ranked
WHERE rn <= 1;
```
**What**: Bestsellers by month.
**Why**: Trend visibility.
**How**: RANK() + filter.

---

### 6. Hourly revenue summary for the last 24 hours
```sql
SELECT DATEPART(HOUR, OrderDate) AS OrderHour,
       SUM(oi.Quantity * oi.PriceAtPurchase) AS Revenue
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
WHERE OrderDate >= DATEADD(HOUR, -24, GETDATE())
GROUP BY DATEPART(HOUR, OrderDate)
ORDER BY OrderHour;
```
**What**: Hourly view.
**Why**: Detect sales peaks.
**How**: GROUP BY hour.

---

### 7. Weekly refund trend
```sql
SELECT DATEPART(WEEK, RefundDate) AS RefundWeek,
       SUM(RefundAmount) AS TotalRefunded
FROM Refunds
GROUP BY DATEPART(WEEK, RefundDate)
ORDER BY RefundWeek;
```
**What**: Refund patterns.
**Why**: Quality control.
**How**: GROUP BY week.

---

### 8. Order counts by weekday
```sql
SELECT DATENAME(WEEKDAY, OrderDate) AS DayName,
       COUNT(*) AS Orders
FROM Orders
GROUP BY DATENAME(WEEKDAY, OrderDate);
```
**What**: Popular days.
**Why**: Staffing/ads.
**How**: Day grouping.

---

### 9. Pivot by payment method monthly
```sql
SELECT * FROM (
    SELECT FORMAT(PaymentDate, 'yyyy-MM') AS PayMonth, PaymentMethod, Amount
    FROM Payments
) AS SourceTable
PIVOT (
    SUM(Amount) FOR PaymentMethod IN ([CreditCard], [PayPal], [Stripe])
) AS PivotPay;
```
**What**: Payment spread.
**Why**: Channel performance.
**How**: PIVOT revenue.

---

### 10. Orders by region (geographic breakdown)
```sql
SELECT Region, COUNT(*) AS Orders
FROM Customers c
JOIN Orders o ON c.CustomerId = o.CustomerId
GROUP BY Region;
```
**What**: Regional performance.
**Why**: Targeting.
**How**: JOIN + GROUP.

---

### 11. Revenue vs Refunds by month
```sql
WITH MonthlyData AS (
  SELECT FORMAT(OrderDate, 'yyyy-MM') AS Month,
         SUM(oi.Quantity * oi.PriceAtPurchase) AS Sales
  FROM Orders o
  JOIN OrderItems oi ON o.OrderId = oi.OrderId
  GROUP BY FORMAT(OrderDate, 'yyyy-MM')
), RefundsData AS (
  SELECT FORMAT(RefundDate, 'yyyy-MM') AS Month,
         SUM(RefundAmount) AS Refunds
  FROM Refunds
  GROUP BY FORMAT(RefundDate, 'yyyy-MM')
)
SELECT m.Month, Sales, ISNULL(r.Refunds, 0) AS Refunds
FROM MonthlyData m
LEFT JOIN RefundsData r ON m.Month = r.Month;
```
**What**: Profit margin view.
**Why**: Net KPI.
**How**: CTEs + JOIN.

---

### 12. Weekly growth rate
```sql
WITH WeeklyRevenue AS (
    SELECT DATEPART(WEEK, OrderDate) AS WeekNum,
           SUM(oi.Quantity * oi.PriceAtPurchase) AS Revenue
    FROM Orders o
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    GROUP BY DATEPART(WEEK, OrderDate)
)
SELECT WeekNum,
       Revenue,
       LAG(Revenue) OVER (ORDER BY WeekNum) AS LastWeek,
       (Revenue - LAG(Revenue) OVER (ORDER BY WeekNum)) * 100.0 / NULLIF(LAG(Revenue) OVER (ORDER BY WeekNum), 0) AS GrowthRate
FROM WeeklyRevenue;
```
**What**: Performance growth.
**Why**: Health check.
**How**: LAG() comparison.

---

### 13. Refunds by reason code (pivot)
```sql
SELECT * FROM (
  SELECT RefundReason, FORMAT(RefundDate, 'yyyy-MM') AS Month, RefundAmount
  FROM Refunds
) AS Source
PIVOT (
  SUM(RefundAmount) FOR RefundReason IN ([Damaged], [WrongSize], [Late])
) AS PivotReasons;
```
**What**: Root cause analysis.
**Why**: Ops optimization.
**How**: PIVOT on reasons.

---

### 14. Daily new customers
```sql
SELECT CAST(CreatedAt AS DATE) AS JoinDate, COUNT(*) AS NewCustomers
FROM Customers
GROUP BY CAST(CreatedAt AS DATE);
```
**What**: Growth tracking.
**Why**: Marketing funnel.
**How**: Signup date group.

---

### 15. Customer churn: last active month
```sql
SELECT CustomerId, MAX(FORMAT(OrderDate, 'yyyy-MM')) AS LastMonthActive
FROM Orders
GROUP BY CustomerId;
```
**What**: Churn detection.
**Why**: Retention focus.
**How**: MAX date group.

---

### 16. Orders by time of day buckets
```sql
SELECT
  CASE
    WHEN DATEPART(HOUR, OrderDate) BETWEEN 0 AND 5 THEN 'Night'
    WHEN DATEPART(HOUR, OrderDate) BETWEEN 6 AND 11 THEN 'Morning'
    WHEN DATEPART(HOUR, OrderDate) BETWEEN 12 AND 17 THEN 'Afternoon'
    ELSE 'Evening'
  END AS TimeBlock,
  COUNT(*) AS Orders
FROM Orders
GROUP BY
  CASE
    WHEN DATEPART(HOUR, OrderDate) BETWEEN 0 AND 5 THEN 'Night'
    WHEN DATEPART(HOUR, OrderDate) BETWEEN 6 AND 11 THEN 'Morning'
    WHEN DATEPART(HOUR, OrderDate) BETWEEN 12 AND 17 THEN 'Afternoon'
    ELSE 'Evening'
  END;
```
**What**: Time segmentation.
**Why**: Campaign timing.
**How**: CASE blocks.

---

### 17. Conversion rate: visits vs orders
```sql
SELECT VisitDate, COUNT(DISTINCT VisitorId) AS Visitors,
       (SELECT COUNT(*) FROM Orders WHERE CAST(OrderDate AS DATE) = VisitDate) AS Orders,
       100.0 * (SELECT COUNT(*) FROM Orders WHERE CAST(OrderDate AS DATE) = VisitDate) / COUNT(DISTINCT VisitorId) AS ConversionRate
FROM SiteVisits
GROUP BY VisitDate;
```
**What**: Marketing ROI.
**Why**: Optimize funnel.
**How**: Dual source compare.

---

### 18. Products added vs sold by category
```sql
WITH Added AS (
  SELECT CategoryId, COUNT(*) AS AddedCount FROM Products GROUP BY CategoryId
), Sold AS (
  SELECT p.CategoryId, COUNT(*) AS SoldCount
  FROM OrderItems oi
  JOIN ProductVariants v ON oi.VariantId = v.VariantId
  JOIN Products p ON v.ProductId = p.ProductId
  GROUP BY p.CategoryId
)
SELECT a.CategoryId, a.AddedCount, s.SoldCount
FROM Added a
LEFT JOIN Sold s ON a.CategoryId = s.CategoryId;
```
**What**: Inventory flow.
**Why**: Lifecycle analysis.
**How**: Dual CTE compare.

---

### 19. View peak revenue day of the month
```sql
SELECT TOP 1 CAST(OrderDate AS DATE) AS PeakDay, SUM(oi.Quantity * oi.PriceAtPurchase) AS DailyRevenue
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
GROUP BY CAST(OrderDate AS DATE)
ORDER BY DailyRevenue DESC;
```
**What**: Identify spikes.
**Why**: Event success.
**How**: ORDER BY sum.

---

### 20. Order heatmap: day vs hour
```sql
SELECT DATENAME(WEEKDAY, OrderDate) AS DayOfWeek,
       DATEPART(HOUR, OrderDate) AS Hour,
       COUNT(*) AS Orders
FROM Orders
GROUP BY DATENAME(WEEKDAY, OrderDate), DATEPART(HOUR, OrderDate);
```
**What**: Temporal density.
**Why**: Staffing/timing.
**How**: 2D group.

---

### 21. Product review trend
```sql
SELECT FORMAT(ReviewDate, 'yyyy-MM') AS Month,
       AVG(Rating) AS AvgRating, COUNT(*) AS TotalReviews
FROM ProductRatings
GROUP BY FORMAT(ReviewDate, 'yyyy-MM');
```
**What**: Quality trend.
**Why**: Product feedback.
**How**: Time avg.

---

### 22. Cart abandonment rate by day
```sql
SELECT CAST(CartCreatedAt AS DATE) AS Day,
       COUNT(*) AS Carts,
       SUM(CASE WHEN IsCheckedOut = 0 THEN 1 ELSE 0 END) AS Abandoned,
       100.0 * SUM(CASE WHEN IsCheckedOut = 0 THEN 1 ELSE 0 END) / COUNT(*) AS AbandonRate
FROM Carts
GROUP BY CAST(CartCreatedAt AS DATE);
```
**What**: Conversion loss.
**Why**: Optimize checkout.
**How**: Ratio logic.

---

### 23. Monthly active users (orders)
```sql
SELECT FORMAT(OrderDate, 'yyyy-MM') AS Month, COUNT(DISTINCT CustomerId) AS ActiveUsers
FROM Orders
GROUP BY FORMAT(OrderDate, 'yyyy-MM');
```
**What**: Activity measurement.
**Why**: Growth metric.
**How**: Distinct count.

---

### 24. Forecast next month sales (moving avg)
```sql
WITH MonthlyRevenue AS (
    SELECT FORMAT(OrderDate, 'yyyy-MM') AS Month,
           SUM(oi.Quantity * oi.PriceAtPurchase) AS Revenue
    FROM Orders o
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    GROUP BY FORMAT(OrderDate, 'yyyy-MM')
)
SELECT *,
       AVG(Revenue) OVER (ORDER BY Month ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS ForecastRevenue
FROM MonthlyRevenue;
```
**What**: Simple forecast.
**Why**: Planning.
**How**: Moving window.

---

### 25. Revenue by campaign tag (UNPIVOTed from wide table)
```sql
SELECT CampaignType, Revenue
FROM (
  SELECT EmailRevenue, SocialRevenue, AdsRevenue
) AS CampaignData
UNPIVOT (
  Revenue FOR CampaignType IN (EmailRevenue, SocialRevenue, AdsRevenue)
) AS FlatCampaign;
```
**What**: Campaign ROI.
**Why**: Channel budget.
**How**: UNPIVOT to normalize.

---

# ✅ Stage 7: JSON, XML, Dynamic SQL & Advanced Aggregation – Extending SQL Power

This stage explores:
- ✅ JSON & XML processing
- ✅ Dynamic SQL for flexible queries
- ✅ Advanced GROUPING SETS, ROLLUP, CUBE
- ✅ Flexible schema manipulation

Each question includes:
- Real-world use case
- What/Why/How format
- Code with explanation

---

### 1–10: [Already included above]

---

### 11. Generate comma-separated customer names for each region
```sql
SELECT Region, STRING_AGG(FullName, ', ') AS Customers
FROM Customers
GROUP BY Region;
```
**What**: Region-based customer list.
**Why**: Region-focused reports.
**How**: Use STRING_AGG with GROUP BY.

---

### 12. Dynamic SQL to filter by product category
```sql
DECLARE @CategoryId INT = 2;
DECLARE @sql NVARCHAR(MAX) =
    'SELECT ProductId, ProductName FROM Products WHERE CategoryId = ' + CAST(@CategoryId AS NVARCHAR);
EXEC sp_executesql @sql;
```
**What**: Flexible filtering.
**Why**: Parameter-driven reports.
**How**: Concatenate input into SQL.

---

### 13. Convert query output to JSON array with nesting
```sql
SELECT CustomerId,
       (SELECT OrderId, OrderDate FROM Orders o WHERE o.CustomerId = c.CustomerId FOR JSON PATH) AS Orders
FROM Customers c;
```
**What**: Nested objects.
**Why**: Structured frontend data.
**How**: JSON PATH subquery.

---

### 14. Parse JSON array into rows
```sql
DECLARE @json NVARCHAR(MAX) = '[{"Id": 1}, {"Id": 2}]';
SELECT value AS RowData FROM OPENJSON(@json);
```
**What**: Expand array.
**Why**: Loop-like row usage.
**How**: OPENJSON root array.

---

### 15. Dynamic pivot for sales by month
```sql
DECLARE @columns NVARCHAR(MAX), @sql NVARCHAR(MAX);
SELECT @columns = STRING_AGG(QUOTENAME(Month), ',')
FROM (SELECT DISTINCT FORMAT(OrderDate, 'yyyy-MM') AS Month FROM Orders) AS Months;

SET @sql = '
SELECT ProductName, ' + @columns + '
FROM (
  SELECT p.ProductName, FORMAT(o.OrderDate, ''yyyy-MM'') AS Month, oi.Quantity
  FROM Orders o
  JOIN OrderItems oi ON o.OrderId = oi.OrderId
  JOIN ProductVariants v ON oi.VariantId = v.VariantId
  JOIN Products p ON v.ProductId = p.ProductId
) AS SourceTable
PIVOT (
  SUM(Quantity) FOR Month IN (' + @columns + ')
) AS PivotTable';

EXEC sp_executesql @sql;
```
**What**: Monthly pivot.
**Why**: Dynamic date columns.
**How**: Dynamic PIVOT.

---

### 16. ROLLUP with COALESCE for report labels
```sql
SELECT COALESCE(c.CategoryName, 'TOTAL') AS Category,
       SUM(oi.Quantity * oi.PriceAtPurchase) AS Revenue
FROM OrderItems oi
JOIN ProductVariants v ON oi.VariantId = v.VariantId
JOIN Products p ON v.ProductId = p.ProductId
JOIN Categories c ON p.CategoryId = c.CategoryId
GROUP BY ROLLUP (c.CategoryName);
```
**What**: Label totals.
**Why**: Friendly output.
**How**: COALESCE null rows.

---

### 17. Return dynamic XML list of orders
```sql
SELECT CustomerId,
       (SELECT OrderId, OrderDate FROM Orders o WHERE o.CustomerId = c.CustomerId FOR XML PATH('Order'), ROOT('Orders'), TYPE)
FROM Customers c;
```
**What**: Hierarchical XML.
**Why**: XML-based services.
**How**: XML PATH with nesting.

---

### 18. Dynamic filter with multiple parameters
```sql
DECLARE @sql NVARCHAR(MAX);
DECLARE @status NVARCHAR(50) = 'Shipped';
DECLARE @region NVARCHAR(50) = 'West';

SET @sql = 'SELECT * FROM Orders o JOIN Customers c ON o.CustomerId = c.CustomerId WHERE 1=1';
IF @status IS NOT NULL SET @sql += ' AND o.Status = ''' + @status + '''';
IF @region IS NOT NULL SET @sql += ' AND c.Region = ''' + @region + '''';
EXEC sp_executesql @sql;
```
**What**: Multi-filter logic.
**Why**: User-driven search.
**How**: If-then dynamic SQL.

---

### 19. Combine JSON aggregation with GROUP BY
```sql
SELECT CustomerId,
       (SELECT OrderId, OrderDate
        FROM Orders o
        WHERE o.CustomerId = c.CustomerId
        FOR JSON PATH) AS OrdersJson
FROM Customers c;
```
**What**: Pre-grouped JSON output.
**Why**: API feed.
**How**: JSON PATH per group.

---

### 20. Generate XML for order invoice
```sql
SELECT o.OrderId, o.OrderDate,
       (SELECT ProductId, Quantity FROM OrderItems oi WHERE oi.OrderId = o.OrderId FOR XML PATH('Item'), TYPE)
FROM Orders o
WHERE OrderId = 1234
FOR XML PATH('Invoice'), ROOT('Invoices');
```
**What**: Invoice format.
**Why**: Export compliance.
**How**: XML PATH + nested.

---

### 21. PIVOT using dynamic categories
```sql
DECLARE @cols NVARCHAR(MAX), @query NVARCHAR(MAX);
SELECT @cols = STRING_AGG(QUOTENAME(CategoryName), ',')
FROM (SELECT DISTINCT c.CategoryName FROM Products p JOIN Categories c ON p.CategoryId = c.CategoryId) AS t;

SET @query = '
SELECT Region, ' + @cols + '
FROM (
  SELECT c.Region, cat.CategoryName, oi.Quantity
  FROM Orders o
  JOIN Customers c ON o.CustomerId = c.CustomerId
  JOIN OrderItems oi ON o.OrderId = oi.OrderId
  JOIN ProductVariants v ON oi.VariantId = v.VariantId
  JOIN Products p ON v.ProductId = p.ProductId
  JOIN Categories cat ON p.CategoryId = cat.CategoryId
) AS src
PIVOT (
  SUM(Quantity) FOR CategoryName IN (' + @cols + ')
) AS pvt';

EXEC sp_executesql @query;
```
**What**: Dynamic category pivot.
**Why**: Flexible columns.
**How**: Dynamic PIVOT.

---

### 22. Use JSON_VALUE for field access
```sql
DECLARE @json NVARCHAR(MAX) = '{ "Customer": { "Name": "Alice" } }';
SELECT JSON_VALUE(@json, '$.Customer.Name') AS CustomerName;
```
**What**: Extract value.
**Why**: JSON field targeting.
**How**: `JSON_VALUE()`.

---

### 23. JSON_ARRAY_AGG emulation with STRING_AGG
```sql
SELECT CustomerId,
       '[' + STRING_AGG('"' + ProductName + '"', ',') + ']' AS ProductArray
FROM (
    SELECT o.CustomerId, p.ProductName
    FROM Orders o
    JOIN OrderItems oi ON o.OrderId = oi.OrderId
    JOIN ProductVariants v ON oi.VariantId = v.VariantId
    JOIN Products p ON v.ProductId = p.ProductId
) AS t
GROUP BY CustomerId;
```
**What**: Emulate JSON array.
**Why**: Compatibility.
**How**: Manual string format.

---

### 24. JSON_QUERY for entire nested object
```sql
DECLARE @json NVARCHAR(MAX) = '{ "Customer": { "Id": 1, "Name": "Bob" } }';
SELECT JSON_QUERY(@json, '$.Customer') AS CustomerObj;
```
**What**: Extract object.
**Why**: Preserve JSON.
**How**: `JSON_QUERY()`.

---

### 25. Aggregate sales using GROUPING SETS
```sql
SELECT c.Region, p.CategoryId, SUM(oi.Quantity * oi.PriceAtPurchase) AS Revenue
FROM Orders o
JOIN Customers c ON o.CustomerId = c.CustomerId
JOIN OrderItems oi ON o.OrderId = oi.OrderId
JOIN ProductVariants v ON oi.VariantId = v.VariantId
JOIN Products p ON v.ProductId = p.ProductId
GROUP BY GROUPING SETS (
    (c.Region, p.CategoryId),
    (c.Region),
    (p.CategoryId),
    ()
);
```
**What**: Custom subtotal layout.
**Why**: Reporting cube control.
**How**: `GROUPING SETS()`.

---

# ✅ Stage 8: Triggers, Auditing, and Data Integrity Automation

This stage explores:
- ✅ AFTER and INSTEAD OF triggers
- ✅ Audit trails for inserts, updates, and deletes
- ✅ Data integrity enforcement
- ✅ Automatic calculations and monitoring

Each of the 25 questions includes:
- Practical use case
- What/Why/How explanation
- Full SQL examples with comments

---

### 1. AFTER INSERT trigger to log new orders
```sql
CREATE TRIGGER trg_AuditOrderInsert
ON Orders
AFTER INSERT
AS
BEGIN
    INSERT INTO OrderAuditLog (OrderId, Action, ActionDate)
    SELECT OrderId, 'INSERT', GETDATE()
    FROM inserted;
END;
```
**What**: Logs order creation.
**Why**: For auditing.
**How**: Uses inserted pseudo-table.

---

### 2. AFTER UPDATE trigger to track price changes
```sql
CREATE TRIGGER trg_AuditProductPriceChange
ON Products
AFTER UPDATE
AS
BEGIN
    INSERT INTO PriceAudit (ProductId, OldPrice, NewPrice, ChangeDate)
    SELECT d.ProductId, d.Price, i.Price, GETDATE()
    FROM deleted d
    JOIN inserted i ON d.ProductId = i.ProductId
    WHERE d.Price <> i.Price;
END;
```
**What**: Logs only changed prices.
**Why**: Price policy compliance.
**How**: Compare deleted vs inserted.

---

### 3. INSTEAD OF DELETE to prevent customer deletion if orders exist
```sql
CREATE TRIGGER trg_PreventCustomerDeletion
ON Customers
INSTEAD OF DELETE
AS
BEGIN
    IF EXISTS (
        SELECT 1 FROM deleted d
        JOIN Orders o ON d.CustomerId = o.CustomerId
    )
    BEGIN
        RAISERROR('Cannot delete customers with existing orders.', 16, 1);
        ROLLBACK;
    END
    ELSE
    BEGIN
        DELETE FROM Customers WHERE CustomerId IN (SELECT CustomerId FROM deleted);
    END
END;
```
**What**: Prevent deletes.
**Why**: Keep data integrity.
**How**: Checks before delete.

---

### 4. AFTER INSERT trigger to auto-calculate inventory
```sql
CREATE TRIGGER trg_UpdateInventoryOnOrder
ON OrderItems
AFTER INSERT
AS
BEGIN
    UPDATE Inventory
    SET QuantityAvailable = QuantityAvailable - i.Quantity
    FROM Inventory inv
    JOIN inserted i ON inv.VariantId = i.VariantId;
END;
```
**What**: Auto-decrement stock.
**Why**: Real-time updates.
**How**: INSERT trigger logic.

---

### 5. INSTEAD OF UPDATE for soft edits with versioning
```sql
CREATE TRIGGER trg_SoftEditProduct
ON Products
INSTEAD OF UPDATE
AS
BEGIN
    INSERT INTO ProductVersions (ProductId, ProductName, Price, ModifiedAt)
    SELECT ProductId, ProductName, Price, GETDATE()
    FROM deleted;

    UPDATE Products
    SET ProductName = i.ProductName,
        Price = i.Price
    FROM inserted i
    WHERE Products.ProductId = i.ProductId;
END;
```
**What**: Archives old record.
**Why**: Change history.
**How**: Versioning table.

---

### 6. AFTER DELETE trigger to log removed users
```sql
CREATE TRIGGER trg_LogDeletedUser
ON Customers
AFTER DELETE
AS
BEGIN
    INSERT INTO DeletionLog (EntityId, EntityType, DeletedAt)
    SELECT CustomerId, 'Customer', GETDATE()
    FROM deleted;
END;
```
**What**: Delete log.
**Why**: Accountability.
**How**: INSERT from deleted.

---

### 7. Prevent negative inventory using AFTER INSERT
```sql
CREATE TRIGGER trg_CheckInventoryLimits
ON OrderItems
AFTER INSERT
AS
BEGIN
    IF EXISTS (
        SELECT 1 FROM inserted i
        JOIN Inventory inv ON i.VariantId = inv.VariantId
        WHERE inv.QuantityAvailable - i.Quantity < 0
    )
    BEGIN
        RAISERROR('Inventory cannot go negative.', 16, 1);
        ROLLBACK;
    END
END;
```
**What**: Inventory guard.
**Why**: Prevent over-selling.
**How**: Post-insert check.

---

### 8. Log refund creation
```sql
CREATE TRIGGER trg_LogRefundInsert
ON Refunds
AFTER INSERT
AS
BEGIN
    INSERT INTO RefundAudit (RefundId, OrderId, Action, Timestamp)
    SELECT RefundId, OrderId, 'Refund Issued', GETDATE()
    FROM inserted;
END;
```
**What**: Track refunds.
**Why**: Customer care.
**How**: Triggered insert.

---

### 9. Trigger to log login attempts
```sql
CREATE TRIGGER trg_LogLoginAttempt
ON LoginEvents
AFTER INSERT
AS
BEGIN
    INSERT INTO LoginAuditLog (UserId, Status, Timestamp)
    SELECT UserId, Status, GETDATE()
    FROM inserted;
END;
```
**What**: Login trail.
**Why**: Security review.
**How**: Trigger insert.

---

### 10. Block product updates during sale event
```sql
CREATE TRIGGER trg_BlockUpdateDuringSale
ON Products
AFTER UPDATE
AS
BEGIN
    IF EXISTS (SELECT 1 FROM inserted WHERE IsOnSale = 1)
    BEGIN
        RAISERROR('Product cannot be updated during sale.', 16, 1);
        ROLLBACK;
    END
END;
```
**What**: Temporary lock.
**Why**: Avoid price issues.
**How**: Conditional check.

---

### 11. Log changes to email field
```sql
CREATE TRIGGER trg_EmailChangeLog
ON Customers
AFTER UPDATE
AS
BEGIN
    INSERT INTO EmailChangeLog (CustomerId, OldEmail, NewEmail, ChangedAt)
    SELECT d.CustomerId, d.Email, i.Email, GETDATE()
    FROM deleted d
    JOIN inserted i ON d.CustomerId = i.CustomerId
    WHERE d.Email <> i.Email;
END;
```
**What**: Email trail.
**Why**: Verification.
**How**: Compare fields.

---

### 12. Create trigger to enforce max order value
```sql
CREATE TRIGGER trg_MaxOrderValue
ON Orders
AFTER INSERT
AS
BEGIN
    IF EXISTS (
        SELECT 1 FROM inserted i
        JOIN OrderItems oi ON i.OrderId = oi.OrderId
        GROUP BY i.OrderId
        HAVING SUM(oi.Quantity * oi.PriceAtPurchase) > 5000
    )
    BEGIN
        RAISERROR('Order exceeds max limit.', 16, 1);
        ROLLBACK;
    END
END;
```
**What**: Order cap.
**Why**: Fraud prevention.
**How**: Post-insert calculation.

---

### 13. Trigger to log changes in inventory quantity
```sql
CREATE TRIGGER trg_LogInventoryChange
ON Inventory
AFTER UPDATE
AS
BEGIN
    INSERT INTO InventoryAudit (VariantId, OldQty, NewQty, ChangedAt)
    SELECT d.VariantId, d.QuantityAvailable, i.QuantityAvailable, GETDATE()
    FROM deleted d
    JOIN inserted i ON d.VariantId = i.VariantId
    WHERE d.QuantityAvailable <> i.QuantityAvailable;
END;
```
**What**: Quantity audit.
**Why**: Warehouse accuracy.
**How**: Track delta.

---

### 14. Prevent deleting default shipping address
```sql
CREATE TRIGGER trg_BlockDefaultShippingDelete
ON ShippingAddresses
INSTEAD OF DELETE
AS
BEGIN
    IF EXISTS (
        SELECT 1 FROM deleted WHERE IsDefault = 1
    )
    BEGIN
        RAISERROR('Cannot delete default shipping address.', 16, 1);
        ROLLBACK;
    END
    ELSE
    BEGIN
        DELETE FROM ShippingAddresses WHERE AddressId IN (SELECT AddressId FROM deleted);
    END
END;
```
**What**: Protect key info.
**Why**: UX/data quality.
**How**: Trigger + ROLLBACK.

---

### 15. Log category changes in products
```sql
CREATE TRIGGER trg_CategoryChangeLog
ON Products
AFTER UPDATE
AS
BEGIN
    INSERT INTO CategoryAudit (ProductId, OldCategory, NewCategory, ChangedAt)
    SELECT d.ProductId, d.CategoryId, i.CategoryId, GETDATE()
    FROM deleted d
    JOIN inserted i ON d.ProductId = i.ProductId
    WHERE d.CategoryId <> i.CategoryId;
END;
```
**What**: Track reclassifications.
**Why**: Catalog accuracy.
**How**: ID check.

---

### 16. Automatically record order timestamp
```sql
CREATE TRIGGER trg_AutoTimestampOrder
ON Orders
AFTER INSERT
AS
BEGIN
    UPDATE Orders
    SET CreatedAt = GETDATE()
    FROM inserted i
    WHERE Orders.OrderId = i.OrderId;
END;
```
**What**: Auto-timestamp.
**Why**: Tracking.
**How**: Update on insert.

---

### 17. Enforce foreign key check via trigger
```sql
CREATE TRIGGER trg_CheckCustomerExists
ON Orders
AFTER INSERT
AS
BEGIN
    IF EXISTS (
        SELECT 1 FROM inserted i
        LEFT JOIN Customers c ON i.CustomerId = c.CustomerId
        WHERE c.CustomerId IS NULL
    )
    BEGIN
        RAISERROR('Customer ID does not exist.', 16, 1);
        ROLLBACK;
    END
END;
```
**What**: Manual FK.
**Why**: Logical layer enforcement.
**How**: Left join NULL.

---

### 18. Track last login in Users table
```sql
CREATE TRIGGER trg_UpdateLastLogin
ON LoginEvents
AFTER INSERT
AS
BEGIN
    UPDATE Users
    SET LastLogin = GETDATE()
    FROM inserted i
    WHERE Users.UserId = i.UserId;
END;
```
**What**: Last seen update.
**Why**: Session control.
**How**: Triggered update.

---

### 19. Deny updates to read-only fields
```sql
CREATE TRIGGER trg_PreventReadOnlyUpdate
ON Products
AFTER UPDATE
AS
BEGIN
    IF EXISTS (
        SELECT 1 FROM inserted i
        JOIN deleted d ON i.ProductId = d.ProductId
        WHERE i.SKU <> d.SKU
    )
    BEGIN
        RAISERROR('Cannot update SKU (read-only).', 16, 1);
        ROLLBACK;
    END
END;
```
**What**: Enforce immutability.
**Why**: Consistency.
**How**: Column diff check.

---

### 20. Create row-based audit log for updates
```sql
CREATE TRIGGER trg_RowAuditOrders
ON Orders
AFTER UPDATE
AS
BEGIN
    INSERT INTO OrderUpdateAudit (OrderId, FieldName, OldValue, NewValue, UpdatedAt)
    SELECT d.OrderId, 'Status', d.Status, i.Status, GETDATE()
    FROM deleted d
    JOIN inserted i ON d.OrderId = i.OrderId
    WHERE d.Status <> i.Status;
END;
```
**What**: Audit field-level.
**Why**: Detailed tracking.
**How**: Audit log structure.

---

### 21. Log cart abandonment
```sql
CREATE TRIGGER trg_LogAbandonedCart
ON Carts
AFTER UPDATE
AS
BEGIN
    INSERT INTO CartAuditLog (CartId, Action, Timestamp)
    SELECT CartId, 'Abandoned', GETDATE()
    FROM inserted
    WHERE IsCheckedOut = 0 AND LastUpdated < DATEADD(HOUR, -24, GETDATE());
END;
```
**What**: Record stale carts.
**Why**: Marketing recovery.
**How**: Time filter.

---

### 22. Track delivery delay changes
```sql
CREATE TRIGGER trg_TrackDelayChange
ON Shipments
AFTER UPDATE
AS
BEGIN
    INSERT INTO ShipmentDelayAudit (ShipmentId, OldExpected, NewExpected, ChangedAt)
    SELECT d.ShipmentId, d.ExpectedDeliveryDate, i.ExpectedDeliveryDate, GETDATE()
    FROM deleted d
    JOIN inserted i ON d.ShipmentId = i.ShipmentId
    WHERE d.ExpectedDeliveryDate <> i.ExpectedDeliveryDate;
END;
```
**What**: SLA breach review.
**Why**: Ops accountability.
**How**: Compare old/new.

---

### 23. Prevent duplicate refunds
```sql
CREATE TRIGGER trg_BlockDuplicateRefund
ON Refunds
AFTER INSERT
AS
BEGIN
    IF EXISTS (
        SELECT r.OrderId FROM inserted i
        JOIN Refunds r ON i.OrderId = r.OrderId
        GROUP BY r.OrderId
        HAVING COUNT(*) > 1
    )
    BEGIN
        RAISERROR('Duplicate refund detected.', 16, 1);
        ROLLBACK;
    END
END;
```
**What**: Refund control.
**Why**: Stop over-refunding.
**How**: Group and count.

---

### 24. Update loyalty points after order
```sql
CREATE TRIGGER trg_LoyaltyPointsUpdate
ON Orders
AFTER INSERT
AS
BEGIN
    UPDATE Customers
    SET LoyaltyPoints = LoyaltyPoints + 10
    FROM Customers c
    JOIN inserted i ON c.CustomerId = i.CustomerId;
END;
```
**What**: Reward automation.
**Why**: Loyalty system.
**How**: INSERT → UPDATE.

---

### 25. Log system-level deletes by admin
```sql
CREATE TRIGGER trg_LogAdminDeletes
ON AdminActions
AFTER INSERT
AS
BEGIN
    INSERT INTO AdminAuditTrail (AdminId, ActionType, TargetTable, ActionDate)
    SELECT AdminId, ActionType, TargetTable, GETDATE()
    FROM inserted;
END;
```
**What**: Admin traceability.
**Why**: Governance.
**How**: Track action logs.

---

# ✅ Stage 9: Query Optimization, Execution Plans & Performance Tuning

This stage focuses on:
- ✅ Identifying slow queries
- ✅ Using execution plans
- ✅ Indexing, statistics, and best practices
- ✅ Avoiding performance pitfalls

Each of the 25 questions includes:
- What the optimization problem is
- Why it matters
- How to fix or analyze it with SQL

---

### 1. Identify slow queries using sys.dm_exec_requests
```sql
SELECT session_id, status, blocking_session_id, wait_type, cpu_time, total_elapsed_time
FROM sys.dm_exec_requests
WHERE status = 'running';
```
**What**: Find long-running queries.
**Why**: Locate bottlenecks.
**How**: Use dynamic management views.

---

### 2. Use execution plan to understand query performance
```sql
-- Click on "Include Actual Execution Plan" in SSMS before running a query:
SELECT * FROM Orders WHERE OrderDate >= '2024-01-01';
```
**What**: Visual query map.
**Why**: Analyze cost.
**How**: SSMS tool tip shows plan.

---

### 3. Missing index suggestion from execution plan
```sql
-- Run this and see plan for index recommendation:
SELECT * FROM Orders WHERE CustomerId = 123 AND OrderDate >= '2024-01-01';
```
**What**: Index hint.
**Why**: Improve reads.
**How**: Add recommended index.

---

### 4. Identify missing indexes using DMVs
```sql
SELECT *
FROM sys.dm_db_missing_index_details;
```
**What**: Find missing indexes.
**Why**: Guide tuning.
**How**: DMV.

---

### 5. Compare logical reads between indexed vs non-indexed column
```sql
SET STATISTICS IO ON;
SELECT * FROM Orders WHERE OrderId = 123;
SELECT * FROM Orders WHERE MONTH(OrderDate) = 1;
```
**What**: Read difference.
**Why**: Understand I/O impact.
**How**: Use SET STATISTICS IO.

---

### 6. Create non-clustered index for frequent filter
```sql
CREATE NONCLUSTERED INDEX idx_Order_CustomerId ON Orders (CustomerId);
```
**What**: Index by column.
**Why**: Improve WHERE performance.
**How**: Add NCI.

---

### 7. Use INCLUDE columns in index to cover query
```sql
CREATE NONCLUSTERED INDEX idx_CoveringOrder ON Orders (CustomerId) INCLUDE (OrderDate, TotalAmount);
```
**What**: Cover query fields.
**Why**: Avoid lookups.
**How**: INCLUDE clause.

---

### 8. Check query plan for key lookup (bad sign)
```sql
-- Use execution plan to find Key Lookup warnings:
SELECT CustomerId, OrderDate FROM Orders WHERE CustomerId = 42;
```
**What**: Key lookup.
**Why**: Indicates missing INCLUDE.
**How**: Tune with covering index.

---

### 9. Use parameterized queries to reuse plans
```sql
DECLARE @cid INT = 123;
SELECT * FROM Orders WHERE CustomerId = @cid;
```
**What**: Plan reuse.
**Why**: Reduce compilation.
**How**: Use parameters.

---

### 10. Avoid function use in WHERE clause
```sql
-- Bad (non-sargable):
SELECT * FROM Orders WHERE MONTH(OrderDate) = 1;
-- Good:
SELECT * FROM Orders WHERE OrderDate >= '2024-01-01' AND OrderDate < '2024-02-01';
```
**What**: SARGability.
**Why**: Index usage.
**How**: Rewrite predicates.

---

### 11. Detect blocking with sys.dm_os_waiting_tasks
```sql
SELECT * FROM sys.dm_os_waiting_tasks WHERE blocking_session_id IS NOT NULL;
```
**What**: See blockers.
**Why**: Resolve contention.
**How**: DMV analysis.

---

### 12. Detect fragmented indexes
```sql
SELECT *
FROM sys.dm_db_index_physical_stats(DB_ID(), NULL, NULL, NULL, 'LIMITED')
WHERE avg_fragmentation_in_percent > 30;
```
**What**: Index health.
**Why**: Reorganize/rebuild.
**How**: DMV index stats.

---

### 13. Rebuild or reorganize index
```sql
ALTER INDEX idx_Order_CustomerId ON Orders REBUILD;
-- Or:
ALTER INDEX idx_Order_CustomerId ON Orders REORGANIZE;
```
**What**: Fix fragmentation.
**Why**: Performance boost.
**How**: ALTER INDEX.

---

### 14. Update statistics manually
```sql
UPDATE STATISTICS Orders;
```
**What**: Refresh stats.
**Why**: Accurate plans.
**How**: Manual trigger.

---

### 15. Use indexed views for performance
```sql
CREATE VIEW vw_OrderRevenue
WITH SCHEMABINDING AS
SELECT CustomerId, COUNT_BIG(*) AS TotalOrders, SUM(TotalAmount) AS Revenue
FROM dbo.Orders
GROUP BY CustomerId;

CREATE UNIQUE CLUSTERED INDEX idx_OrderRevenue ON vw_OrderRevenue (CustomerId);
```
**What**: Pre-aggregated data.
**Why**: Fast reads.
**How**: Indexed view.

---

### 16. Compare table scans vs seeks
```sql
-- Use execution plan to check if query does seek or scan:
SELECT * FROM Orders WHERE CustomerId = 456;
```
**What**: Access method.
**Why**: Index validation.
**How**: Execution plan type.

---

### 17. Avoid SELECT * in production
```sql
-- Bad:
SELECT * FROM Products;
-- Good:
SELECT ProductId, ProductName, Price FROM Products;
```
**What**: Reduces I/O.
**Why**: Leaner queries.
**How**: Select specific columns.

---

### 18. Use proper data types (avoid implicit conversions)
```sql
-- This causes scan due to conversion:
SELECT * FROM Orders WHERE CustomerId = '123';
-- Correct:
SELECT * FROM Orders WHERE CustomerId = 123;
```
**What**: Data type mismatch.
**Why**: Forces conversion.
**How**: Match data types.

---

### 19. Monitor plan cache for expensive queries
```sql
SELECT TOP 10
    qs.total_elapsed_time / qs.execution_count AS AvgElapsedTime,
    qs.execution_count,
    qt.text
FROM sys.dm_exec_query_stats qs
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) qt
ORDER BY AvgElapsedTime DESC;
```
**What**: Find slow queries.
**Why**: Tune top offenders.
**How**: DMVs.

---

### 20. Use FORCESEEK to override optimizer
```sql
SELECT * FROM Orders WITH (FORCESEEK)
WHERE CustomerId = 123;
```
**What**: Override scan.
**Why**: Index enforcement.
**How**: Query hint.

---

### 21. Detect parameter sniffing issue
```sql
-- Use different execution plan for same query:
EXEC sp_executesql N'SELECT * FROM Orders WHERE CustomerId = @id', N'@id INT', @id = 1;
```
**What**: Varies by input.
**Why**: Plan mismatch.
**How**: Sniffing analysis.

---

### 22. Use OPTION(RECOMPILE) for one-time optimization
```sql
SELECT * FROM Orders WHERE CustomerId = 123 OPTION(RECOMPILE);
```
**What**: Forces new plan.
**Why**: Avoid reuse issues.
**How**: Recompile hint.

---

### 23. Use EXISTS instead of COUNT(*) > 0
```sql
-- Better:
IF EXISTS (SELECT 1 FROM Orders WHERE CustomerId = 123)
-- Slower:
IF (SELECT COUNT(*) FROM Orders WHERE CustomerId = 123) > 0
```
**What**: Short-circuit.
**Why**: Faster.
**How**: EXISTS.

---

### 24. Monitor wait stats globally
```sql
SELECT * FROM sys.dm_os_wait_stats ORDER BY wait_time_ms DESC;
```
**What**: System waits.
**Why**: Detect blocking/resource bottlenecks.
**How**: Wait stats.

---

### 25. Use SET STATISTICS TIME and IO for manual benchmarking
```sql
SET STATISTICS TIME ON;
SET STATISTICS IO ON;
SELECT * FROM Orders WHERE OrderId = 100;
```
**What**: See actual query cost.
**Why**: Optimization proof.
**How**: SSMS outputs CPU/reads.

---

# ✅ Stage 10: SQL Server Security, Permissions, and Hardening

This stage focuses on:
- ✅ Managing users and roles
- ✅ Granting and revoking permissions
- ✅ Row-level security and schema hardening
- ✅ Best practices for secure database operations

Each of the 25 questions includes:
- Real-world use case
- What, Why, and How explanation
- Fully commented SQL examples

---

### 1. Create a SQL login and user for a developer
```sql
CREATE LOGIN DevUser WITH PASSWORD = 'StrongPassword123';
CREATE USER DevUser FOR LOGIN DevUser;
```
**What**: Creates SQL login and maps it to DB user.
**Why**: Authenticated DB access.
**How**: Two-step login-to-user mapping.

---

### 2. Grant SELECT permission on Products to DevUser
```sql
GRANT SELECT ON Products TO DevUser;
```
**What**: Allows read access.
**Why**: Least privilege principle.
**How**: `GRANT` specific table permission.

---

### 3. Revoke SELECT permission from DevUser
```sql
REVOKE SELECT ON Products FROM DevUser;
```
**What**: Removes access.
**Why**: Access restriction.
**How**: `REVOKE` statement.

---

### 4. Create a database role and assign permissions
```sql
CREATE ROLE SalesReporter;
GRANT SELECT ON Orders TO SalesReporter;
GRANT SELECT ON OrderItems TO SalesReporter;
EXEC sp_addrolemember 'SalesReporter', 'DevUser';
```
**What**: Role-based control.
**Why**: Easier group permission.
**How**: Role creation + assignment.

---

### 5. Deny DELETE on Products table for DevUser
```sql
DENY DELETE ON Products TO DevUser;
```
**What**: Prevents data loss.
**Why**: Safety net.
**How**: `DENY` overrides `GRANT`.

---

### 6. Check effective permissions for a user
```sql
SELECT * FROM fn_my_permissions(NULL, 'DATABASE');
```
**What**: Lists active rights.
**Why**: Review permissions.
**How**: Built-in function.

---

### 7. Assign schema-level permission
```sql
GRANT SELECT, INSERT ON SCHEMA::Sales TO DevUser;
```
**What**: Grant at schema level.
**Why**: Broad table coverage.
**How**: SCHEMA keyword.

---

### 8. Create contained database user without login
```sql
CREATE USER AppUser WITH PASSWORD = 'AppSecurePass1!';
```
**What**: Loginless access.
**Why**: Supports cloud/contained DB.
**How**: Local-only user.

---

### 9. Enable row-level security (RLS) for Orders
```sql
CREATE FUNCTION fn_RLSFilter (@UserId INT)
RETURNS TABLE
WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS Result WHERE @UserId = USER_ID();

CREATE SECURITY POLICY OrderPolicy
ADD FILTER PREDICATE dbo.fn_RLSFilter(UserId) ON dbo.Orders
WITH (STATE = ON);
```
**What**: Restrict by user.
**Why**: Enforce access rows.
**How**: RLS policy.

---

### 10. Create a login with default database
```sql
CREATE LOGIN Analyst WITH PASSWORD = 'StrongPwd!456', DEFAULT_DATABASE = SalesDB;
```
**What**: Sets login defaults.
**Why**: Dev/analyst convenience.
**How**: `DEFAULT_DATABASE` clause.

---

### 11. Create a read-only role for analysts
```sql
CREATE ROLE ReadOnlyAccess;
GRANT SELECT ON SCHEMA::dbo TO ReadOnlyAccess;
EXEC sp_addrolemember 'ReadOnlyAccess', 'Analyst';
```
**What**: Read-only role.
**Why**: Data protection.
**How**: SCHEMA-based GRANT.

---

### 12. Drop a user and role safely
```sql
EXEC sp_droprolemember 'SalesReporter', 'DevUser';
DROP USER DevUser;
DROP ROLE SalesReporter;
```
**What**: Cleanup access.
**Why**: Avoid orphan roles/users.
**How**: Use `sp_droprolemember` first.

---

### 13. Enforce password policy on login creation
```sql
CREATE LOGIN SecureLogin WITH PASSWORD = 'MySecurePass123!' CHECK_POLICY = ON;
```
**What**: Follows Windows policy.
**Why**: Enforce strength.
**How**: CHECK_POLICY clause.

---

### 14. View all database users
```sql
SELECT name, type_desc FROM sys.database_principals
WHERE type IN ('S', 'U', 'G') AND name NOT LIKE 'db_%';
```
**What**: User overview.
**Why**: Audit access.
**How**: System table filter.

---

### 15. Grant stored procedure execution only
```sql
GRANT EXECUTE ON dbo.GetCustomerOrders TO DevUser;
```
**What**: Allow use, not data.
**Why**: Secure API model.
**How**: GRANT EXECUTE.

---

### 16. Assign CONTROL permission to manage object
```sql
GRANT CONTROL ON Orders TO DevUser;
```
**What**: Full object-level control.
**Why**: Object owner privileges.
**How**: CONTROL = highest object-level right.

---

### 17. Deny DROP on a table
```sql
DENY ALTER, CONTROL ON Products TO DevUser;
```
**What**: Prevent object drop.
**Why**: Hardening.
**How**: DENY key permissions.

---

### 18. Prevent privilege escalation
```sql
-- NEVER grant ALTER ANY LOGIN or CONTROL SERVER unless absolutely required
DENY ALTER ANY LOGIN TO DevUser;
```
**What**: Avoid escalation.
**Why**: Security boundary.
**How**: DENY at server level.

---

### 19. Assign db_datareader and db_datawriter roles
```sql
EXEC sp_addrolemember 'db_datareader', 'DevUser';
EXEC sp_addrolemember 'db_datawriter', 'DevUser';
```
**What**: Built-in roles.
**Why**: Default read/write.
**How**: `sp_addrolemember`.

---

### 20. Create login with expiration policy
```sql
CREATE LOGIN TempUser WITH PASSWORD = 'TempPass123!' MUST_CHANGE, CHECK_EXPIRATION = ON;
```
**What**: Enforce change.
**Why**: Expiring credentials.
**How**: MUST_CHANGE + CHECK_EXPIRATION.

---

### 21. View current user's permissions
```sql
SELECT * FROM fn_my_permissions(NULL, 'DATABASE');
```
**What**: Session audit.
**Why**: Verify access.
**How**: Built-in function.

---

### 22. Limit user to specific IP using firewall (external)
```bash
# Outside SQL Server – configure firewall rule to restrict source IP
```
**What**: Layered security.
**Why**: Network-level protection.
**How**: Infrastructure config.

---

### 23. Restrict UPDATE to only certain columns
```sql
GRANT UPDATE (Email, PhoneNumber) ON Customers TO DevUser;
```
**What**: Partial update.
**Why**: Field-level control.
**How**: Column-specific GRANT.

---

### 24. View all user permissions on a table
```sql
SELECT * FROM fn_my_permissions('Products', 'OBJECT');
```
**What**: Granular check.
**Why**: Validate access.
**How**: fn_my_permissions.

---

### 25. Audit schema changes using DDL triggers
```sql
CREATE TRIGGER trg_DDL_Log
ON DATABASE
FOR CREATE_TABLE, ALTER_TABLE, DROP_TABLE
AS
BEGIN
    INSERT INTO SchemaAuditLog (EventType, ObjectName, EventTime)
    SELECT EVENTDATA().value('(/EVENT_INSTANCE/EventType)[1]', 'NVARCHAR(100)'),
           EVENTDATA().value('(/EVENT_INSTANCE/ObjectName)[1]', 'NVARCHAR(100)'),
           GETDATE();
END;
```
**What**: Schema change trace.
**Why**: Governance.
**How**: DDL trigger + EVENTDATA.

---
# ✅ Stage 11: ETL, SQL Jobs, Scheduling & Automation

This stage covers:
- ✅ Extract, Transform, Load (ETL) logic in SQL
- ✅ SQL Server Agent Jobs
- ✅ Scheduling and Alerts
- ✅ Automating data flows and maintenance

Each question includes:
- Use case scenario
- What, Why, and How explanation
- SQL or T-SQL example with comments

---

### 1. Create a staging table for ETL
```sql
CREATE TABLE StagingOrders (
    OrderId INT,
    CustomerId INT,
    OrderDate DATETIME,
    Amount DECIMAL(10,2),
    ImportBatchId UNIQUEIDENTIFIER DEFAULT NEWID()
);
```
**What**: Temporary staging table.
**Why**: Separate raw imports.
**How**: Create a temp storage.

---

### 2. Insert data into staging from flat source
```sql
BULK INSERT StagingOrders
FROM 'C:\Data\orders_2024.csv'
WITH (
    FIELDTERMINATOR = ',',
    ROWTERMINATOR = '\n',
    FIRSTROW = 2
);
```
**What**: Load raw file.
**Why**: Flat file integration.
**How**: Use `BULK INSERT`.

---

### 3. Cleanse and transform staged data
```sql
UPDATE StagingOrders
SET Amount = ISNULL(Amount, 0),
    OrderDate = ISNULL(OrderDate, GETDATE());
```
**What**: Replace nulls.
**Why**: Data consistency.
**How**: `ISNULL` for null handling.

---

### 4. Insert from staging to production with filter
```sql
INSERT INTO Orders (OrderId, CustomerId, OrderDate, TotalAmount)
SELECT OrderId, CustomerId, OrderDate, Amount
FROM StagingOrders
WHERE Amount > 0;
```
**What**: ETL load step.
**Why**: Exclude invalid data.
**How**: Filtered insert.

---

### 5. Create a stored procedure to automate ETL load
```sql
CREATE PROCEDURE sp_Load_Orders_From_Staging
AS
BEGIN
    DELETE FROM OrdersBackup;
    INSERT INTO OrdersBackup SELECT * FROM Orders;

    INSERT INTO Orders (OrderId, CustomerId, OrderDate, TotalAmount)
    SELECT OrderId, CustomerId, OrderDate, Amount
    FROM StagingOrders;

    DELETE FROM StagingOrders;
END;
```
**What**: Encapsulated ETL logic.
**Why**: Reusability.
**How**: Procedure with staging.

---

### 6. Create SQL Server Agent job to call ETL proc
```sql
-- Use SSMS GUI or script:
-- Job Step: EXEC sp_Load_Orders_From_Staging
```
**What**: Scheduled ETL.
**Why**: Automation.
**How**: Agent job runs stored proc.

---

### 7. Schedule job to run daily at 2 AM
```sql
-- In SQL Agent: create a new schedule
-- Frequency: Recurring, Daily
-- Time: 02:00 AM
```
**What**: Automate execution.
**Why**: Nightly batch.
**How**: SQL Server Agent schedule.

---

### 8. Archive old orders to history table
```sql
INSERT INTO OrdersHistory
SELECT * FROM Orders WHERE OrderDate < DATEADD(YEAR, -2, GETDATE());

DELETE FROM Orders WHERE OrderDate < DATEADD(YEAR, -2, GETDATE());
```
**What**: Archive data.
**Why**: Keep DB lean.
**How**: Insert + delete.

---

### 9. Log ETL run start and end times
```sql
INSERT INTO ETLLog (Step, RunTime)
VALUES ('Start Load', GETDATE());
-- ETL Logic Here
INSERT INTO ETLLog (Step, RunTime)
VALUES ('End Load', GETDATE());
```
**What**: ETL tracking.
**Why**: Debug and audit.
**How**: Log time.

---

### 10. Handle duplicate rows using ROW_NUMBER()
```sql
WITH CTE AS (
  SELECT *,
         ROW_NUMBER() OVER (PARTITION BY OrderId ORDER BY OrderDate DESC) AS rn
  FROM StagingOrders
)
DELETE FROM CTE WHERE rn > 1;
```
**What**: Deduplication.
**Why**: Avoid key violations.
**How**: Use window function.

---

### 11. Truncate staging after successful ETL
```sql
TRUNCATE TABLE StagingOrders;
```
**What**: Cleanup.
**Why**: Reset for next batch.
**How**: Fast truncate.

---

### 12. Log failed records to error table
```sql
INSERT INTO ETLErrorLog
SELECT * FROM StagingOrders WHERE Amount IS NULL;
```
**What**: Capture bad data.
**Why**: Investigate failures.
**How**: Conditional insert.

---

### 13. Send email alert if ETL fails
```sql
-- In Agent Job: configure notification on failure to send operator email
```
**What**: Alert system.
**Why**: Reliability.
**How**: Agent notifications.

---

### 14. Generate audit summary of ETL batch
```sql
SELECT COUNT(*) AS TotalRows, MAX(OrderDate) AS LatestOrder
FROM StagingOrders;
```
**What**: Batch overview.
**Why**: Summary report.
**How**: Aggregation query.

---

### 15. Use TRY_CAST() to validate data
```sql
SELECT * FROM StagingOrders
WHERE TRY_CAST(OrderDate AS DATETIME) IS NULL;
```
**What**: Validate types.
**Why**: Prevent errors.
**How**: TRY_CAST.

---

### 16. Transform using CASE statement
```sql
SELECT *,
       CASE WHEN Amount > 1000 THEN 'High' ELSE 'Normal' END AS Priority
FROM StagingOrders;
```
**What**: Business logic.
**Why**: Flag importance.
**How**: CASE transformation.

---

### 17. Create a job that rebuilds indexes monthly
```sql
-- In Agent: Job step to run:
EXEC sp_MSforeachtable 'ALTER INDEX ALL ON ? REBUILD';
```
**What**: Maintenance.
**Why**: Performance.
**How**: Scheduled SQL Agent job.

---

### 18. Purge logs older than 30 days
```sql
DELETE FROM ETLLog WHERE RunTime < DATEADD(DAY, -30, GETDATE());
```
**What**: Retention policy.
**Why**: Keep logs lean.
**How**: Date filter delete.

---

### 19. Detect failed rows during bulk insert
```sql
-- Use ERRORFILE option to redirect errors:
BULK INSERT StagingOrders
FROM 'C:\orders.csv'
WITH (ERRORFILE = 'C:\errors.txt');
```
**What**: Debug failures.
**Why**: Understand load issues.
**How**: ERRORFILE output.

---

### 20. Monitor job status from msdb
```sql
SELECT job.name, h.run_date, h.run_status
FROM msdb.dbo.sysjobs job
JOIN msdb.dbo.sysjobhistory h ON job.job_id = h.job_id;
```
**What**: Job result.
**Why**: Status tracking.
**How**: msdb system tables.

---

### 21. Track row-level ETL changes
```sql
SELECT OrderId, ChangeType, ChangedAt
FROM ETLChangeLog
WHERE ChangedAt >= CAST(GETDATE() AS DATE);
```
**What**: Auditing changes.
**Why**: Compliance.
**How**: Change log query.

---

### 22. Retry failed ETL batch
```sql
EXEC sp_Load_Orders_From_Staging;
-- After fixing bad data
```
**What**: Recovery step.
**Why**: Fault tolerance.
**How**: Manual retry.

---

### 23. Partition large ETL loads by date
```sql
SELECT * FROM StagingOrders
WHERE OrderDate BETWEEN '2024-01-01' AND '2024-01-31';
```
**What**: Chunk data.
**Why**: Manageable loads.
**How**: Date filtering.

---

### 24. Use temp table in ETL for intermediate result
```sql
SELECT * INTO #FilteredOrders
FROM StagingOrders
WHERE Amount > 0;
```
**What**: Staging temp data.
**Why**: Intermediate transformation.
**How**: SELECT INTO temp table.

---

### 25. Build a summary table for reporting
```sql
SELECT CustomerId, COUNT(*) AS TotalOrders, SUM(Amount) AS TotalAmount
INTO OrderSummary
FROM Orders
GROUP BY CustomerId;
```
**What**: Final load product.
**Why**: Reporting performance.
**How**: Aggregate summary table.

---

# ✅ Stage 12: SQL Server Internals – Transactions, Isolation, ACID, and Concurrency

This stage explores:
- ✅ Understanding Transactions and ACID principles
- ✅ Isolation levels (Read Committed, Repeatable Read, Serializable, Snapshot)
- ✅ Locking and blocking
- ✅ Deadlocks and concurrency handling

Each question includes:
- Real-world use case
- What, Why, and How explanation
- Fully commented SQL examples

## ✅ Stage 12: SQL Server Internals – Transactions, Isolation, ACID, and Concurrency  
---

### 1. **Begin a transaction and commit manually**
```sql
BEGIN TRANSACTION;
    UPDATE Products SET Price = Price + 10 WHERE CategoryId = 1;
COMMIT TRANSACTION;
```
- **What**: Manual control over transaction.
- **Why**: Ensures that all changes are successful before saving to database.
- **How**: Uses `BEGIN` and `COMMIT` to explicitly define transactional boundaries.

---

### 2. **Rollback a transaction on error**
```sql
BEGIN TRANSACTION;
BEGIN TRY
    DELETE FROM Customers WHERE CustomerId = 101;
    COMMIT;
END TRY
BEGIN CATCH
    ROLLBACK;
    PRINT 'Rollback due to error: ' + ERROR_MESSAGE();
END CATCH;
```
- **What**: Rollbacks changes in case of error.
- **Why**: Prevents data inconsistency by rolling back incomplete transactions.
- **How**: Uses `TRY...CATCH` for error detection and rollback.

---

### 3. **Check if you're inside a transaction**
```sql
SELECT @@TRANCOUNT AS OpenTransactions;
```
- **What**: Displays the number of open transactions.
- **Why**: Useful for debugging and verifying nested transaction states.
- **How**: `@@TRANCOUNT` system variable indicates active transaction depth.

---

### 4. **Set isolation level to READ UNCOMMITTED**
```sql
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
SELECT * FROM Orders;
```
- **What**: Reads uncommitted (dirty) data.
- **Why**: Minimizes blocking for fast reporting but risks inconsistent results.
- **How**: Executes with `READ UNCOMMITTED` isolation level.

---

### 5. **Use REPEATABLE READ to lock rows**
```sql
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
BEGIN TRAN;
SELECT * FROM Products WHERE CategoryId = 1;
-- Other sessions cannot update/delete these rows until COMMIT
COMMIT;
```
- **What**: Locks rows read in a transaction.
- **Why**: Prevents phantom updates or non-repeatable reads.
- **How**: Isolation level ensures rows are stable until transaction ends.

---




### 6. Use SERIALIZABLE to block insert of phantom rows
```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
BEGIN TRAN;
SELECT * FROM Products WHERE CategoryId = 1;
-- Prevents any INSERT of new CategoryId = 1 during this transaction
COMMIT;
```
**What**: Highest isolation.
**Why**: Prevent phantom rows.
**How**: SERIALIZABLE.

---

### 7. Use SNAPSHOT isolation for versioned reads
```sql
ALTER DATABASE YourDB SET ALLOW_SNAPSHOT_ISOLATION ON;
SET TRANSACTION ISOLATION LEVEL SNAPSHOT;
BEGIN TRAN;
SELECT * FROM Products;
COMMIT;
```
**What**: Versioned read.
**Why**: No blocking.
**How**: Snapshot mode.

---

### 8. Identify blocking sessions
```sql
SELECT blocking_session_id, session_id, wait_type, wait_time
FROM sys.dm_exec_requests
WHERE blocking_session_id <> 0;
```
**What**: Detect blockers.
**Why**: Performance impact.
**How**: DMV.

---

### 9. Create a deadlock scenario
```sql
-- Session A:
BEGIN TRAN;
UPDATE Products SET Price = Price + 1 WHERE ProductId = 1;
-- Session B:
BEGIN TRAN;
UPDATE Products SET Price = Price + 1 WHERE ProductId = 2;
-- Back in Session A:
UPDATE Products SET Price = Price + 1 WHERE ProductId = 2;
-- Session B does:
UPDATE Products SET Price = Price + 1 WHERE ProductId = 1;
```
**What**: Simulates deadlock.
**Why**: For testing.
**How**: Cross-lock.

---

### 10. Detect deadlocks using system_health extended event
```sql
-- In SSMS:
-- Go to Management > Extended Events > system_health > View Target Data
-- Filter for deadlock events
```
**What**: Find deadlocks.
**Why**: Debug concurrency.
**How**: Use built-in XEvent.

---

### 11. Set XACT_ABORT ON to auto rollback on error
```sql
SET XACT_ABORT ON;
BEGIN TRAN;
UPDATE Orders SET Status = 'Shipped' WHERE OrderId = 1;
-- Force error
SELECT 1/0;
COMMIT;
```
**What**: Rollback on error.
**Why**: Auto safeguard.
**How**: XACT_ABORT.

---

### 12. View active transactions
```sql
SELECT * FROM sys.dm_tran_active_transactions;
```
**What**: List running TXNs.
**Why**: Monitor locks.
**How**: DMV usage.

---

### 13. Detect open transactions by session
```sql
DBCC OPENTRAN;
```
**What**: See oldest open TXN.
**Why**: Prevent log bloat.
**How**: DBCC command.

---

### 14. Monitor locks held by a session
```sql
SELECT request_session_id, resource_type, resource_description
FROM sys.dm_tran_locks;
```
**What**: Lock inventory.
**Why**: Investigate blocking.
**How**: DMV query.

---

### 15. Use READ COMMITTED SNAPSHOT to reduce locking
```sql
ALTER DATABASE YourDB SET READ_COMMITTED_SNAPSHOT ON;
```
**What**: Replace locks with versions.
**Why**: Improves concurrency.
**How**: Version-based reads.

---

### 16. Use explicit transaction across multiple steps
```sql
BEGIN TRAN;
UPDATE Inventory SET QuantityAvailable = QuantityAvailable - 1 WHERE VariantId = 5;
INSERT INTO OrderItems (OrderId, VariantId, Quantity) VALUES (1001, 5, 1);
COMMIT;
```
**What**: Multi-step TXN.
**Why**: Ensure atomic update.
**How**: BEGIN...COMMIT block.

---

### 17. View current isolation level
```sql
DBCC USEROPTIONS;
```
**What**: Session settings.
**Why**: Confirm isolation.
**How**: DBCC.

---

### 18. Demonstrate dirty read
```sql
-- Session 1:
BEGIN TRAN;
UPDATE Products SET Price = 999 WHERE ProductId = 1;
-- Session 2:
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
SELECT * FROM Products WHERE ProductId = 1;
-- Session 1 ROLLBACK
```
**What**: Dirty read.
**Why**: Test visibility.
**How**: Parallel test.

---

### 19. Demonstrate phantom read
```sql
-- Session 1:
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
BEGIN TRAN;
SELECT * FROM Products WHERE CategoryId = 1;
-- Session 2 inserts new Product with CategoryId = 1
-- Session 1: SELECT * again → sees extra row
COMMIT;
```
**What**: Phantom.
**Why**: Test repeatability.
**How**: Row count change.

---

### 20. Demonstrate non-repeatable read
```sql
-- Session 1:
BEGIN TRAN;
SELECT Price FROM Products WHERE ProductId = 1;
-- Session 2 updates that product
-- Session 1: SELECT again → different value
COMMIT;
```
**What**: Changed row.
**Why**: Validate repeatability.
**How**: Cross-session.

---

### 21. Retry transaction using loop pattern
```sql
DECLARE @retry INT = 0;
WHILE @retry < 3
BEGIN
    BEGIN TRY
        BEGIN TRAN;
        -- your SQL
        COMMIT;
        BREAK;
    END TRY
    BEGIN CATCH
        ROLLBACK;
        SET @retry += 1;
        WAITFOR DELAY '00:00:01';
    END CATCH
END;
```
**What**: Retry logic.
**Why**: Handle deadlocks.
**How**: TRY/CATCH loop.

---

### 22. Use application locks
```sql
EXEC sp_getapplock @Resource = 'MyProcess', @LockMode = 'Exclusive';
-- Your SQL logic
EXEC sp_releaseapplock @Resource = 'MyProcess';
```
**What**: Manual locking.
**Why**: Serialize workflow.
**How**: `sp_getapplock()`.

---

### 23. Enable deadlock trace flag
```sql
DBCC TRACEON (1222, -1);
```
**What**: Log deadlocks.
**Why**: Capture event.
**How**: Trace flag.

---

### 24. Prevent deadlocks using ordered access
```sql
-- Always update tables in the same order in every proc
-- e.g., First update Inventory, then Orders
```
**What**: Avoid crossover.
**Why**: Deadlock prevention.
**How**: Standard sequence.

---

### 25. Demonstrate ACID: Atomicity, Consistency, Isolation, Durability
```sql
-- Atomicity: All-or-nothing with COMMIT/ROLLBACK
-- Consistency: Constraints enforced within TXN
-- Isolation: Test with isolation levels
-- Durability: Confirm data persists after COMMIT
```
**What**: ACID principle.
**Why**: Transaction integrity.
**How**: Live testing.

---

# ✅ Stage 13: Advanced SQL Debugging, Logging, and Error Handling

This stage covers:
- ✅ TRY/CATCH blocks for handling errors
- ✅ Logging error messages and context
- ✅ Debugging and tracing stored procedures
- ✅ Handling unexpected behaviors in SQL logic

Each question includes:
- Use case
- What, Why, How explanation
- Code examples with comments

---

### 1. Use TRY/CATCH block in a stored procedure
```sql
BEGIN TRY
    UPDATE Orders SET Status = 'Failed' WHERE OrderId = 99999;
END TRY
BEGIN CATCH
    PRINT 'An error occurred: ' + ERROR_MESSAGE();
END CATCH;
```
**What**: Catch and report error.
**Why**: Graceful failure.
**How**: Wrap with TRY/CATCH.

---

### 2. Log error details to an audit table
```sql
BEGIN TRY
    UPDATE Inventory SET QuantityAvailable = QuantityAvailable - 1 WHERE VariantId = 123;
END TRY
BEGIN CATCH
    INSERT INTO ErrorLog (ErrorMessage, ErrorTime)
    VALUES (ERROR_MESSAGE(), GETDATE());
END CATCH;
```
**What**: Persist error.
**Why**: For analysis.
**How**: ERROR_MESSAGE + INSERT.

---

### 3. Capture error number and severity
```sql
SELECT ERROR_NUMBER() AS ErrorCode, ERROR_SEVERITY() AS Severity, ERROR_MESSAGE() AS Message;
```
**What**: Get context.
**Why**: Improve logs.
**How**: Error functions.

---

### 4. Detect division by zero error
```sql
BEGIN TRY
    SELECT 100 / 0;
END TRY
BEGIN CATCH
    PRINT 'Caught division by zero.';
END CATCH;
```
**What**: Exception case.
**Why**: Prevent failure.
**How**: TRY block.

---

### 5. Prevent divide by zero using NULLIF
```sql
SELECT 100 / NULLIF(@divisor, 0);
```
**What**: Avoid exception.
**Why**: Safe division.
**How**: NULLIF.

---

### 6. Use RAISERROR to return custom error
```sql
IF @Amount <= 0
    RAISERROR('Amount must be greater than zero.', 16, 1);
```
**What**: Manual error.
**Why**: Validation logic.
**How**: RAISERROR.

---

### 7. Add error context to logs
```sql
INSERT INTO ErrorLog (Message, Line, Procedure, Time)
VALUES (
    ERROR_MESSAGE(), ERROR_LINE(), ERROR_PROCEDURE(), GETDATE()
);
```
**What**: Full trace.
**Why**: Better debugging.
**How**: More error functions.

---

### 8. Exit stored procedure on error
```sql
IF @ErrorCode <> 0 RETURN;
```
**What**: Short-circuit.
**Why**: Prevent continued execution.
**How**: RETURN.

---

### 9. Handle insert failure
```sql
BEGIN TRY
    INSERT INTO Orders (OrderId) VALUES (1);
END TRY
BEGIN CATCH
    INSERT INTO ErrorLog (Message) VALUES (ERROR_MESSAGE());
END CATCH;
```
**What**: Insert guard.
**Why**: Audit insert issues.
**How**: Error trap.

---

### 10. Log number of rows affected
```sql
UPDATE Orders SET Status = 'Processed' WHERE Status = 'Pending';
PRINT @@ROWCOUNT;
```
**What**: Affected count.
**Why**: Validate update.
**How**: @@ROWCOUNT.

---

### 11. Capture the exact line number of error
```sql
BEGIN TRY
    SELECT 1 / 0;
END TRY
BEGIN CATCH
    PRINT 'Error occurred on line: ' + CAST(ERROR_LINE() AS NVARCHAR);
END CATCH;
```
**What**: Trace error location.
**Why**: Faster debugging.
**How**: ERROR_LINE().

---

### 12. Identify the stored procedure where error occurred
```sql
BEGIN TRY
    EXEC InvalidProcedure;
END TRY
BEGIN CATCH
    PRINT 'Error in procedure: ' + ISNULL(ERROR_PROCEDURE(), 'Unknown');
END CATCH;
```
**What**: Show source of failure.
**Why**: Targeted fix.
**How**: ERROR_PROCEDURE().

---

### 13. Log all SQL errors into a structured error log table
```sql
CREATE TABLE ErrorLog (
    ErrorId INT IDENTITY(1,1) PRIMARY KEY,
    ErrorMessage NVARCHAR(MAX),
    ErrorSeverity INT,
    ErrorState INT,
    ErrorLine INT,
    ErrorProcedure NVARCHAR(128),
    ErrorDate DATETIME DEFAULT GETDATE()
);
```
**What**: Persistent log table.
**Why**: Error tracking.
**How**: Structured table.

---

### 14. Use THROW to re-throw error after logging
```sql
BEGIN TRY
    SELECT 1 / 0;
END TRY
BEGIN CATCH
    INSERT INTO ErrorLog (ErrorMessage, ErrorSeverity, ErrorLine, ErrorProcedure)
    VALUES (ERROR_MESSAGE(), ERROR_SEVERITY(), ERROR_LINE(), ERROR_PROCEDURE());
    THROW;
END CATCH;
```
**What**: Preserve error stack.
**Why**: Alert application.
**How**: THROW after log.

---

### 15. Use RETURN codes for basic error flow
```sql
CREATE PROCEDURE sp_CheckProductStock
AS
BEGIN
    IF (SELECT QuantityAvailable FROM Inventory WHERE VariantId = 1) <= 0
        RETURN 1;
    RETURN 0;
END;
```
**What**: Exit codes.
**Why**: Simple error signaling.
**How**: RETURN.

---

### 16. Use PRINT for debugging inside procedure
```sql
PRINT 'Starting data validation...';
PRINT 'Validation complete.';
```
**What**: Debug statements.
**Why**: Trace flow.
**How**: PRINT messages.

---

### 17. Validate and skip bad rows using TRY_CONVERT
```sql
SELECT * FROM Orders
WHERE TRY_CONVERT(DATE, OrderDate) IS NOT NULL;
```
**What**: Safe casting.
**Why**: Avoid conversion errors.
**How**: TRY_CONVERT.

---

### 18. Use table variable for intermediate error data
```sql
DECLARE @Errors TABLE (Message NVARCHAR(MAX));
BEGIN TRY
    EXEC SomeBrokenProc;
END TRY
BEGIN CATCH
    INSERT INTO @Errors VALUES (ERROR_MESSAGE());
END CATCH;
SELECT * FROM @Errors;
```
**What**: Temp error capture.
**Why**: Later review.
**How**: Table variable.

---

### 19. Handle batch execution errors with TRY/CATCH
```sql
BEGIN TRY
    EXEC sp_InsertBatchA;
    EXEC sp_InsertBatchB;
END TRY
BEGIN CATCH
    EXEC sp_LogError @Message = ERROR_MESSAGE();
END CATCH;
```
**What**: Wrap batch steps.
**Why**: Capture global failure.
**How**: Outer TRY block.

---

### 20. Add audit trail for every error
```sql
CREATE TRIGGER trg_ErrorAudit
ON dbo.ErrorLog
AFTER INSERT
AS
BEGIN
    INSERT INTO ErrorAudit (ErrorId, Notified, NotificationDate)
    SELECT ErrorId, 0, GETDATE()
    FROM inserted;
END;
```
**What**: Error post-log.
**Why**: Notification layer.
**How**: DML trigger.

---

### 21. Use RAISERROR with custom error number
```sql
RAISERROR (50001, 16, 1) WITH LOG;
```
**What**: Log to Event Viewer.
**Why**: Custom code.
**How**: RAISERROR.

---

### 22. Include input parameters in error log
```sql
BEGIN CATCH
    INSERT INTO ErrorLog (ErrorMessage)
    VALUES ('ProductId=' + CAST(@ProductId AS NVARCHAR) + ' — ' + ERROR_MESSAGE());
END CATCH;
```
**What**: Context logging.
**Why**: Easier tracing.
**How**: Concatenate inputs.

---

### 23. Use nested TRY/CATCH blocks
```sql
BEGIN TRY
    BEGIN TRY
        SELECT 1 / 0;
    END TRY
    BEGIN CATCH
        PRINT 'Inner block caught';
    END CATCH;
END TRY
BEGIN CATCH
    PRINT 'Outer block caught';
END CATCH;
```
**What**: Scoped error.
**Why**: Layered safety.
**How**: Nested blocks.

---

### 24. Create centralized error handling proc
```sql
CREATE PROCEDURE sp_LogError (@Message NVARCHAR(MAX))
AS
BEGIN
    INSERT INTO ErrorLog (ErrorMessage, ErrorDate)
    VALUES (@Message, GETDATE());
END;
```
**What**: Reusable logger.
**Why**: DRY principle.
**How**: Centralized logic.

---

### 25. Log SQL job failures via job history
```sql
SELECT *
FROM msdb.dbo.sysjobhistory
WHERE run_status = 0
ORDER BY run_date DESC;
```
**What**: Job failure log.
**Why**: Operational monitoring.
**How**: SQL Agent history.

---

## ✅ Stage 14: Materialized Views, Indexed Views & SQL Refactoring

This stage covers:
- ✅ Materialized views and indexed views
- ✅ Refactoring complex queries
- ✅ View optimization strategies
- ✅ Refresh logic and schema-bound views

Each question includes:
- Real-world scenario
- What, Why, and How explanation
- SQL code with comments

---

### 1. Create a basic view for customer orders
```sql
CREATE VIEW vw_CustomerOrders AS
SELECT o.OrderId, o.CustomerId, o.OrderDate, SUM(oi.Quantity * oi.PriceAtPurchase) AS TotalAmount
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
GROUP BY o.OrderId, o.CustomerId, o.OrderDate;
```
- **What**: Creates a logical view.
- **Why**: Reusable query for reports.
- **How**: Aggregation + join.

---

### 2. Create a SCHEMABINDING view (required for indexed views)
```sql
CREATE VIEW vw_SalesSummary
WITH SCHEMABINDING
AS
SELECT CustomerId, COUNT_BIG(*) AS TotalOrders, SUM(TotalAmount) AS Revenue
FROM dbo.Orders
GROUP BY CustomerId;
```
- **What**: Schema-bound view.
- **Why**: Required for indexes.
- **How**: `WITH SCHEMABINDING` keyword.

---

### 3. Add a clustered index to materialize the view
```sql
CREATE UNIQUE CLUSTERED INDEX idx_vw_SalesSummary ON vw_SalesSummary (CustomerId);
```
- **What**: Materializes view.
- **Why**: Speeds up SELECT.
- **How**: Required on unique column set.

---

### 4. Refactor subqueries using views
```sql
-- Instead of nesting:
SELECT * FROM (
    SELECT CustomerId, COUNT(*) AS Orders FROM Orders GROUP BY CustomerId
) a
-- Use:
SELECT * FROM vw_SalesSummary;
```
- **What**: Replace nested queries.
- **Why**: Improves readability.
- **How**: Use named view.

---

### 5. View product performance
```sql
CREATE VIEW vw_TopProducts AS
SELECT p.ProductId, p.ProductName, SUM(oi.Quantity) AS TotalSold
FROM Products p
JOIN OrderItems oi ON p.ProductId = oi.ProductId
GROUP BY p.ProductId, p.ProductName;
```
- **What**: Analyze sales.
- **Why**: Useful for dashboards.
- **How**: Join + GROUP BY.

---

### 6. Use views for pagination
```sql
CREATE VIEW vw_PagedProducts AS
SELECT *, ROW_NUMBER() OVER (ORDER BY ProductId) AS RowNum
FROM Products;
```
- **What**: Supports paging.
- **Why**: Backend API scenarios.
- **How**: Add ROW_NUMBER().

---

### 7. Filter data in a view
```sql
CREATE VIEW vw_RecentOrders AS
SELECT * FROM Orders WHERE OrderDate > DATEADD(DAY, -30, GETDATE());
```
- **What**: Only recent data.
- **Why**: Improve performance.
- **How**: Add WHERE clause.

---

### 8. Partition data inside view
```sql
CREATE VIEW vw_OrderRank AS
SELECT *, RANK() OVER (PARTITION BY CustomerId ORDER BY OrderDate DESC) AS OrderRank
FROM Orders;
```
- **What**: Rank orders per customer.
- **Why**: Refactor for leaderboards.
- **How**: Use `RANK()`.

---

### 9. Handle NULLs in view output
```sql
CREATE VIEW vw_CustomerTotals AS
SELECT CustomerId, ISNULL(SUM(TotalAmount), 0) AS TotalSpent
FROM Orders
GROUP BY CustomerId;
```
- **What**: Avoid nulls.
- **Why**: For reporting.
- **How**: Use ISNULL.

---

### 10. Refactor repeated JOIN into view
```sql
CREATE VIEW vw_ProductCategory AS
SELECT p.ProductId, p.ProductName, c.CategoryName
FROM Products p
JOIN Categories c ON p.CategoryId = c.CategoryId;
```
- **What**: Join logic reusable.
- **Why**: Remove duplication.
- **How**: View abstraction.

---

### 11. Add view for financial summary
```sql
CREATE VIEW vw_FinancialSummary AS
SELECT YEAR(OrderDate) AS Year, MONTH(OrderDate) AS Month,
       SUM(TotalAmount) AS Revenue
FROM Orders
GROUP BY YEAR(OrderDate), MONTH(OrderDate);
```
- **What**: Monthly breakdown.
- **Why**: Executive summary.
- **How**: Group by year/month.

---

### 12. Optimize view using indexed column
```sql
CREATE VIEW vw_Optimized AS
SELECT ProductId, ProductName FROM Products WHERE IsActive = 1;
```
- **What**: Leverages index.
- **Why**: Better seek performance.
- **How**: Indexed WHERE field.

---

### 13. Nested views for modular design
```sql
CREATE VIEW vw_OrderItemsDetailed AS
SELECT oi.OrderItemId, o.CustomerId, p.ProductName, oi.Quantity
FROM OrderItems oi
JOIN Orders o ON oi.OrderId = o.OrderId
JOIN Products p ON oi.ProductId = p.ProductId;
```
- **What**: Multi-table abstraction.
- **Why**: Central query reuse.
- **How**: Joins in a view.

---

### 14. Parameterize filtering using table-valued function (TVF)
```sql
CREATE FUNCTION dbo.fn_CustomerOrders (@CustomerId INT)
RETURNS TABLE
AS
RETURN (
    SELECT * FROM Orders WHERE CustomerId = @CustomerId
);
```
- **What**: Acts like view with input.
- **Why**: Dynamic view-like behavior.
- **How**: Table-valued function.

---

### 15. Use view to isolate sensitive columns
```sql
CREATE VIEW vw_PublicOrders AS
SELECT OrderId, OrderDate, Status FROM Orders;
```
- **What**: Prevents access to sensitive data.
- **Why**: Security layer.
- **How**: Limit columns.

---

### 16. Track changes via view on audit log
```sql
CREATE VIEW vw_RecentChanges AS
SELECT * FROM AuditLog WHERE ChangeDate > DATEADD(HOUR, -1, GETDATE());
```
- **What**: Monitors recent activity.
- **Why**: System integrity check.
- **How**: Time-based filter.

---

### 17. Use view with UNION ALL
```sql
CREATE VIEW vw_AllUsers AS
SELECT CustomerId AS Id, Name, 'Customer' AS Type FROM Customers
UNION ALL
SELECT EmployeeId, FullName, 'Employee' FROM Employees;
```
- **What**: Combine sources.
- **Why**: Unified identity view.
- **How**: Use UNION ALL.

---

### 18. Indexed view for computed column
```sql
CREATE VIEW vw_ProductMargin
WITH SCHEMABINDING
AS
SELECT ProductId, Price - Cost AS Margin FROM dbo.Products;
GO
CREATE UNIQUE CLUSTERED INDEX idx_Margin ON vw_ProductMargin (ProductId);
```
- **What**: Store computed values.
- **Why**: Query speed.
- **How**: Use SCHEMABINDING + index.

---

### 19. Refresh a materialized view (manually)
```sql
-- Drop and recreate or reindex view
ALTER INDEX idx_vw_SalesSummary ON vw_SalesSummary REBUILD;
```
- **What**: Refresh stale data.
- **Why**: Improve consistency.
- **How**: Manual refresh.

---

### 20. Use view to support API DTO shape
```sql
CREATE VIEW vw_API_ProductDetails AS
SELECT ProductId, ProductName, Price FROM Products WHERE IsDiscontinued = 0;
```
- **What**: Matches frontend DTO.
- **Why**: API contract.
- **How**: Tailor structure.

---

### 21. Use indexed view for monthly KPI dashboard
```sql
CREATE VIEW vw_KPI_Monthly
WITH SCHEMABINDING
AS
SELECT YEAR(OrderDate) AS Year, MONTH(OrderDate) AS Month,
       COUNT_BIG(*) AS Orders, SUM(TotalAmount) AS Revenue
FROM dbo.Orders
GROUP BY YEAR(OrderDate), MONTH(OrderDate);
GO
CREATE UNIQUE CLUSTERED INDEX idx_KPI_Month ON vw_KPI_Monthly (Year, Month);
```
- **What**: Dashboard boost.
- **Why**: Real-time speed.
- **How**: Indexed grouped view.

---

### 22. Troubleshoot view dependency
```sql
EXEC sp_depends 'vw_CustomerOrders';
```
- **What**: Show dependent objects.
- **Why**: Impact analysis.
- **How**: System stored proc.

---

### 23. Refresh all views in batch
```sql
EXEC sp_refreshview 'vw_CustomerOrders';
EXEC sp_refreshview 'vw_TopProducts';
```
- **What**: Ensures accuracy.
- **Why**: After schema change.
- **How**: Refresh view metadata.

---

### 24. Analyze query plan for view performance
```sql
-- Enable Actual Execution Plan and run:
SELECT * FROM vw_TopProducts;
```
- **What**: Tune performance.
- **Why**: Spot bottlenecks.
- **How**: View execution plan.

---

### 25. Script view to deploy to multiple environments
```sql
-- Use SSMS: Right-click > Script View as > CREATE To > New Query Editor Window
```
- **What**: Reusable definition.
- **Why**: Version-controlled.
- **How**: Generate script.

---

## ✅ Stage 15: Capstone SQL Mastery Review & Project Challenge

This stage is the grand finale of your SQL Mastery journey. It includes:
- ✅ Cumulative knowledge application
- ✅ Real-world business reporting problems
- ✅ Optimization, tuning, refactoring
- ✅ Data modeling and integrity enforcement

Each challenge includes:
- Business case
- What, Why, and How explanation
- Fully commented SQL query

---

### 1. Generate customer purchase history report
```sql
SELECT c.CustomerId, c.Name, o.OrderId, o.OrderDate, SUM(oi.Quantity * oi.PriceAtPurchase) AS Total
FROM Customers c
JOIN Orders o ON c.CustomerId = o.CustomerId
JOIN OrderItems oi ON o.OrderId = oi.OrderId
GROUP BY c.CustomerId, c.Name, o.OrderId, o.OrderDate;
```
- **What**: Detailed purchase record.
- **Why**: For customer behavior insights.
- **How**: Joins + grouping + aggregation.

---

### 2. Find top 5 selling products last month
```sql
SELECT TOP 5 p.ProductName, SUM(oi.Quantity) AS TotalSold
FROM Products p
JOIN OrderItems oi ON p.ProductId = oi.ProductId
JOIN Orders o ON o.OrderId = oi.OrderId
WHERE o.OrderDate BETWEEN DATEADD(MONTH, -1, GETDATE()) AND GETDATE()
GROUP BY p.ProductName
ORDER BY TotalSold DESC;
```
- **What**: Hot product ranking.
- **Why**: Inventory planning.
- **How**: Filter + JOIN + GROUP + ORDER.

---

### 3. Show customer lifetime value
```sql
SELECT CustomerId, SUM(TotalAmount) AS LifetimeValue
FROM Orders
GROUP BY CustomerId;
```
- **What**: Total spend.
- **Why**: Loyalty tracking.
- **How**: Simple group + sum.

---

### 4. Detect customers with no orders
```sql
SELECT c.CustomerId, c.Name
FROM Customers c
LEFT JOIN Orders o ON c.CustomerId = o.CustomerId
WHERE o.OrderId IS NULL;
```
- **What**: Inactive customers.
- **Why**: Retargeting.
- **How**: `LEFT JOIN` + `IS NULL`.

---

### 5. Identify revenue drop by comparing months
```sql
SELECT 
    FORMAT(OrderDate, 'yyyy-MM') AS OrderMonth,
    SUM(TotalAmount) AS Revenue,
    LAG(SUM(TotalAmount)) OVER (ORDER BY FORMAT(OrderDate, 'yyyy-MM')) AS PreviousMonthRevenue
FROM Orders
GROUP BY FORMAT(OrderDate, 'yyyy-MM');
```
- **What**: Month-on-month change.
- **Why**: Trend analysis.
- **How**: `LAG()` function.

---

### 6. Find customers who placed multiple orders on the same day
```sql
SELECT CustomerId, OrderDate, COUNT(*) AS OrderCount
FROM Orders
GROUP BY CustomerId, OrderDate
HAVING COUNT(*) > 1;
```
- **What**: Repeated orders.
- **Why**: Flag duplicates or VIPs.
- **How**: `HAVING` with `COUNT`.

---

### 7. Display top 3 revenue-generating customers
```sql
SELECT TOP 3 CustomerId, SUM(TotalAmount) AS Revenue
FROM Orders
GROUP BY CustomerId
ORDER BY Revenue DESC;
```
- **What**: Identify top customers.
- **Why**: VIP targeting.
- **How**: Sort & limit.

---

### 8. Calculate average order value by customer
```sql
SELECT CustomerId, AVG(TotalAmount) AS AvgOrderValue
FROM Orders
GROUP BY CustomerId;
```
- **What**: Per-order value.
- **Why**: Customer segmentation.
- **How**: `AVG()`.

---

### 9. Count distinct products per order
```sql
SELECT OrderId, COUNT(DISTINCT ProductId) AS UniqueProducts
FROM OrderItems
GROUP BY OrderId;
```
- **What**: Basket diversity.
- **Why**: Upselling insights.
- **How**: `COUNT DISTINCT`.

---

### 10. Use CTE to summarize daily revenue
```sql
WITH DailyRevenue AS (
    SELECT CAST(OrderDate AS DATE) AS Day, SUM(TotalAmount) AS Revenue
    FROM Orders
    GROUP BY CAST(OrderDate AS DATE)
)
SELECT * FROM DailyRevenue;
```
- **What**: Daily report.
- **Why**: Operational summary.
- **How**: CTE for modularity.

---

### 11. Detect missing orders (gap in sequence)
```sql
SELECT OrderId
FROM Orders o
WHERE NOT EXISTS (
    SELECT 1 FROM Orders o2 WHERE o2.OrderId = o.OrderId + 1
);
```
- **What**: Gaps in IDs.
- **Why**: Data integrity.
- **How**: `NOT EXISTS`.

---

### 12. Find orders exceeding category average spend
```sql
SELECT o.OrderId, o.TotalAmount, p.CategoryId
FROM Orders o
JOIN OrderItems oi ON o.OrderId = oi.OrderId
JOIN Products p ON oi.ProductId = p.ProductId
WHERE o.TotalAmount > (
    SELECT AVG(o2.TotalAmount)
    FROM Orders o2
    JOIN OrderItems oi2 ON o2.OrderId = oi2.OrderId
    JOIN Products p2 ON oi2.ProductId = p2.ProductId
    WHERE p2.CategoryId = p.CategoryId
);
```
- **What**: Outliers.
- **Why**: Big-spender detection.
- **How**: Correlated subquery.

---

### 13. Pivot monthly product sales using PIVOT
```sql
SELECT *
FROM (
    SELECT ProductId, FORMAT(OrderDate, 'MMM') AS MonthName, Quantity
    FROM OrderItems oi
    JOIN Orders o ON o.OrderId = oi.OrderId
) src
PIVOT (
    SUM(Quantity) FOR MonthName IN ([Jan], [Feb], [Mar], [Apr], [May])
) pvt;
```
- **What**: Pivot table.
- **Why**: Dashboard charts.
- **How**: `PIVOT`.

---

### 14. Use window function to rank customers
```sql
SELECT CustomerId, SUM(TotalAmount) AS Revenue,
       RANK() OVER (ORDER BY SUM(TotalAmount) DESC) AS CustomerRank
FROM Orders
GROUP BY CustomerId;
```
- **What**: Leaderboard.
- **Why**: Recognize top clients.
- **How**: Window + `RANK()`.

---

### 15. Use CASE for conditional labels
```sql
SELECT ProductId, ProductName,
       CASE 
           WHEN Price > 1000 THEN 'Premium'
           WHEN Price > 500 THEN 'Standard'
           ELSE 'Budget'
       END AS Category
FROM Products;
```
- **What**: Tiering.
- **Why**: Classification.
- **How**: `CASE` logic.

---

### 16. Write a trigger to log order updates
```sql
CREATE TRIGGER trg_LogOrderUpdate
ON Orders
AFTER UPDATE
AS
INSERT INTO OrderAudit (OrderId, ModifiedAt)
SELECT i.OrderId, GETDATE() FROM inserted i;
```
- **What**: Log updates.
- **Why**: Change tracking.
- **How**: `AFTER UPDATE` trigger.

---

### 17. Use JSON to output order details
```sql
SELECT OrderId, CustomerId, OrderDate,
       (SELECT ProductId, Quantity
        FROM OrderItems WHERE OrderId = o.OrderId
        FOR JSON PATH) AS ItemsJson
FROM Orders o;
```
- **What**: JSON structure.
- **Why**: API-ready response.
- **How**: `FOR JSON PATH`.

---

### 18. Track stock status using derived column
```sql
SELECT ProductId, ProductName, QuantityAvailable,
       CASE WHEN QuantityAvailable = 0 THEN 'Out of Stock'
            WHEN QuantityAvailable < 10 THEN 'Low Stock'
            ELSE 'In Stock' END AS StockStatus
FROM Inventory;
```
- **What**: Inventory insight.
- **Why**: Restocking alerts.
- **How**: CASE expression.

---

### 19. Use CROSS APPLY to fetch latest order per customer
```sql
SELECT c.CustomerId, c.Name, o.OrderId, o.OrderDate
FROM Customers c
CROSS APPLY (
    SELECT TOP 1 * FROM Orders o
    WHERE o.CustomerId = c.CustomerId
    ORDER BY o.OrderDate DESC
) o;
```
- **What**: Lateral join.
- **Why**: One-to-many filtering.
- **How**: `CROSS APPLY`.

---

### 20. Create a view for dashboard summary
```sql
CREATE VIEW vw_DashboardSummary AS
SELECT COUNT(*) AS TotalOrders,
       SUM(TotalAmount) AS TotalRevenue,
       COUNT(DISTINCT CustomerId) AS ActiveCustomers
FROM Orders;
```
- **What**: Aggregated stats.
- **Why**: Instant overview.
- **How**: VIEW abstraction.

---

### 21. Use CTE to get orders above average
```sql
WITH OrderAverages AS (
    SELECT AVG(TotalAmount) AS AvgOrderValue FROM Orders
)
SELECT * FROM Orders
WHERE TotalAmount > (SELECT AvgOrderValue FROM OrderAverages);
```
- **What**: Above average spenders.
- **Why**: Outlier detection.
- **How**: Modular CTE.

---

### 22. Archive old orders to history table
```sql
INSERT INTO OrderHistory
SELECT * FROM Orders WHERE OrderDate < DATEADD(YEAR, -2, GETDATE());
DELETE FROM Orders WHERE OrderDate < DATEADD(YEAR, -2, GETDATE());
```
- **What**: Archiving.
- **Why**: DB performance.
- **How**: Insert + delete.

---

### 23. Detect frequent returners (customers with >2 refunds)
```sql
SELECT CustomerId, COUNT(*) AS RefundCount
FROM Refunds
GROUP BY CustomerId
HAVING COUNT(*) > 2;
```
- **What**: High-return customers.
- **Why**: Fraud prevention.
- **How**: GROUP + HAVING.

---

### 24. Optimize query using covering index
```sql
-- Create index for common query pattern
CREATE NONCLUSTERED INDEX idx_Orders_Customer_Date
ON Orders (CustomerId, OrderDate) INCLUDE (TotalAmount);
```
- **What**: Faster reads.
- **Why**: Index tuning.
- **How**: `INCLUDE` clause.

---

### 25. Generate summary export using TEMP table
```sql
SELECT CustomerId, SUM(TotalAmount) AS TotalSpent
INTO #CustomerSummary
FROM Orders
GROUP BY CustomerId;

SELECT * FROM #CustomerSummary;
```
- **What**: Temp summary table.
- **Why**: Export/report.
- **How**: `SELECT INTO` temp table.

---

## 🧠 SQL Mastery Final Mock Exam –  Questions & Answers with Commentary

### ✅ Question 21: Show order totals with discount applied
```sql
-- Calculate 10% discount if total > 1000
SELECT OrderId, TotalAmount,
       CASE WHEN TotalAmount > 1000 THEN TotalAmount * 0.9 ELSE TotalAmount END AS DiscountedTotal
FROM Orders;
```

### ✅ Question 22: Customers with multiple shipping addresses
```sql
SELECT CustomerId, COUNT(*) AS AddressCount
FROM Addresses
GROUP BY CustomerId
HAVING COUNT(*) > 1;
```

### ✅ Question 23: Get average delivery time
```sql
-- Use DATEDIFF between OrderDate and DeliveredDate
SELECT AVG(DATEDIFF(DAY, OrderDate, DeliveredDate)) AS AvgDeliveryDays
FROM Orders
WHERE DeliveredDate IS NOT NULL;
```

### ✅ Question 24: Find orders not delivered within 5 days
```sql
SELECT *
FROM Orders
WHERE DATEDIFF(DAY, OrderDate, DeliveredDate) > 5;
```

### ✅ Question 25: Most popular product per category
```sql
SELECT CategoryId, ProductId, TotalSold
FROM (
  SELECT p.CategoryId, oi.ProductId, SUM(oi.Quantity) AS TotalSold,
         RANK() OVER (PARTITION BY p.CategoryId ORDER BY SUM(oi.Quantity) DESC) AS rk
  FROM Products p
  JOIN OrderItems oi ON p.ProductId = oi.ProductId
  GROUP BY p.CategoryId, oi.ProductId
) a
WHERE rk = 1;
```

### ✅ Question 26: Create a simple stored procedure
```sql
CREATE PROCEDURE sp_GetCustomerOrders @CustomerId INT
AS
BEGIN
    SELECT * FROM Orders WHERE CustomerId = @CustomerId;
END;
```

### ✅ Question 27: Add an AFTER INSERT trigger
```sql
CREATE TRIGGER trg_NewOrderLog
ON Orders
AFTER INSERT
AS
BEGIN
    INSERT INTO OrderLog (OrderId, LoggedAt)
    SELECT OrderId, GETDATE() FROM inserted;
END;
```

### ✅ Question 28: Rebuild indexes on all tables
```sql
EXEC sp_MSforeachtable 'ALTER INDEX ALL ON ? REBUILD';
```

### ✅ Question 29: Use TRY_CAST to handle conversion safely
```sql
SELECT * FROM Orders
WHERE TRY_CAST(OrderDate AS DATETIME) IS NOT NULL;
```

### ✅ Question 30: Log SQL errors using TRY/CATCH
```sql
BEGIN TRY
    -- Risky operation
    DELETE FROM Orders WHERE OrderId = -999;
END TRY
BEGIN CATCH
    INSERT INTO ErrorLog (Message, ErrorDate)
    VALUES (ERROR_MESSAGE(), GETDATE());
END CATCH;
```

### ✅ Question 31: Find 5 most recent orders per customer
```sql
SELECT * FROM (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY CustomerId ORDER BY OrderDate DESC) AS rn
  FROM Orders
) a
WHERE rn <= 5;
```

### ✅ Question 32: Detect gaps in identity column
```sql
SELECT Id + 1 AS MissingFrom
FROM Orders o
WHERE NOT EXISTS (SELECT 1 FROM Orders o2 WHERE o2.Id = o.Id + 1);
```

### ✅ Question 33: Check for SARGable WHERE clause
```sql
-- GOOD (SARGable)
SELECT * FROM Orders WHERE OrderDate >= '2024-01-01';
-- BAD (Not SARGable)
-- SELECT * FROM Orders WHERE YEAR(OrderDate) = 2024;
```

### ✅ Question 34: Count active customers (ordered this year)
```sql
SELECT COUNT(DISTINCT CustomerId)
FROM Orders
WHERE YEAR(OrderDate) = YEAR(GETDATE());
```

### ✅ Question 35: Dynamic SQL to select from table
```sql
DECLARE @TableName NVARCHAR(100) = 'Orders';
EXEC('SELECT TOP 10 * FROM ' + @TableName);
```

### ✅ Question 36: Transactions with rollback on error
```sql
BEGIN TRAN;
BEGIN TRY
    UPDATE Products SET Price = Price + 5;
    COMMIT;
END TRY
BEGIN CATCH
    ROLLBACK;
    PRINT 'Error: ' + ERROR_MESSAGE();
END CATCH;
```

### ✅ Question 37: Use COALESCE to fill NULLs
```sql
SELECT CustomerId, COALESCE(Phone, 'No Phone') AS ContactPhone
FROM Customers;
```

### ✅ Question 38: Recursive CTE for hierarchy
```sql
WITH CategoryCTE AS (
    SELECT CategoryId, CategoryName, ParentId
    FROM Categories
    WHERE ParentId IS NULL
    UNION ALL
    SELECT c.CategoryId, c.CategoryName, c.ParentId
    FROM Categories c
    JOIN CategoryCTE ct ON c.ParentId = ct.CategoryId
)
SELECT * FROM CategoryCTE;
```

### ✅ Question 39: Apply row-level security via view
```sql
CREATE VIEW vw_OrdersLimited AS
SELECT * FROM Orders WHERE Region = SYSTEM_USER();
```

### ✅ Question 40: Use APPLY to get last order for each customer
```sql
SELECT c.CustomerId, c.Name, o.OrderId, o.OrderDate
FROM Customers c
CROSS APPLY (
  SELECT TOP 1 * FROM Orders o
  WHERE o.CustomerId = c.CustomerId
  ORDER BY o.OrderDate DESC
) o;
```

### ✅ Question 41: Use EXCEPT to find unmatched rows
```sql
SELECT ProductId FROM Products
EXCEPT
SELECT DISTINCT ProductId FROM OrderItems;
```

### ✅ Question 42: Common error – invalid GROUP BY
```sql
-- This fails:
-- SELECT CustomerId, OrderDate FROM Orders GROUP BY CustomerId;
-- FIX: All selected columns must be in GROUP BY or aggregates
SELECT CustomerId, MAX(OrderDate) FROM Orders GROUP BY CustomerId;
```

### ✅ Question 43: Create a filtered index
```sql
CREATE INDEX idx_ActiveProducts ON Products(ProductName)
WHERE IsActive = 1;
```

### ✅ Question 44: Use indexed view with SCHEMABINDING
```sql
CREATE VIEW vw_CategorySales
WITH SCHEMABINDING
AS
SELECT p.CategoryId, SUM(oi.Quantity) AS TotalQty
FROM dbo.Products p
JOIN dbo.OrderItems oi ON p.ProductId = oi.ProductId
GROUP BY p.CategoryId;
GO
CREATE UNIQUE CLUSTERED INDEX idx_CategorySales ON vw_CategorySales (CategoryId);
```

### ✅ Question 45: UNION vs UNION ALL
```sql
-- UNION removes duplicates
-- UNION ALL keeps all rows
SELECT City FROM Customers
UNION
SELECT City FROM Suppliers;
```

### ✅ Question 46: Use temporary table to summarize orders
```sql
SELECT CustomerId, SUM(TotalAmount) AS Revenue
INTO #OrderSummary
FROM Orders
GROUP BY CustomerId;
SELECT * FROM #OrderSummary;
```

### ✅ Question 47: Detect blocking session
```sql
SELECT blocking_session_id, session_id
FROM sys.dm_exec_requests
WHERE blocking_session_id <> 0;
```

### ✅ Question 48: Monitor long-running queries
```sql
SELECT TOP 10 *
FROM sys.dm_exec_requests r
JOIN sys.dm_exec_sessions s ON r.session_id = s.session_id
ORDER BY r.total_elapsed_time DESC;
```

### ✅ Question 49: Get index usage stats
```sql
SELECT OBJECT_NAME(i.object_id), i.name, user_seeks, user_scans
FROM sys.dm_db_index_usage_stats s
JOIN sys.indexes i ON s.object_id = i.object_id AND s.index_id = i.index_id
WHERE database_id = DB_ID();
```

### ✅ Question 50: Use FOR XML to serialize product list
```sql
SELECT ProductId, ProductName
FROM Products
FOR XML PATH('Product'), ROOT('Products');
```

---

## 🧠 SQL Mastery Senior-Level Interview Challenge – 50 Questions & Deep Dive Answers

This project document contains 50 high-level SQL questions specifically crafted for senior SQL developers. These challenges are designed to test real-world expertise across optimization, internals, indexing, query tuning, dynamic SQL, ETL, security, and advanced analytics.

Each solution includes:
- Detailed commentary
- Optimization rationale
- Best practices

---

### ✅ Question 1: Identify and fix a non-SARGable query
```sql
-- Poor (Non-SARGable, prevents index usage)
SELECT * FROM Orders WHERE YEAR(OrderDate) = 2023;

-- Better (SARGable)
SELECT * FROM Orders
WHERE OrderDate >= '2023-01-01' AND OrderDate < '2024-01-01';
-- WHY: Preserves index seek by avoiding functions on column
```

### ✅ Question 2: Analyze execution plan and reduce key lookup
```sql
-- Query causing key lookup:
SELECT ProductName, Price FROM Products WHERE ProductId = 5;

-- FIX: Create covering index
CREATE NONCLUSTERED INDEX idx_Product_Cover ON Products(ProductId) INCLUDE (ProductName, Price);
-- WHY: Prevents extra bookmark lookup for non-key columns
```

### ✅ Question 3: Detect and resolve a deadlock
```sql
-- Use system_health extended event
SELECT XEvent.query('(event/data[@name="resource"])[1]') AS DeadlockInfo
FROM sys.fn_xe_file_target_read_file('system_health*.xel', NULL, NULL, NULL);
-- WHY: Captures and reveals deadlock XML graph
```

### ✅ Question 4: Design a partitioned table for 5 years of orders
```sql
-- Create partition function
CREATE PARTITION FUNCTION pf_OrderDateRange (DATE)
AS RANGE LEFT FOR VALUES ('2020-12-31','2021-12-31','2022-12-31','2023-12-31','2024-12-31');

-- WHY: Efficient I/O management by year ranges
```

### ✅ Question 5: Write a recursive CTE to calculate category depth
```sql
WITH CategoryTree AS (
    SELECT CategoryId, CategoryName, ParentId, 0 AS Level
    FROM Categories
    WHERE ParentId IS NULL
    UNION ALL
    SELECT c.CategoryId, c.CategoryName, c.ParentId, ct.Level + 1
    FROM Categories c
    JOIN CategoryTree ct ON c.ParentId = ct.CategoryId
)
SELECT * FROM CategoryTree ORDER BY Level;
-- WHY: Explores hierarchy levels
```

...

### ✅ Question 6: Resolve parameter sniffing
```sql
-- Use local variables to avoid sniffing
DECLARE @LocalCategoryId INT = 4;
SELECT * FROM Products WHERE CategoryId = @LocalCategoryId;
-- WHY: SQL Server won't reuse a poor plan cached from skewed parameter
```

### ✅ Question 7: Re-write RBAR logic into set-based query
```sql
-- Poor (RBAR - Row By Agonizing Row)
DECLARE cursor CURSOR FOR SELECT OrderId FROM Orders;
-- FIX: Set-based version
UPDATE Orders SET Status = 'Shipped' WHERE OrderDate < GETDATE();
-- WHY: Set-based operations scale better
```

### ✅ Question 8: Use window function to get moving average
```sql
SELECT OrderDate, SUM(TotalAmount) OVER (ORDER BY OrderDate ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS MovingAvg
FROM Orders;
-- WHY: Tracks 7-day revenue trends
```

### ✅ Question 9: Find most recent 3 orders per region
```sql
SELECT * FROM (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY Region ORDER BY OrderDate DESC) AS rn
  FROM Orders
) a WHERE rn <= 3;
-- WHY: Combines partition + filtering
```

### ✅ Question 10: Identify most CPU-intensive queries
```sql
SELECT TOP 10 qs.total_worker_time, qs.execution_count, qt.text
FROM sys.dm_exec_query_stats qs
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) qt
ORDER BY qs.total_worker_time DESC;
-- WHY: Spot expensive workloads
```

### ✅ Question 11: Dynamic pivot table for category sales
```sql
DECLARE @cols NVARCHAR(MAX);
SELECT @cols = STRING_AGG(QUOTENAME(CategoryName), ',')
FROM Categories;

DECLARE @sql NVARCHAR(MAX);
SET @sql = '
SELECT * FROM (
  SELECT c.CategoryName, o.OrderDate, oi.Quantity
  FROM OrderItems oi
  JOIN Products p ON oi.ProductId = p.ProductId
  JOIN Categories c ON p.CategoryId = c.CategoryId
  JOIN Orders o ON oi.OrderId = o.OrderId
) src
PIVOT (
  SUM(Quantity) FOR CategoryName IN (' + @cols + ')
) AS pvt;';
EXEC sp_executesql @sql;
-- WHY: Reusable category dashboard
```

### ✅ Question 12: Audit access to sensitive table
```sql
CREATE TRIGGER trg_LogSelect
ON Employees
AFTER SELECT
AS
BEGIN
    INSERT INTO AuditLog (ActionType, Username, ActionDate)
    VALUES ('SELECT', SYSTEM_USER, GETDATE());
END;
-- WHY: Regulatory logging
```

### ✅ Question 13: Design ETL staging table with change tracking
```sql
CREATE TABLE StagingOrders (
    OrderId INT,
    CustomerId INT,
    OrderDate DATETIME,
    HashValue AS CHECKSUM(*),
    Processed BIT DEFAULT 0
);
-- WHY: Detect changes efficiently
```

### ✅ Question 14: Explain fill factor in indexing
- **Fill Factor** determines how much space to leave on index pages.
- Lower fill factor (e.g., 80) reduces page splits during heavy inserts.
- Set during index creation: `WITH (FILLFACTOR = 80)`

### ✅ Question 15: Monitor blocked sessions
```sql
SELECT blocking_session_id, session_id, wait_type, wait_time, wait_resource
FROM sys.dm_exec_requests
WHERE blocking_session_id <> 0;
-- WHY: Investigate concurrency bottlenecks
```
...

### ✅ Question 16: Use indexed view for performance
```sql
CREATE VIEW vw_ProductRevenue
WITH SCHEMABINDING
AS
SELECT p.ProductId, SUM(oi.Quantity * oi.PriceAtPurchase) AS Revenue
FROM dbo.Products p
JOIN dbo.OrderItems oi ON p.ProductId = oi.ProductId
GROUP BY p.ProductId;
GO
CREATE UNIQUE CLUSTERED INDEX ix_ProductRevenue ON vw_ProductRevenue(ProductId);
-- WHY: Materializes complex aggregation
```

### ✅ Question 17: Compare temp table vs table variable for large joins
```sql
-- Temp tables are better for large rowsets due to statistics
-- Table variables skip auto-generated stats
-- Use temp tables for joins, filtering
```

### ✅ Question 18: Log slow queries with duration threshold
```sql
-- Use Extended Events or query store, but sample:
SELECT *
FROM sys.dm_exec_query_stats
WHERE total_elapsed_time / execution_count > 1000000; -- 1 second avg
```

### ✅ Question 19: Tune bulk insert performance
- Use `TABLOCK`, batch sizes, `ROWS_PER_BATCH`, and disable indexes during load
```sql
BULK INSERT Orders FROM 'orders.csv'
WITH (
    FIRSTROW = 2,
    FIELDTERMINATOR = ',',
    ROWTERMINATOR = '\n',
    TABLOCK,
    BATCHSIZE = 5000
);
```

### ✅ Question 20: Query to show index fragmentation
```sql
SELECT dbschemas.[name] AS 'Schema', dbtables.[name] AS 'Table',
       dbindexes.[name] AS 'Index', indexstats.avg_fragmentation_in_percent
FROM sys.dm_db_index_physical_stats (DB_ID(), NULL, NULL, NULL, 'LIMITED') indexstats
JOIN sys.tables dbtables ON dbtables.[object_id] = indexstats.[object_id]
JOIN sys.schemas dbschemas ON dbtables.[schema_id] = dbschemas.[schema_id]
JOIN sys.indexes AS dbindexes ON dbindexes.[object_id] = indexstats.[object_id]
                                AND indexstats.index_id = dbindexes.index_id;
```

### ✅ Question 21: Find skewed distribution using histograms
```sql
DBCC SHOW_STATISTICS ('Orders', idx_OrderDate);
-- WHY: Check histogram for distribution skew
```

### ✅ Question 22: Generate XML output for nested invoices
```sql
SELECT i.InvoiceId, i.Total,
       (
         SELECT ProductId, Quantity
         FROM InvoiceLines WHERE InvoiceId = i.InvoiceId
         FOR XML PATH('Item'), ROOT('Items'), TYPE
       ) AS ItemsXML
FROM Invoices i;
```

### ✅ Question 23: Implement row-level security using predicate function
```sql
CREATE FUNCTION fn_SecurityPredicate (@Region AS NVARCHAR(50))
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN SELECT 1 AS Result WHERE @Region = USER_NAME();
-- Apply with SECURITY POLICY
```

### ✅ Question 24: Cleanup orphaned foreign keys
```sql
SELECT o.OrderId
FROM Orders o
LEFT JOIN Customers c ON o.CustomerId = c.CustomerId
WHERE c.CustomerId IS NULL;
```

### ✅ Question 25: Use CTE to paginate with total count
```sql
WITH Paginated AS (
    SELECT *, ROW_NUMBER() OVER (ORDER BY OrderDate DESC) AS RowNum
    FROM Orders
)
SELECT * FROM Paginated WHERE RowNum BETWEEN 11 AND 20;
```

...








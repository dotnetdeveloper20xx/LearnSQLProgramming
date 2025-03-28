# LearnSQLProgramming

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


















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























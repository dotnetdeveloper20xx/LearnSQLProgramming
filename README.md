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



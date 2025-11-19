### Chapter 1: Building the Foundation (DDL)

You are a local shoe shop and have decided to go online. Before you can upload pictures of your shoes, you need to build the digital "containers" to hold the data.

This is **DDL (Data Definition Language)**.

```sql
CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    Name TEXT,
    Size INT,
    Price INT,
    StockCount INT
);
```

**The Situation:** You also realize that when people buy things, you need to save _their_ details, not just the product details. You need a customer list.

```sql
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    FullName TEXT,
    Email TEXT,
    Address TEXT
);
```


### Chapter 2: Stocking the Shelves (DML)

The tables are built, but they are empty. Your website is currently blank. You need to put your physical stock into the digital system.

**The Situation:** A shipment of Nike sneakers and Adidas running shoes arrives at your shop.

**The Action:** We put data into the tables. This is **DML (Data Manipulation Language)**.

```sql
INSERT INTO Products (ProductID, Name, Size, Price, StockCount)
VALUES (101, 'Nike Air', 10, 150, 20);

INSERT INTO Products (ProductID, Name, Size, Price, StockCount)
VALUES (102, 'Adidas Run', 9, 120, 15);
```


### Chapter 3: Opening the Doors (DQL)

Your website is live! A user named "Sarah" visits your homepage. She doesn't want to see everything; she specifically filters for "Nike" shoes.

**The Situation:** The website needs to look into your `Products` table and find only the items Sarah is looking for.

**The Action:** We query the data.

This is **DQL (Data Query Language)**.

```sql
SELECT Name, Price 
FROM Products 
WHERE Name LIKE 'Nike%';
```

_Result:_ The website displays "Nike Air - $150".


### Chapter 4: The First Sale (DML)

Sarah loves the shoes. She registers on your site and clicks "Buy Now."

**The Situation:** Two things must happen here.

1. We need to save Sarah's info through the website (HTML Forms).
2. We need to record her specific Order so she can track the status and progress.

First, we create an `Orders` table (DDL) because we forgot to do that earlier:

```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    ProductID INT,
    Status TEXT
);
```

Now, we process Sarah's action (DML):

```sql
-- 1. Add Sarah to Customers
INSERT INTO Customers (CustomerID, FullName, Email, Address)
VALUES (1, 'Sarah Jones', 'sarah@email.com', '123 Maple St');

-- 2. Create the Order
INSERT INTO Orders (OrderID, CustomerID, ProductID, Status)
VALUES (500, 1, 101, 'Processing');
```


### Chapter 5: Inventory Management (DML)

The order is placed. However, your digital inventory still says you have 20 pairs of Nike Airs. But Sarah just bought one! If you don't change this, you might sell a pair you don't have.

**The Situation:** You need to update the stock count to reflect the sale.

**The Action:** We update existing records.

```sql
UPDATE Products
SET StockCount = 19
WHERE ProductID = 101;
```


### Chapter 6: Handling Returns (DML)

A week later, Sarah emails you. The shoes are too tight. She cancels the order and returns the item.

**The Situation:** You don't want to delete the order history (for accounting), but you need to change the status so you know not to ship it (or that it was returned).

```sql
UPDATE Orders
SET Status = 'Cancelled'
WHERE OrderID = 500;
```


### Chapter 7: Hiring a Manager (DCL)

Business is booming. You are too busy packing boxes to manage the database. You hire a manager, Bob, to update prices and check stock. However, you don't want Bob to be able to _delete_ your entire product list or see customer credit card info.

**The Situation:** You need to give Bob permission to view and update products, but nothing else.

**The Action:** We control access. This is **DCL (Data Control Language)**.

```sql
-- Give Bob the keys
GRANT SELECT, UPDATE ON Products TO Bob;

-- Ensure Bob cannot delete tables
REVOKE DELETE ON Products FROM Bob;
```


### Chapter 8: Renovating the Shop (DDL)

After a year, you realize you need to track the "Color" of your shoes. When you built the `Products` table in Chapter 1, you didn't think of that.

**The Situation:** You need to change the structure of your table without losing all the data inside it.

**The Action:** We alter the table definition.

```sql
ALTER TABLE Products
ADD Color TEXT;
```

Now, you can go back and update your inventory:

```sql
UPDATE Products
SET Color = 'White'
WHERE ProductID = 101;
```

By following this story, you have effectively built, managed, secured, and upgraded a database.

# 📝 Assignment: Database Design and Normalization

## 🎯 **Learning Objectives**
* 🛠️ **Understand the principles of good database design** and **normalization**.
* 💡 **Apply normalization techniques** to improve database structure and efficiency.
* 🔍 **Learn First, Second, and Third Normal Forms** (1NF, 2NF, 3NF) to eliminate redundancy and optimize data storage.

---

## 📋 **What You'll Need**
* 💻 A computer with internet access.
* ✍️ A code editor (e.g., Visual Studio Code).
* 🖥️ MySQL Workbench or another SQL database environment.

---


## 📝 Submission Instructions  
📂 Write all your SQL queries in the **answers.sql** file.  
✍️ Answer each question concisely and make sure your queries are clear and correct.  
🗣️ Structure your responses clearly, and use comments if necessary to explain your approach.

--- 

## 📚 Assignment Questions

---

### Question 1 Achieving 1NF (First Normal Form) 🛠️
Task:
- You are given the following table **ProductDetail**:

| OrderID | CustomerName  | Products                        |
|---------|---------------|---------------------------------|
| 101     | John Doe      | Laptop, Mouse                   |
| 102     | Jane Smith    | Tablet, Keyboard, Mouse         |
| 103     | Emily Clark   | Phone                           |


- In the table above, the **Products column** contains multiple values, which violates **1NF**.
- **Write an SQL query** to transform this table into **1NF**, ensuring that each row represents a single product for an order

sql
CREATE TABLE ProductDetail_1NF AS
SELECT
    OrderID,
    CustomerName,
    Product
FROM
    ProductDetail
UNPIVOT (Product FOR ProductIndex IN (Laptop, Mouse, Tablet, Keyboard, Phone)) AS unpvt;
```

Explanation:

1.  CREATE TABLE ProductDetail\_1NF AS: This creates a new table named `ProductDetail_1NF` to store the result.
2.  SELECT OrderID, CustomerName, Product:  We select the columns we want in the new table.
3.  FROM ProductDetail: We get the data from the original table.
4.  UNPIVOT: The UNPIVOT operator transforms multiple columns into rows.

(1NF): The SQL query provided above transforms the table into 1NF.


### Question 2 Achieving 2NF (Second Normal Form) 🧩

- You are given the following table **OrderDetails**, which is already in **1NF** but still contains partial dependencies:

| OrderID | CustomerName  | Product      | Quantity |
|---------|---------------|--------------|----------|
| 101     | John Doe      | Laptop       | 2        |
| 101     | John Doe      | Mouse        | 1        |
| 102     | Jane Smith    | Tablet       | 3        |
| 102     | Jane Smith    | Keyboard     | 1        |
| 102     | Jane Smith    | Mouse        | 2        |
| 103     | Emily Clark   | Phone        | 1        |

- In the table above, the **CustomerName** column depends on **OrderID** (a partial dependency), which violates **2NF**. 

- Write an SQL query to transform this table into **2NF** by removing partial dependencies. Ensure that each non-key column fully depends on the entire primary key.

sql
-- Create the Customers table
CREATE TABLE Customers (
    OrderID INT PRIMARY KEY,
    CustomerName VARCHAR(255)
);

-- Populate the Customers table
INSERT INTO Customers (OrderID, CustomerName)
SELECT DISTINCT OrderID, CustomerName
FROM OrderDetails;

-- Create the OrderProducts table (this will be our new OrderDetails table in 2NF)
CREATE TABLE OrderProducts (
    OrderID INT,
    Product VARCHAR(255),
    Quantity INT,
    PRIMARY KEY (OrderID, Product),
    FOREIGN KEY (OrderID) REFERENCES Customers(OrderID)
);

-- Populate the OrderProducts table
INSERT INTO OrderProducts (OrderID, Product, Quantity)
SELECT OrderID, Product, Quantity
FROM OrderDetails;

-- Drop the original OrderDetails table (optional, but recommended)
DROP TABLE OrderDetails;
```

Explanation:

1.  Customers Table:
    *   We create a `Customers` table to store the `OrderID` and `CustomerName`.
    *   `OrderID` is the primary key.
2.  OrderProducts Table:
    *   We create an `OrderProducts` table to store the `OrderID`, `Product`, and `Quantity`.
    *   The primary key is a composite key of `OrderID` and `Product`.
    *   We add a foreign key to link back to the `Customers` table.
3.  Populating Tables:
    *   We populate the `Customers` table with distinct `OrderID` and `CustomerName` values.
    *   We populate the `OrderProducts` table with the order details.
4.  Dropping the original OrderDetails table:
    *   We drop the original table, after creating the 2NF tables.
(2NF): The SQL queries provided above transform the table into 2NF.
Good luck 🚀

**Basic SELECT Solutions**

1. Write a solution to find the ids of products that are both low fat and recyclable.

| Column Name | Type    |
|-------------|---------|
| product_id  | int     |
| low_fats    | enum    |
| recyclable  | enum    |

product_id is the primary key (column with unique values) for this table.
low_fats is an ENUM (category) of type ('Y', 'N') where 'Y' means this product is low fat and 'N' means it is not.
recyclable is an ENUM (category) of types ('Y', 'N') where 'Y' means this product is recyclable and 'N' means it is not.
 

Soultion:
```sql
SELECT product_id
FROM Products
WHERE low_fats = 'Y' AND recyclable = 'Y';
```

2. Find the names of the customer that are not referred by the customer with id = 2. Return the result table in any order.

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |
| referee_id  | int     |

In SQL, id is the primary key column for this table.
Each row of this table indicates the id of a customer, their name, and the id of the customer who referred them.
 
Soultion:
```sql
SELECT name
FROM Customer
WHERE referee_id != 2 OR referee_id IS NULL;
```

**JOIN Solutions**

1. Write a solution to show the unique ID of each user, If a user does not have a unique ID replace just show null.
Return the result table in any order.

Table: Employees

| Column Name   | Type    |
|---------------|---------|
| id            | int     |
| name          | varchar |

id is the primary key (column with unique values) for this table.
Each row of this table contains the id and the name of an employee in a company.
 

Table: EmployeeUNI

| Column Name   | Type    |
|---------------|---------|
| id            | int     |
| unique_id     | int     |

(id, unique_id) is the primary key (combination of columns with unique values) for this table.
Each row of this table contains the id and the corresponding unique id of an employee in the company.

Solution:
```sql
SELECT 
    e.name,
    eui.unique_id
FROM 
    Employees e
LEFT JOIN 
    EmployeeUNI eui
ON 
    e.id = eui.id;
```

2. Write a solution to report the product_name, year, and price for each sale_id in the Sales table.
Return the resulting table in any order.

Table: Sales

| Column Name | Type  |
|-------------|-------|
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |

(sale_id, year) is the primary key (combination of columns with unique values) of this table.
product_id is a foreign key (reference column) to Product table.
Each row of this table shows a sale on the product product_id in a certain year.
Note that the price is per unit.
 

Table: Product

| Column Name  | Type    |
|--------------|---------|
| product_id   | int     |
| product_name | varchar |

product_id is the primary key (column with unique values) of this table.
Each row of this table indicates the product name of each product.
 
Solution:
```sql
SELECT 
    p.product_name, 
    s.year, 
    s.price
FROM 
    Sales s
JOIN 
    Product p
ON 
    s.product_id = p.product_id;
```

3. Write a solution to find the IDs of the users who visited without making any transactions and the number of times they made these types of visits. Return the result table sorted in any order.  

Table: Visits

| Column Name | Type |
|-------------|------|
| visit_id    | int  |
| customer_id | int  |

visit_id is the column with unique values for this table.
This table contains information about the customers who visited the mall.
 

Table: Transactions

| Column Name    | Type    |
|----------------|---------|
| transaction_id | int     |
| visit_id       | int     |
| amount         | int     |

transaction_id is column with unique values for this table.
This table contains information about the transactions made during the visit_id.
 
Solution:
```sql
SELECT 
    v.customer_id, 
    COUNT(*) AS count_no_trans
FROM 
    Visits v
LEFT JOIN 
    Transactions t
ON 
    v.visit_id = t.visit_id
WHERE 
    t.transaction_id IS NULL
GROUP BY 
    v.customer_id;
```

4. Write a solution to find all dates' id with higher temperatures compared to its previous dates (yesterday).
Return the result table in any order.

Table: Weather

| Column Name   | Type    |
|---------------|---------|
| id            | int     |
| recordDate    | date    |
| temperature   | int     |

id is the column with unique values for this table.
There are no different rows with the same recordDate.
This table contains information about the temperature on a certain day.
 
Solution:
```sql
SELECT 
    w1.id
FROM 
    Weather w1
JOIN 
    Weather w2
ON 
    w1.recordDate = DATE_ADD(w2.recordDate, INTERVAL 1 DAY)
WHERE 
    w1.temperature > w2.temperature;
```
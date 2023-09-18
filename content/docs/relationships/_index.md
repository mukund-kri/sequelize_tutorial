---
weight: 40
title: Relationships
bookFlatSection: true
---

# Relationships

{{< chaptertoc >}}

## Relationship / Association in SQL primer

SQL relations, also known as database relationships, are used to connect two or more tables in a relational database. This allows us to efficiently store and retrieve data, and to enforce data integrity.

There are three main types of SQL relations:

1. **One-to-one:** This type of relationship occurs when a row in one table can be associated with only one row in another table, and vice versa. For example, a Customers table might have a one-to-one relationship with a CustomerAddresses table. Each customer can have only one address, and each address can belong to only one customer.

2. **One-to-many:** This type of relationship occurs when a row in one table can be associated with multiple rows in another table, but each row in the second table can be associated with only one row in the first table. For example, a Customers table might have a one-to-many relationship with an Orders table. Each customer can place multiple orders, but each order can belong to only one customer.

3. **Many-to-many:** This type of relationship occurs when a row in one table can be associated with multiple rows in another table, and vice versa. For example, a Products table might have a many-to-many relationship with a Categories table. A product can belong to multiple categories, and a category can contain multiple products.

SQL relations are implemented using foreign keys. A foreign key is a column in one table that references the primary key of another table. For example, the Orders table might have a foreign key column called customer_id that references the primary key column of the Customers table. This indicates that each order must belong to a valid customer.

SQL relations can be used to query data from multiple tables. For example, we could use a JOIN query to select all of the orders placed by customers who live in California. SQL relations also allow us to enforce data integrity constraints. For example, we could create a constraint that prevents us from deleting a customer if they have any outstanding orders.

SQL relations are a powerful tool for modeling and managing data in a relational database. By understanding the different types of relations and how to implement them, we can create database schemas that are efficient, flexible, and reliable.
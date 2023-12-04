---
weight: 51
title: Transactions
---

# Transactions


A transaction in a SQL database is a sequence of one or more database operations that are treated as a single unit. Transactions are important for ensuring data integrity in a database, especially in a multi-user environment where multiple users may be accessing and modifying data at the same time.

## Properties of transactions

All transactions in SQL databases must adhere to the ACID properties:

* **Atomicity**: Each transaction must be either completely successful or completely unsuccessful. There is no middle ground.
* **Consistency**: Transactions must maintain the consistency of the database. This means that transactions cannot violate any of the database's constraints or integrity rules.
* **Isolation**: Transactions must be isolated from each other. This means that one transaction cannot see the uncommitted changes of another transaction.
* **Durability**: Once a transaction is committed, it must be durable and cannot be undone.

## Transaction lifecycle

The lifecycle of a transaction consists of the following steps:

* **Start**: The transaction begins.
* **Execute statements**: The transaction executes one or more SQL statements.
* **Commit**: If the transaction is successful, it is committed and the changes made by the statements are made permanent.
* **Rollback**: If the transaction is unsuccessful, it is rolled back and all of the changes made by the statements are undone.

## Transactions in Sequelize

Since Sequelize is an ORM, it provides an abstraction over the database and the transactions are no different. Sequelize provides a `transaction` method on the `sequelize` object which returns a promise. The promise is resolved with a transaction object which can be used to perform operations on the database.

The following methods correspond to the lifecycle of a transaction:

* `transaction.transaction()`: Starts a transaction.

The methdos to commit or rollback a transaction work inside of a try-catch block. 

* `transaction.commit()`: Commits a transaction. Usually called in the finally block of a try-catch block. This means the transaction was successful.
* `transaction.rollback()`: Rolls back a transaction. Usually called in the catch block of a try-catch block. This means the transaction failed.

## Structure of a transaction in Sequelize

```js
// Start a transaction
const transaction = await sequelize.transaction();

try {
    // Sequelize operations go here.

    // If all is well, commit the transaction.
    await transaction.commit();
} catch (err) {

    // if there are any errors, rollback the transaction
    // all db operations the try block are undone
    await transaction.rollback();
}
```

## Full Example

The complete code for this example is here. Please read the code and keep it open in another tab while you read this section.

{{<fullcode "https://github.com/mukund-kri/sequelize_tutorial_code/blob/relationships/08_one_to_many.js">}}

We provide two examples. 

The first one is the function `transcationFail` which demonstrates a failed transaction. 

Note ...
1. on line 51 we start a transaction with `await sequelize.transaction()`
2. any sequelize operation in the try block is assumed to be part of the transaction.
3. on line 70 we commit the transaction with `await transaction.commit()`. But this
code is never reached because of the error on line 62.
4. on line 76 in the catch block we rollback the transaction with `await transaction.rollback()`.


The second one is the function `transcationSuccess` which demonstrates a successful transaction.

Note ...
1. on line 93 we start a transaction with `await sequelize.transaction()`.
2. any sequelize operation in the try block is assumed to be part of the transaction.
3. on line 112 we commit the transaction with `await transaction.commit()`.
4. on line 117 in the catch block we rollback the transaction with `await transaction.rollback()`. But this code is never reached because there is no error in the try block.

{{< pagebottomnav >}}

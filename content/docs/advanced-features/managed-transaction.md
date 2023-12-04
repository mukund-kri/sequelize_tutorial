---
weight: 52
title: Managed Transaction
---

# Managed Transaction

Manged transactions are a way to avoid code duplication with a couple of simple conventions. Which are ...

1. If the try block is successful, the transaction is committed.
2. If the try block fails, the transaction is rolled back.

That's it. This elemintes the need to write call the `transaction.commit()` and `transaction.rollback()` methods.


## Structure of a managed transaction in Sequelize

```js
    sequelize.transaction(async (transaction) => {
        // Sequelize operations go here.

        // NOTE: No need to commit or rollback the transaction.
    })
    .then(() => {
        // We can chain other operations here.
    })
    .catch((err) => {
        // Handle errors here.
    });
```


## Full Example

The complete code for this example is here. Please read the code and keep it open in another tab while you read this section.
        
{{<github-ref "" >}}

**Notes:**
1. The code that forms *transaction* is now a callback function that is passed to the `sequelize.transaction()` method.
2. If this function is successful, the transaction is committed. This is automatic.
3. If the function throws an exception, the transaction is rolled back. This is also automatic.
4. A promise is returned by the `sequelize.transaction()` method. Which we can use to chain other operations.

{{< pagebottomnav >}}
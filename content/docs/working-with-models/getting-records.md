---
weight: 5
title: Getting Records
---

# Getting records

Querying aka `SELECT` forms the the heart of `SQL`, and is a huge topic, which we have
a whole chapter on. Here we're just going to look at the shape of the API.

## `SELECT *` with sequelize

The simplest query is `SELECT * FROM table`. In sequelize, this is done with the `findAll`
method.

```javascript
Question.findAll()
    .then(questions => console.log(questions))
    .catch(err => console.log('Error:', err));
```

The `findAll` method returns a promise. If the promise is resolved, it returns an array
of all the questions in the database. If the promise is rejected, it returns an error.

The generated SQL for the above operation is ...

```bash
Executing (default): SELECT `id`, `title`, `body`, `answer`, `createdAt`, `updatedAt` FROM `Questions` AS `Question`;
```

**Wait a minute!** The SQL generated has the fields `id`, `createdAt` and `updatedAt` but
are not defined in the model. What's going on?

Sequelize automatically adds the fields `id`, `createdAt` and `updatedAt` to all models.

`id` :: obviously is the auto generated primary key of the table.
`createdAt` :: is the timestamp when the row was created.
`updatedAt` :: is the timestamp when the row was last updated.

By default, sequelize adds these fields to all models. We can turn this off; but that's
advanced stuff and we'll ignore for now.

**Notes:**

- All the models have a comprehensive API that is equivalent to all the operations
we can do in standard SQL.
- The `findAll` method is equivalent to `SELECT * FROM ...`.
- As with all the API `findAll` method returns a promise which makes the code asynchronous.




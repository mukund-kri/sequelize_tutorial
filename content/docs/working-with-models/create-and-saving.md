---
weight: 4
title: Create and Save
---

# Create and Save rows to the database

Now that we have a model, let's look at creating and saving data to the database. This
is completely done in javascript, which is the `O` in `ORM`. All operations happen
on javascript objects and sequelize takes care of translating these operations to
SQL and executing them on the database.

## Inserting a Row

How to insert a row into the database? Let's look at the code first ...

```javascript

let question = Question.build({
    title: 'Capital of France',
    body: 'What is the capital of France?',
    answer: 'Paris'
});
// At this point, the question is not saved to the database yet.

// Save the question to the database
await question.save()
```

The first part of the code creates a new `Question` object. This object is not saved
to the database yet. It is just a javascript object. The second part of the code
saves the object to the database. The `save()` method returns a promise. If the
promise is resolved, the question is saved successfully. If the promise is rejected,
the question is not saved and the error is logged.

Sequelize, when run in development mode (which is default) will log the SQL generated
to the console. For the above operation it should look like ...

```bash
Executing (default): INSERT INTO `Questions` (`id`,`title`,`body`,`answer`,`createdAt`,`updatedAt`) VALUES (NULL,$1,$2,$3,$4,$5);
```

We can see that the `save` method generated an `INSERT` statement and executed it.

## `create` method

The `create` method is far more convinent then the `build` and `save` method. It
combines the two steps into one. It creates a new object and saves it to the database.

```javascript
Question.create({
    title: 'Capital of France',
    body: 'What is the capital of France?',
    answer: 'Paris'
})
.then(question => console.log('Question saved successfully'))
.catch(err => console.log('Error saving question:', err));
```

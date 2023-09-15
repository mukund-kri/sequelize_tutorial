---
weight: 32
title: Finders
---

# Finders

We have seen the `findAll`  method in the previous chapter, which is the most used of
the so called `finders` methods. In this chapter we will see the other `finders` methods.

## `findByPk`

A very common use case, at least in web applications, is to find a record by its primary
key. This is where the `findByPk` method comes in handy. For example:

```js
let questionById = await Question.findByPk(1);
if (questionById) {
    console.log('Question with id 1:', JSON.stringify(questionById, null, 4));
} else {
    console.log('Question with id 1 not found');
}
```

The equivalent SQL would be:

```sql
SELECT * FROM Questions WHERE id = 1;
```

** Notes: **
- The `findByPk` method takes the primary key value as an argument.
- The `findByPk` method returns `null` if the record is not found.


## `findOne`

The `findOne` method is used to find a single record from the database. While the 
`findByPk` is specifically used to find a record by its primary key, called id. The
`findOne` method supports the full where clause syntax. For example:

```js
let questionByTitle = await Question.findOne({
    where: {
        title: 'Capital of France'
    }
});
```

The equivalent SQL would be:

```sql
SELECT * FROM Questions WHERE title = 'Capital of France';
```

** Notes: **
- The `query object` is passed as an argument to the `findOne` method. This is the same
as the `findAll` method.
- If a matching record is found, it is returned as an object.
- If no matching record is found, `null` is returned same as the `findByPk` method.


## `findOrCreate`

The `findOrCreate` method is used to find a record by the specified `where` clause. If
a matching record is found, it is returned. If no matching record is found, a new record
is created with the values specified in the `defaults` property of the `query object`.

```js
let [question, created] = await Question.findOrCreate({
    where: {
        title: 'Capital of France'
    },
    defaults: {
        body: 'What is the capital of France?',
        answer: 'Paris'
    }
});

if (created) {
    console.log('New question created:', JSON.stringify(question, null, 4));
} else {
    console.log('Question already exists:', JSON.stringify(question, null, 4));
}
```

Here two SQL queries are executed. First the `SELECT` query to find a record by the
specified `where` clause. If a matching record is found, it is returned. If no matching
record is found, a new record is created with the values specified in the `defaults`
using the `INSERT` query.

```sql
SELECT * FROM Questions WHERE title = 'Capital of France' LIMIT 1;
INSERT INTO Questions (title, body, answer) VALUES ('Capital of France', 'What is the capital of France?', 'Paris');
```

** Notes: **
- The return value of the `findOrCreate` method is an array of two elements. The first
element is the record found or created. The second element is a boolean value indicating
whether the record was created or not.

## Conclusion

There is one more `finder` method called `findAndCountAll`. Look that up in the
documentation. We will not be using it in this tutorial.

We covered the `finders` methods in this chapter. In the next chapter we will look at
ordering, grouping and SQL functions.

{{< pagebottomnav >}}

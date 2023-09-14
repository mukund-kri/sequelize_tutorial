---
weight: 31
title: Query Basics
---
# Querying Basics

In this section we'll dive a little deeper into querying your database using Sequelize.
The `findAll` method is the most common way to query your database. In this section 
we'll explore how to implement the `where clause` and SQL operator equivalents in
Sequelize.

Note: in sequelize, the so called "Query Language" is specified using Objects properties
(specifically in the form of key, value pairs), and not a separate language like SQL. 

So the first property of this `query language` we are going to look at is the `where` 
property.

## The `where` property

### Introduction to the `where` property
Simple use of the `where` property:

```js
let questionById = await Question.findAll({
    where: {
        id: 1
    }
});
```

The equivalent SQL would be:

```sql
SELECT * FROM Questions WHERE id = 1;
```

** Notes: **
- The `query object` is passed as an argument to the `findAll` method.
- The `where` property is an also an object.
- The `where` property is used to specify the `where clause` of the SQL query.
- In the above case the properties of the `where` object are the column names of the 
table and the values is the value to match in SQL.

### Multiple conditions in the `where` property

```js
let questionsWithMultipleWhere = await Question.findAll({
    where: {
        title: 'Capital of France',
        answer: 'Paris'
    }
});
```

The equivalent SQL would be:

```sql
SELECT * FROM Questions WHERE title = 'Capital of France' AND answer = 'Paris';
```

** Notes: **
- Mutliple properties in the `where` object are treated as `AND` conditions in SQL.


### Using SQL operators in the `where` property

We can also use SQL operators in the `where` property. For example:

```js
let questionsWithAnd = await Question.findAll({
    where: {
        [Sequelize.Op.and]: [
            { title: 'Capital of France' },
            { answer: 'Paris' }
        ]
    }
});
```

which exactly same as the previous query.

Sequelize provides all the operators that are available in standard SQL, such as the
logical operators `AND`, `OR`, `NOT`, etc., and others like `LIKE`, `IN`, `BETWEEN`, etc.
Refer to the documentation for a complete list of operators.

### The `like` operator

Let's do one more example using the `like` operator before we move to the next chapter.

```js
let questionsWithLike = await Question.findAll({
    where: {
        title: {
            [Sequelize.Op.like]: '%France%'
        }
    }
});
```

The equivalent SQL would be:

```sql
SELECT * FROM Questions WHERE title LIKE '%France%';
```

{{< pagebottomnav >}}

---
weight: 33
title: Ordering, Grouping and SQL functions
---

# Ordering, Grouping and SQL Functions

## Ordering

The `order` property of the query object is used to replicate the `order by` clause of
SQL in sequelize. The following example shows how to use the `order` property:

```js
let allQuestionsOrdered = await Question.findAll({
    order: [
        ['answer', 'ASC']
    ]
});
```

The equivalent SQL would be:

```sql
SELECT * FROM Questions ORDER BY answer ASC;
```

** Notes: **
- The `order` property is an array of arrays.
- The inner array contains two elements. The first element is the column name and the
second element is the sort order.

As with SQL we can also specify multiple columns to sort by. For example:

```js
let allQuestionsOrdered = await Question.findAll({
    order: [
        ['answer', 'ASC'],
        ['title', 'DESC'],
        [Sequelize.literal('LENGTH(body)'), 'ASC']
    ]
});
```

** Notes: **
- As with SQL there is no limit to the number of columns we can sort by.
- We can also use SQL functions in the `order` property. For example, in the above
example we are sorting by the length of the `body` column.
- We will cover associations in the next section. For now, just note that we can also
sort by columns in associated tables. 

## SQL Functions

Here explore how to use SQL functions like `COUNT`, `SUM`, `AVG`, which are the most
commonly used SQL functions from sequelize.

### `COUNT`

The `count` method is used to count the number of records in a table. For example:

```js
let questionCount = await Question.count();
```

This counts all the records in the `Questions` table. The equivalent SQL would be:

```sql
SELECT COUNT(*) FROM Questions;
```

Obviously, count also supports the query object especially the `where` property. 
For example:

Get the number of questions in the database that have the word 'capital' in the title

```js
let questionCountWithWhere = await Question.count({
    where: {
        title: {
            [Sequelize.Op.like]: '%capital%'
        }
    }
});
```

The equivalent SQL would be:

```sql
SELECT COUNT(*) FROM Questions WHERE title LIKE '%capital%';
```

This lets you do more complex things like finding the the question with the longest 
body. 

```js
let questionWithLongestBody = await Question.findOne({
    order: [
        [sequelize.fn('length', sequelize.col('body')), 'DESC']
    ]
});
```

## Grouping

An finally let's look at how to `GROUP BY` in sequelize. The `group` property of the
query object is used to replicate the `GROUP BY` clause of SQL in sequelize. 

The following example is a little contrived but it shows how to use the `group` property.
Here we group by the `answer` column and count the number of questions for each answer.

```js
let questionCountByAnswer = await Question.findAll({
    attributes: ['answer', [sequelize.fn('COUNT', sequelize.col('answer')), 'count']],
    group: ['answer']
});
```

The equivalent SQL would be:

```sql
SELECT answer, COUNT(answer) AS count FROM Questions GROUP BY answer;
```


{{< pagebottomnav >}}

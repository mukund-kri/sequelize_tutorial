---
weight: 43
title: Many to Many
---

# Many to Many relationships

{{<fullcode "" >}}

In `Many to Many` relationships, a row in table A can have many matching rows in table B, and a row in table B can have many matching rows in table A. For example, a question can have multiple tags, and a tag can belong to multiple questions.

In the case of one-to-many relationships, we put the foreign key in the many side, ie. the table the `belongs` to the other table.

But in the case of many-to-many relationships, A `belongs` to B and also B `belongs` to A. Now where do we put the foreign key? 

The answer is that we don't put the foreign key in either table. Instead we create a third table, called a `join table` or an `association table` which has foreign keys to both the tables.


## Many-to-Many in Sequelize

Before we go ahead, like in one-to-many relationships, sequelize takes care of creating 
the `primary key` and `foreign key` columns for us. In this case, sequelize will also create the `join table` for us.

## API

```js

// Define the Question and Tag models.
const Question = sequelize.define('questions', {/* attributes */}, {});
const Tag = sequelize.define('tags', {/* attributes */}, {});

// Define the relationship between Question and Tag.
// A question has many tags. A tag has many questions -- Many to Many.
Question.belongsToMany(Tag, { through: 'QuestionTag' });
Tag.belongsToMany(Question, { through: 'QuestionTag' });

```

When we do a `sequelize.sync()` sequelize will create the two table, the association table and the foreign keys.

Which should look something like this:

```sql
```
{{< pagebottomnav >}}

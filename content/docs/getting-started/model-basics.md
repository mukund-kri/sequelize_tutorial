---
weight: 4
title: Model Basics
---

# Model Basics

Now that we have connected to the database, let's create a model and see how the 
model is mapped to a table in the database.

## What is a model?

A model is a class that represents a table in the database. 

ORM is defined as Object Relational Mapping. 

Map between Objects and Relational wolds ...

| Object World | Relational World |
| ------------ | ---------------- |
| Class        | Table            |
| Object       | Row              |
| Property     | Column           |

Model is another name for the Class that maps to a database table.

## Creating a model

We'll create a model called `Question`. `Question` has 3 properties, title, body and 
answer. 


```javascript
const Question = sequelize.define('Question', {
    title: {
        type: Sequelize.STRING,
        allowNull: false
    },
    body: {
        type: Sequelize.TEXT,
        allowNull: true
    },
    answer: {
        type: Sequelize.STRING,
        allowNull: false
    },
}, {
    // Other model options go here. We have none at the moment
});
```

Notes:
1. `id` the primary key is absent!! Sequelize takes responsibility for creating the
'id` primary key.
2. the properties also have additional options which, describe the column properties. 
like type, nullability etc.
3. The second argument to the `define` method is an object which contains additional
model options. We'll see more of this later.

## Model -> Table mapping

Now we have a model, let's see how to convert into a table on the Relational side.

```javascript

sequelize.sync({ force: true })
    .then(() => {
        console.log(`Database & tables created!`);
    });
```

Will construct the table in the database. 

```bash
> node 02_model_basics.js
```

If we run this code, sequelize spits out the SQL it used to create table. We see 
something like this ...

```bash
Executing (default): DROP TABLE IF EXISTS `Questions`;
Executing (default): CREATE TABLE IF NOT EXISTS `Questions` (`id` INTEGER PRIMARY KEY AUTOINCREMENT, `title` VARCHAR(255) NOT NULL, `body` TEXT, `answer` VARCHAR(255) NOT NULL, `createdAt` DATETIME NOT NULL, `updatedAt` DATETIME NOT NULL);
Executing (default): PRAGMA INDEX_LIST(`Questions`)
Table created successfully

```

And if we login to sqlite3 with the command line tool, we can see the newly created table.

```bash
> sqlite3 database.sqlite
sqlite> .tables
Questions
sqlite> .schema Questions
CREATE TABLE IF NOT EXISTS `Questions` (`id` INTEGER PRIMARY KEY AUTOINCREMENT, `title` VARCHAR(255) NOT NULL, `body` TEXT, `answer` VARCHAR(255) NOT NULL, `createdAt` DATETIME NOT NULL, `updatedAt` DATETIME NOT NULL);
```

The full code is available [here](https://github.com/mukund-kri/sequelize_tutorial_code/blob/model-basics/02_model_basics.js)


## Next

Now that we have a model and a table, let's see how to insert, delete and get
data from the table.
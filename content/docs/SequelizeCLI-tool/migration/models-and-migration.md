---
weight: 64
title: Models and Migration
---

# Models and Migration

### In this section we will ...

1. Use the Sequelize CLI to create a model and a migration for that model
2. Modify the generated model to suit our needs
3. Modify the generated migration to suit our needs

## Step 1: Create a model and a migration for that model

Previously we coded our models by hand. Following the convention of Sequelize projects we covered the model files are put in the `models` directory. The code for the models are also different from the code we wrote by hand.

Things that are different:
1. The whole project uses commonjs modules. I have'nt found a generator that generates es6 modules. So, let's stick with commonjs for now. 
2. Each model has it's own file in the `models` directory. The file name is the singular form of the model name. For example, the model `User` is in the file `user.js`.
3. Loading the models is done by the `index.js` file in the `models` directory. It takes care of loading all the models and exporting them.

For more information on the differences between commonjs and es6 modules see [this article](https://medium.com/computed-comparisons/commonjs-vs-amd-vs-requirejs-vs-es6-modules-2e814b114a0b).

OK, enough talk. Let's create a model and a migration for that model.

```bash
> npx sequelize-cli model:generate --name Question --attributes title:string,body:text,answer:string
```

This should create ...
1. a model file in the `models` directory called `question.js`. The model should have the attributes `title`, `body` and `answer`, with minimal configuration.

2. a migration file in the `migrations` directory called `<timestamp>-create-question.js`. The migration should have the attributes `title`, `body` and `answer`, with minimal configuration.

### Improve the generated model and migration

The generated model is fairly minimal. We need to add a lot of code, for constraints, associations, validations, etc. 

In our case it just the NULL constraints on the fields of our `Question` model. Finally it should look like this:

```javascript
'use strict';
const {
  Model
} = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class Question extends Model {

    static associate(models) {
      // define association here
    }
  }
  Question.init({
    title: {
      type: DataTypes.STRING,
      allowNull: false,
    },
    body: {
      type: DataTypes.TEXT,
      allowNull: true,
    },
    answer: {
      type: DataTypes.STRING,
      allowNull: true,
    },
  }, {
    sequelize,
    modelName: 'Question',
  });
  return Question;
};
```

No on to the migration file. The sole purpose of migration files is to track changes to the database schema. It has two methods: `up` and `down`. The `up` method is used to apply the migration. The `down` method is used to undo the migration.

As with the model file the generated migration is fairly minimal. We need to add all the constraints that were added to the model file to the migration file also. Finally it should look like this:

```javascript
'use strict';
/** @type {import('sequelize-cli').Migration} */
module.exports = {
  async up(queryInterface, Sequelize) {
    await queryInterface.createTable('Questions', {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER
      },
      title: {
        type: Sequelize.STRING,
        allowNull: false,
      },
      body: {
        type: Sequelize.TEXT,
        allowNull: false,
      },
      answer: {
        type: Sequelize.STRING,
        allowNull: false,
      },
      createdAt: {
        allowNull: false,
        type: Sequelize.DATE
      },
      updatedAt: {
        allowNull: false,
        type: Sequelize.DATE
      }
    });
  },
  async down(queryInterface, Sequelize) {
    await queryInterface.dropTable('Questions');
  }
};
```

**Notes:**
1. The field in the migration file mirror the fields in the model file.
2. Extra fields `id`, `createdAt` and `updatedAt` are added to the migration file. These fields are usually added by Sequelize automatically. But since the table creation duty is delegated to the migration file we need to add them manually.
3. We see that the migration file has two functions: `up` and `down`. The `up` function is used to apply the migration. The `down` function is used to undo the migration. We'll see how to do that in the next chapter.

Right! we're ready to run the migration, ie. use the migration file to create the table in the database.

{{< pagebottomnav >}}
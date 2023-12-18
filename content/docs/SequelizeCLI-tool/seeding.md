---
weight: 70
title: Seeding Data
---

# Seeding Data

It's a good idea to have some seed data in the database when you are developing an 
application. This way when you `run` your application you can **see** what it outputs.
Especially the UI.

Also for testing you absolutely need test data, hence you need to seed the database.

In Sequelize, the seeding mechanism is provided by the `sequelize-cli` tool. It is 
very similar to the migration mechanism. We have a number of `seed` files in the
`seeders` directory. Each file contains a `up` and a `down` function. The `up` function.
The `up` function is used to apply the seed data and the `down` function is used to
un-apply the seed data.

So let's add some seed data to our `question` table.

## Generating seed data for `Question` model

First we need to generate a seed file for the `Question` model. It similar to generating
a migration file.

```bash
npx sequelize-cli seed:generate --name demo-questions
```

This will generate a file `<date stamp>-demo-questions.js` in the `seeders` directory. 
In the `up` function we will add the seed data and in the `down` function we will remove
the seed data.

```js
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.bulkInsert('Questions', [
      {
        title: 'What is your favourite programming language?',
        answer: 'JavaScript',
        createdAt: new Date(),
        updatedAt: new Date()
      },

    ], {});
  },

  down: async (queryInterface, Sequelize) => {
    await queryInterface.bulkDelete('Questions', null, {});
  }
};
```

OK, the code snippet add a single question to the `Questions` table on the `up` function
and removes it on the `down` function.

Ideally, we should have a lot of questions in the seed data. The code with much more
seed data is on [GitHub](https://example.com).

## Applying and Un-applying seed data

Applying and Un-applying seed data is similar to applying and un-applying migrations.


```bash
npx sequelize-cli db:seed:all
npx sequelize-cli db:seed:undo:all
```

### Checking if the seed data

I have written a simple script to list all the questions in the DB 
[`check-seed-data.js`](https://example.com). The way we can check if the seed data is applied or not.

```bash
node check-seed-data.js
```

{{< pagebottomnav >}}

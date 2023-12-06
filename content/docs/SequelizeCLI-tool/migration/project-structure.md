---
weight: 62
title: Project Structure
---

# Migration from scratch

In this section we'll learn how to create a project from scratch using `sequelize-cli`. We'll also learn how to create migrations, run them and undo them.

## Project Structure

What do you mean by project structure?

Well, if `sequelize-cli` is to keep track of migrations (and also seeders) then it needs to know where to look for them. It also needs to know where the models are. And, for seeding it needs to where the seeders are. By convention, `sequelize-cli` expects the following structure:

```
.
├── config
│   └── config.json
├── migrations
├── models
└── seeders
```

**Notes:**

It's pretty obvious what each directory contains, but let's go over them anyway:

* The `config` directory contains the `config.json` file which contains the database configuration.
* The `migrations` directory contains the migration files.
* The `models` directory contains the model files.
* The `seeders` directory contains the seeder files.


## Project generation

Don't worry, you don't have to create this structure manually. `sequelize-cli` can do it for you. Just run the following command:

```bash
$ npx sequelize-cli init
```

This should generate a project with empty migration, model and seeder directories.

I the next few sections we'll learn how to connect to the database, create models, create migrations, run migrations and undo migrations.

{{< pagebottomnav >}}
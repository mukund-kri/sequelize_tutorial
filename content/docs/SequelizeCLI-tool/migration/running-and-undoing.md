---
weight: 65
title: Running and Undoing Migrations
---

# Running and Undoing Migrations

Here let's actually run the migration (create/modify tables in the DB) and undo the migration (the exact opposite of running the migration).

We'll do this in three steps:
1. run the migration
2. undo the migration
3. re-applying the migration again

## Step 1: Run the migration

The command to run the migration is:

```bash
> npx sequelize-cli db:migrate
```

This should create the table in the database. If there were no errors the command should output something like this:

```bash
Sequelize CLI [Node: 20.6.0, CLI: 6.6.2, ORM: 6.35.1]

Loaded configuration file "config/config.json".
Using environment "development".
== 20231207031450-create-question: migrating =======
== 20231207031450-create-question: migrated (0.021s)
```

At this point log into sqlite and check if the table was created.

```bash
> sqlite3 db.development.sqlite
```

Once logged in use the `.schema` command of sqlite to see the schema of newly created table. It should look like this:

```sql
CREATE TABLE `Questions`(
  `id` INTEGER PRIMARY KEY AUTOINCREMENT,
  `title` VARCHAR(255) NOT NULL,
  `body` TEXT,
  `answer` VARCHAR(255) NOT NULL,
  `createdAt` DATETIME NOT NULL,
  `updatedAt` DATETIME NOT NULL
);
```

**Notes:**

We see that the migration file has two functions: `up` and `down`. 

The `up` function is used to apply the migration. Basically `sequelize-cli` keeps track of all the migration that have been applied (run). When we run the `db:migrate` command `sequelize-cli` checks which migrations have not been applied and calls the `up` function of those migrations.


The `down` function is used to undo the migration. We'll see how to do that in the next step.

## Step 2: Undo the migration

The command to undo the migration is:

```bash
> npx sequelize-cli db:migrate:undo
```

This should undo the migration. If there were no errors the command should output something like this:

```bash
Sequelize CLI [Node: 20.6.0, CLI: 6.6.2, ORM: 6.35.1]

Using environment"development".
== 20231207031450-create-question: reverting =======
== 20231207031450-create-question: reverted (0.21s)
```

At this point log into sqlite and check if the table was deleted.


## Step 3: Rinse and repeat

We can run the migration again by running the command:

```bash
> npx sequelize-cli db:migrate
```

We do this any number of times. The migration will be applied only once. If the migration has already been applied it will be skipped.

{{< pagebottomnav >}}
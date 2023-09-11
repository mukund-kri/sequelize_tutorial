---
weight: 3
title: Connect to db
---

# Connecting to a Sqlite database

Now that we have installed all the software we need, let's connect to a database. We're
not going work with data yet, we'll just connect to the database and see how it works.

Ok first lets deal with imports ...

```javascript
const { Sequelize } = require('sequelize');
```

Next we create a sequelize object. This object is the main entry point to the sequelize
library. It is used to configure the library and to connect to the database.

```javascript
const sequelize = new Sequelize({
    dialect: 'sqlite',
    storage: 'db.sqlite'
});
```

Here, we pass 2 options to the Sequelize constructor ...

- `dialect` is the type of database we are connecting to, in this case `sqlite``.
- `storage` DB file name. This is specific to sqlite. Other databases have different
  options like host, port, username, password etc.

```javascript
sequelize.authenticate()
    .then(() => {
        console.log('Connection has been established successfully.');
    })
    .catch(err => {
        console.error('Unable to connect to the database:', err);
    });
```

Now that we have a sequelize object, we can connect to the database. The following
code just connects to database and logs in. If successful, it logs a success message
and if not, it logs an error message.

The full code is available [here](https://github.com/mukund-kri/sequelize_tutorial_code/blob/getting-started/01_getting_started.js).

Next up we'll create a model and see how to create tables in the database, and how to
insert data into the tables.
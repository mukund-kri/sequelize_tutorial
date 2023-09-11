---
name: Installation
weight: 2
---

# Installing Sequelize

Before we proceed, let's check if have the prerequisites installed.

First make sure you are in a node project. If not, create one.

```bash
> mkdir sequelize-tutorial
> cd sequelize-tutorial
> npm init -y
```

### NodeJS and npm.

```bash
> node --version
> npm --version
```
Install if not installed. If you are having trouble installing NodeJS, go talk to your
mentor.

### sqlite3 development library
The node js sqlite3 library is a wrapper around c sqlite3 driver. Make sure it is 
installed

```bash
> npm install libsqlite3-dev
```

### Sequelize and dependencies
Sequelize does not directly talk to sqlite3. That is done by a `driver`, we are
going to use the sqlite database so we need the sqlite driver. 

```bash
> npm install sqlite3
```

Finally install sequelize itself.

```bash
> npm install sequelize sqlite3
```


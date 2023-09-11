# Sequelize Tutorial

## Background

This tutorial is generated from my notes which I use in my fullstack bootcamp. This
tutorial is built for the bootcamp, but I'm putting it out in open; if it helps
anyone, that's great.

In this tutorial, we'll be using `Sequelize` to interact with `sqlite` database. Sqlite
is a lightweight database which is really great for development as it is not a
full-fledged database server. It's just a file which you can copy around and use 
it anywhere. It's great for development, but don't use it in production. I recommend
`PostgreSQL` for production.

## Prerequisites

- Ubuntu :: Our Bootcamp is Unbutu only, so the examples/command are for Ubuntu. You 
    can use any other OS, but you'll have to figure out the commands yourself.
- NodeJS :: NodeJS version 14 or above.
- Javascript :: Decent knowledge of Javascript.
- sqlite :: sqlite3 command line tool. You can install it using `sudo apt install sqlite3`
- sql :: Basic knowledge of SQL.

## Sqlite Skills

We'll login to sqlite3 command line tool and explore how data is stored on the
database side of things. So we will be using the sqlite3 command line quite a bit.

So you should know the basics of sqlite3 command line tool. There are lot of 
resources on youtube and internet. [This one](https://www.youtube.com/watch?v=dFzJ4UPNL1w)
is adequate.

## What is Sequelize?

Sequelize is an open-source Object Relational Mapping (ORM) library for Node.js. It 
allows you to interact with databases in a more object-oriented way.

ORMs are a way to abstract the underlying database from your code, Which means you
write database related code in Javascript not in SQL. The advantages of this are many,
the mental load of programming in two languages is eliminated, debugging is easier,
its database agnostic ie. we can switch databases without changing our code etc.

Sequelize supports a wide range of databases, including MySQL, PostgreSQL, SQLite, and 
Microsoft SQL Server.

There are **MANY** good ORMs for NodeJS. I expect you to use one of them in your
work. We choose Sequelize because it is the most popular one and it is well maintained.
The concepts / skills you learn in Sequelize will transfer easily to other ORMs.


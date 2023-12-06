---
weight: 61
title: Migration
---

# Migration

## What is Migration? Theory

In the previous chapter we saw how to create models and associate them with each other. But what if we want to change the structure of our database? For example, what if we want to add a new column to an existing table? Or what if we want to remove a column from a table? Or what if we want to rename a column? Or what if we want to change the data type of a column?

This problem of schema changes are compounded when working in teams. Getting schema changes from one developer to another safely (not overwriting each other's changes) is also a big problem.

This is where **Migrations** come in. Migrations are a way to make schema changes to the database in a safe and repeatable way. Migrations are also a way to share schema changes with other developers. They work by creating a file that contains the schema changes. This file can then be tracked in git thus solving versioning and collaboration problems.

In this section we'll see how to do migrations in Sequelize.

{{< chaptertoc >}} 

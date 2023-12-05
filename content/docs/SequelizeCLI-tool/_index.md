---
weight: 60
title: Sequelize CLI
bookFlatSection: true
---

# Sequelize CLI

Apart form the Sequelize library, Sequelize also ships a CLI (Command Line Interface) tool. This tool can be used to perform various operations on your Sequelize project.

A few things `sequelize-cli` can do:

- Initialize a project
- Create skeleton files for models, migrations, seeders etc.
- Migrations (This the BIG one, we'll cover this in detail)
- Seeding (Basically, populating your database with dummy data for development/testing purposes)

## Installation

You have to install `sequelize-cli` separately.

```bash
npm install --save-dev sequelize-cli
```

## Usage

In this chapter we'll the two big uses of `sequelize-cli`, namely **Migrations** and **Seeding**.

{{< chaptertoc >}}

---
weight: 24
title: Deleting Records
---

# Deleting Records

Deleting a record is as simple as calling the `destroy` method on the model instance. 

Given a model instance `question`:

```js
question.destroy();
```

Will remove the corresponding row from the database.

The generated SQL will look something like this:

```sql
Executing (default): DELETE FROM `Questions` WHERE `id` = 33
```

{{< pagebottomnav >}}

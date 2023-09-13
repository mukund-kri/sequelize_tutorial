---
weight: 23
title: Updating Records
---

# Updating Records

The API for updating records are similar to the API for creating records. There are two
ways to update records. 

### The `save` API
Let's look at the code first ...


```javascript
... other code 
// assume question is already defined and saved to the database

// Let's update
question.answer = 'Paris is the capital of France';

// Save the changes
await question.save();

... other code
```

As with `build` and `save`, it is a two step process. First we update the object on
the javascript side. Then we use the `save` method to save the changes to the database.

The generated SQL should look something like ...

```bash
Executing (default): UPDATE `Questions` SET `answer`=$1,`updatedAt`=$2 WHERE `id` = $3
```

### The `update` API

{{< pagebottomnav >}}

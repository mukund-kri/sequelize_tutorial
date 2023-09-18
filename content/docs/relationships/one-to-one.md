---
weight: 41
title: One to One
---

# One to One relationships

## Relationship API

All 3 types of in Sequelize are done with the following 4 API calls on the model:

- `hasOne`
- `belongsTo`
- `hasMany`
- `belongsToMany`

The API calls are all very similar, so we will only cover `hasOne` and `belongsTo` in this section. The other two will be covered in the next section.

## One to One

A one to one relationship is when a row in table A can have at most one matching row in table B, and vice versa. For example, a user might have exactly one profile. The foreign key is defined on the target model (Profile).

```js

const User = sequelize.define('user', {/* attributes */});
const Profile = sequelize.define('profile', {/* attributes */});

User.hasOne(Profile);
Profile.belongsTo(User);

```

That it, sequelize will add the foreign key to the target model.

Let's look at the generated schema:

```sql
CREATE TABLE IF NOT EXISTS `users` (
  `id` INTEGER NOT NULL auto_increment , 
  ... Other fields
  `createdAt` DATETIME NOT NULL, 
  `updatedAt` DATETIME NOT NULL, 
  PRIMARY KEY (`id`)
);

CREATE TABLE IF NOT EXISTS `profiles` (
  `id` INTEGER NOT NULL auto_increment , 
  `userId` INTEGER, 
  ... Other fields
  `createdAt` DATETIME NOT NULL, 
  `updatedAt` DATETIME NOT NULL, 
  PRIMARY KEY (`id`), 
  FOREIGN KEY (`userId`) REFERENCES `users` (`id`) ON DELETE SET NULL ON UPDATE CASCADE
);
```

You see the relation is defined by adding a `userId` column to the `profiles` table
and defining a foreign key constraint.

## Instances

Now that we have defined the relation, let's look at how to create instances.

```js

const user = await User.create({ /* ... */ });
const profile = await Profile.create({ /* ... */ });

// set the association
await user.setProfile(profile);
user.save(); // save the user, so that it gets the profile's id as foreign key
```

The call to `user.save()` triggers 2 insert statements. One for the user and one for the profile. And finally an update statement to set the foreign key on the profile.

### Querying

```js



{{< pagebottomnav >}}

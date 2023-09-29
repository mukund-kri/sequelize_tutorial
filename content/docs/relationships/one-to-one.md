---
weight: 41
title: One to One
---

# One to One relationships

{{<fullcode "https://github.com/mukund-kri/sequelize_tutorial_code/blob/relationships/07_one_to_one.js">}}

## Relationship API

All 3 types of relationships in Sequelize are defined with the following 4 API calls on the model:

- `hasOne`
- `belongsTo`
- `hasMany`
- `belongsToMany`

The API calls are all very similar, but, we need only `hasOne` and `belongsTo` for one to one relationships. We'll look at the other 2 in the next sections.

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

Now that we have created some instances, let's look at how to query them, especially 
how to include the associated instance. By default, sequelize will not include the 
associated model instance. You have to use the automatic `get` method. Let's look at
some code so that this becomes clear.

{{<highlight javascript "linenos=true">}}
    // Get a user
    let user = await User.findOne({
        where: {
            name: 'John Doe'
        }
    });
    console.log('User profile:', JSON.stringify(user, null, 4));
    // Note the profile is not retrieved by default. We need to explicitly ask for it.

    // Get profile
    let profile = await user.getProfile();
    console.log('User profile:', JSON.stringify(profile, null, 4));
{{</highlight>}}

Running this code outputs something like this:

{{<highlight javascript "linenos=true">}}
User profile: {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe#example.com",
    "createdAt": "2023-09-18T07:56:40.205Z",
    "updatedAt": "2023-09-18T07:56:40.205Z"
}
User profile: {
    "id": 1,
    "bio": "I am a new user",
    "createdAt": "2023-09-18T07:56:40.222Z",
    "updatedAt": "2023-09-18T07:56:40.232Z",
    "UserId": 1
}
{{</highlight>}}

As you can see there 2 separate API calls here. 
1. `User.findOne` to get the user. But this does not include the profile.
2. `user.getProfile` to get the profile. This is an automatic method that sequelize adds to the model instance. 

#### Eager Loading

If you want to get the profile along with the user, in one shot you need to use the 
`include` property of the query object. For example:

{{<highlight javascript "linenos=true">}}
    // Get a user
    let user = await User.findOne({
        where: {
            name: 'John Doe'
        },
        include: [Profile]
    });
    console.log('User + profile:', JSON.stringify(user, null, 4));
{{</highlight>}}

Running this code outputs like this:

{{<highlight javascript "linenos=true">}}
User + profile: {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe#example.com",
    "createdAt": "2023-09-18T07:56:40.205Z",
    "updatedAt": "2023-09-18T07:56:40.205Z",
    "Profile": {
        "id": 1,
        "bio": "I am a new user",
        "createdAt": "2023-09-18T07:56:40.222Z",
        "updatedAt": "2023-09-18T07:56:40.232Z",
        "UserId": 1
    }
}
{{</highlight>}}

As you can see, the profile is included in the user object returned by the `findOne` call.


{{< pagebottomnav >}}

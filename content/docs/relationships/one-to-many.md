---
weight: 42
title: One to Many 
---

# One to Many relationships

{{<fullcode "https://github.com/mukund-kri/sequelize_tutorial_code/blob/relationships/08_one_to_many.js">}}

In `One to Many` relationships, a row in table A can have many matching rows in table B, but a row in table B can have at most one matching row in table A. For example, a question can have multiple options, but a option can only belong to one user.

The following example is a little contrived, but it illustrates the point. 

We define two models, `Question` and `Option` and define a one to many relationship between them.

{{<highlight javascript "linenos=true">}}
// Define the Question model.
const Question = sequelize.define('questions', {/* attributes */}, {});

// Define the Option model.
const Option = sequelize.define('options', {/* attributes */}, {});

// Define the relationship between Question and Option. 
// A question has many options. An option belongs to a question. 
// Hence, we say that the relationship is one-to-many.
Question.hasMany(Option);
Option.belongsTo(Question);
{{</highlight>}}

The only difference between this and the one-to-one relationship is that we use `hasMany` and `belongsTo` instead of `hasOne` and `belongsTo`. And, on the Object side a Question
will have field `options` which will be an array of `Option` objects.

Let's look at the generated schema:

{{<highlight sql "linenos=true">}}
CREATE TABLE IF NOT EXISTS `questions` (
  `id` INTEGER NOT NULL auto_increment , 
  ... Other fields
  `createdAt` DATETIME NOT NULL, 
  `updatedAt` DATETIME NOT NULL, 
  PRIMARY KEY (`id`)
);

CREATE TABLE IF NOT EXISTS `options` (
  `id` INTEGER NOT NULL auto_increment , 
  `questionId` INTEGER, 
  ... Other fields
  `createdAt` DATETIME NOT NULL, 
  `updatedAt` DATETIME NOT NULL, 
  PRIMARY KEY (`id`), 
  FOREIGN KEY (`questionId`) REFERENCES `questions` (`id`) ON DELETE SET NULL ON UPDATE CASCADE
);
{{</highlight>}}

This is exactly the same as the one-to-one relationship at the schema level of the 
database and you as the developer have to remember that the relationship is one-to-one
or one-to-many. 

Sequelize on the other hand, has different APIs for one-to-one and one-to-many relationships.

## Inserting data

As with the one-to-one relationship, we can insert data in the same way as we did before,
by creating a `question` and then creating a bunch of `options` and using the `set` API to
associate the options with the question.

{{<highlight javascript "linenos=true">}}
// Create a question
let question = await Question.create({
    title: 'Capital of France',
    body: 'What is the capital of France?',
    answer: 'Paris'
});

// Create some options
let options = await Option.bulkCreate([{
    title: 'London',
    correct: false
}, {
    title: 'Paris',
    correct: true
}, {
    title: 'New York',
    correct: false
}, {
    title: 'San Francisco',
    correct: false
}]);

// Associate the options with the question
await question.setOptions(options);
{{</highlight>}}

## Retrieving data

Again, we can retrieve data in two ways:
1. Lazy loading :: Here we only retrieve the question and then use the `get` API to retrieve the associated options.
2. Eager loading :: Here we retrieve the question and the associated options in one go.

### Lazy loading

{{<highlight javascript "linenos=true">}}
// Retrieve the question
{{</highlight>}}

### Eager loading

{{<highlight javascript "linenos=true">}}
// Retrieve the question and the associated options
{{</highlight>}}

### Getting the `one` from the `many`

As with any relationship, there are two sides. In the previous section, we looked at 
how to get the `many` from the `one` ie. get  the `options` form the `question`. In this
section, we will look at how to get the `one` from the `many`.

{{<highlight javascript "linenos=true">}}
// Retrieve the options and the associated question
{{</highlight>}}


{{< pagebottomnav >}}

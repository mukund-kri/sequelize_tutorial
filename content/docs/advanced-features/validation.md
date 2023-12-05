---
weight: 53
title: Validation
---

# Validation

The process of checking the data being written to the database, against a set of rules is called validation. Sequelize provides a way to validate the data before it is written to the database.

## Validation vs. Constraints

Constraints are rules that are defined on the database level. They exactly same as
the ones in SQL. For example, the `NOT NULL` constraint ensures that a column cannot be null, the `UNIQUE` constraint ensures that a column cannot have duplicate values, etc.

Validation on the other hand is done in JavaScript and is done before the data is written to the database. Sequelize provides this functionality.

## Validation in Sequelize

Validation can be done in two ways:
* At the field level. This is done by passing a `validate` object to the field definition.
* At the model level. This is done by passing a `validate` object to the model definition.

### Field level validation

Field level validation is done by passing a `validate` object to the field definition. The `validate` object is an object with keys as the validation rules and values as the options for the validation rules.

```js
const User = sequelize.define('user', {
    name: {
        type: DataTypes.STRING,
        validate: {
            len: {
                args: [3, 10],
                msg: 'Name must be between 3 and 10 characters long.'
            },
            isAlpha: {
                msg: 'Name must only contain letters.'
            }
        }
    }
});
```

**Notes:**
* The `validate` object is an object with keys as the validation rules and values as the options for the validation rules.
* The `len` validation rule checks the length of the string. It takes an array of two numbers as the argument. The first number is the minimum length and the second number is the maximum length.
* The `isAlpha` validation rule checks if the string contains only alphabets.

Sequelize comes with a lot of pre-build validators. You can find the list of all the validators [here](https://sequelize.org/master/manual/validations-and-constraints.html#per-attribute-validations).

### Custom validators
We can also do validations not provided by sequelize. For example, we can check if the name is a palindrome (I know, this will never have a use-case in production, but bear with me). We can do this by passing a function to the `validate` object.

```js
const User = sequelize.define('user', {
    name: {
        type: DataTypes.STRING,
        validate: {
            isPalindrome(value) {
                if (value !== value.split('').reverse().join('')) {
                    throw new Error('Name must be a palindrome.');
                }
            }
        }
    }
});
```
**Notes:**

* In this case the `validator` is a method. This method takes one parameter, the value of the field being validated.
* The method body is the validation logic. If the validation fails, we throw an error.

### Model level validation

Sometimes we need to validate across multiple fields. For example, we might want to check if the `password` and `confirmPassword` fields are the same. We can't do this with field
level validation because we can only access the value of the field being validated. I this
case we need to use model level validation.

In the following example, let's do just that, check if the `password` and `confirmPassword` fields are the same on a `User` model.

Note: DON'T DO THIS!. Don't store confirm password, You will be duplicating data. This is just a demonstration of model level validation.


```js
const User = sequelize.define('user', {
    password: {
        type: DataTypes.STRING,
        validate: {
            len: {
                args: [8, 20],
                msg: 'Password must be between 8 and 20 characters long.'
            }
        }
    },
    confirmPassword: {
        type: DataTypes.STRING,
        validate: {
            len: {
                args: [8, 20],
                msg: 'Password must be between 8 and 20 characters long.'
            },
            isConfirmed(value) {
                if (value !== this.password) {
                    throw new Error('Password does not match.');
                }
            }
        }
    }
}, {
    validate: {
        bothPasswordsMustBeSet() {
            if ((this.password === undefined) !== (this.confirmPassword === undefined)) {
                throw new Error('Both password fields must be set.');
            }
        }
    }
});
```

**Notes:**

* The `validation` is done by a method called `bothPasswordsMustBeSet()`. 
* the `this` inside is bound to the model instance, and hence, have access all the fields of the model instance.


{{< pagebottomnav >}}

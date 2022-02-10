
![](/images/ajv-0.png)

A "helper function" is a function you write because you need that particular functionality in multiple places and because it makes the code more readable. A good example is an average function. You'd write a function named avg or similar, that takes in a list of numbers, and returns the average value from that list.

## Create A helper Function To Create A Validator

There are three places we expected users to send data within it, **request body**, **request headers**, and **request query params**.

We will use the ajv scheme validator to create a helper function to create the schema validator for the three places.

Create a new file at the helper folder called `create-validator.js` and write the following code on it:

```js
const _ = require('lodash');
const SchemaValidator = require('./schema-validator');

/**
 *
 * @param {Object} schemas
 * @param {Object} schemas.bodySchema
 * @param {Object} schemas.querySchema
 * @param {Object} schemas.paramsSchema
 * @description this function for create ajv validators
 */

const createValidators = (schemas = {}) => {
  const { bodySchema = null, querySchema = null, paramsSchema = null } = schemas;

  let bodyValidator = null;
  let queryValidator = null;
  let paramsValidator = null;

  if (!_.isNil(bodySchema)) {
    bodyValidator = new SchemaValidator(bodySchema);
  }

  if (!_.isNil(querySchema)) {
    queryValidator = new SchemaValidator(querySchema);
  }

  if (!_.isNil(paramsSchema)) {
    paramsValidator = new SchemaValidator(paramsSchema);
  }

  return {
    bodyValidator,
    queryValidator,
    paramsValidator,
  };
};

module.exports = createValidators;
```

### What is lodash ?

> Lodash is a JavaScript library that provides utility functions for common programming tasks using the functional programming paradigm. [More](https://lodash.com/)

Right now we created the helper function let's use it in the next lessons.


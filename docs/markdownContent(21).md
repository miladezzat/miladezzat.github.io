
![](/images/ajv-1.png)

> **Remember That:** Middleware is a function that has access to the request object, response object, and the next function which points to the next middleware. Since the request and response object is available, middleware is able to change these objects, run any code, or even end the request/response cycle.

## Create AJV Validation Middleware

After we create a helper function to create a validation validator, let us create a middleware that used this validator to apply it to our APIs.

Create a `validation.js` file on the middleware folder

```js
const _ = require('lodash');
const { Errors: { BadRequestError } } = require('error-handler-e2');

const validation = (validators = {}) => async (req, res, next) => {
  try {
    const { bodyValidator, queryValidator, paramsValidator } = validators;

    if (!_.isNil(bodyValidator)) {
      const body = await bodyValidator.validate(req.body);
      req.body = body;
    }

    if (!_.isNil(queryValidator)) {
      const query = await queryValidator.validate(req.query);
      req.query = query;
    }

    if (!_.isNil(paramsValidator)) {
      const params = await paramsValidator.validate(req.params);
      req.params = params;
    }

    next();
  } catch (error) {
    throw new BadRequestError('Data Validation Error', { error });
  }
};

module.exports = validation;

```

**Congratulation** you create everything you need to apply the validation layer on the APIs, we will use it in the next lessons.


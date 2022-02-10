![](/images/install-set-ajv.png)

## What is AJV?

The fastest JSON validator for Node.js and browser.

>AJV for Another JSON Schema Validator is a javascript library that is used to validate a data object against a structure defined using a JSON Schema. It comes with a command-line feature that allows you to easily test your validation schema with some fake data.

## Install AJV

We will use the [AJV npm package](https://www.npmjs.com/package/ajv) on our project, to install it using this command `npm i ajv`.

Also, we will need to install some packages to help us with adding custom messages, adding keywords, and adding formats [AJV Formats](https://www.npmjs.com/package/ajv-formats), [AJV Keywords](https://www.npmjs.com/package/ajv-keywords) and [AJV Errors](https://www.npmjs.com/package/ajv-errors/v/1.0.1)

To install these packages `npm i ajv-formats ajv-keywords ajv-errors`

### Validation Layer Structure

We will create the file on the `config` folder called `ajv.config.js` and `ajv-schema-validator.js` at the `helper` folder.

- Open the  `ajv.config.js` file and write the following code on it.

```js
const addFormats = require('ajv-formats');
const addKeywords = require('ajv-keywords');
const addCustomErrorMessage = require('ajv-errors');

const config = {
  // To return all catch errors
  allErrors: true,
  // Use defaults provided in the schema if the field is not provided
  useDefaults: true,
  $data: true,
  // To remove any fields that are passed in by the user that are not in the schema
  removeAdditional: 'all',
  // All user inputs are strings by default, coercion is used to cast all fields to their appropriate type
  // Array value is used to coerce scalar values to single element in an array
  coerceTypes: 'array',
};

/**
 * @param {Ajv} ajv - the ajv instance
 */
const configureAjv = (ajv) => {
  addKeywords(ajv);
  addFormats(ajv);
  addCustomErrorMessage(ajv);

  //= ================================== Custom keywords ======================//
  // Add custom keyword validator which accepts a function to validate the provided data
  ajv.addKeyword({
    keyword: 'validator',
    errors: true,
    modifying: true,
    compile: ([validator, customErrorMessage]) => function validate(data) {
      const valid = validator(data);
      if (!valid) {
        validate.errors = [{
          keyword: 'validate',
          message: customErrorMessage,
          params: { keyword: 'validate' },
        }];
      }
      return valid;
    },
  });
};

module.exports = {
  configureAjv,
  config,
};
```

- Open the `schema-validator.js` file and write the following code on it.
```js
const Ajv = require('ajv').default;
const _ = require('lodash');
const { config, configureAjv } = require('../../config/ajv.config');

class SchemaValidator {
  constructor(schema) {
    if (_.isNil(schema) || _.isEmpty(schema)) {
      throw new Error('missing or invalid dependencies');
    }
    this.schemas = schema;
    this.ajvValidate = null;
  }

  validate(doc) {
    const validationResult = this._validate(doc);

    if (validationResult) {
      return Promise.resolve(doc);
    }

    const errorObject = new Error('invalid object');
    errorObject.details = this._formatErrors(this.ajvValidate.errors);

    return Promise.reject(errorObject);
  }

  _validate(doc) {
    if (_.isNil(this.ajvValidate)) {
      this._createValidatorInstance();
    }

    return this.ajvValidate(doc);
  }

  _createValidatorInstance() {
    const ajv = new Ajv(config);
    configureAjv(ajv);

    const lastSchema = _.last(this.schemas);
    const schemas = _.dropRight(this.schemas);

    if (!_.isEmpty(schemas)) {
      ajv.addSchema(schemas);
    }

    this.ajvValidate = ajv.compile(lastSchema);
  }

  _formatErrors(errorsArray) {
    return _.map(errorsArray, (anError) => _.omit(anError, 'schemaPath'));
  }
}

module.exports = SchemaValidator;
```

In the next lesson, we will create a helper function to create AJV validator.









![](/images/ajv-2.jpeg)

Ajv takes a schema for your JSON data and converts it into a very efficient JavaScript code that validates your data according to the schema. To create a schema you can use either JSON Schema or JSON Type Definition - check out Choosing schema language, they have different advantages and disadvantages.

## What is JSON Schema?

> JSON Schema is a vocabulary that allows you to annotate and validate JSON documents. [For More](https://json-schema.org/)

## Registration JSON Schema
Let us create the register JSON schema, open the user folder create a file called `user.validation.schema.js` and write the following code

```js
const createValidator = require('../helper/create-validator');

const registrationValidationSchema = {
  // this is the body json schema to validate the body request only
  bodySchema: [
    {

      $id: '/schemas/educative.io/users/create/body',

      title: 'Schema',

      type: 'object',

      additionalProperties: false,

      required: ['email', 'password'],

      properties: {
        firstName: {
          type: 'string',
          transform: ['trim'],
          minLength: 1,
          maxLength: 50,
        },

        lastName: {
          type: 'string',
          transform: ['trim'],
          minLength: 1,
          maxLength: 50,
        },

        email: {
          type: 'string',
          format: 'email',
          transform: ['trim', 'toLowerCase'],
        },

        password: {
          type: 'string',
          minLength: 8,
          maxLength: 50,
        },
      },
    },
  ],
};

module.exports = {
  registrationValidator: createValidator(registrationValidationSchema),
};
```

### Apply The validation Middleware On The Registration.

Open user route and add the following codes
```js
const userValidation = require('./user.validation.schema');
const validationMiddleware = require('../middleware/validation');

...

router.post('/register', validationMiddleware(userValidation.registrationValidator), async (req, res) => {
  const user = await userService.register(req.body);

  res.status(200).send({ user });
});
```

### Test validation Layer By Postman
1. Missing Data
![](/images/ajv-3.png)

2. User Already Exists
![](/images/ajv-4.png)

3. Invalid Data length
![](/images/ajv-5.png)

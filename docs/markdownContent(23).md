## Login JSON Schema
Open the `user.validation.schema.js` and set the following code

```js
const loginValidationSchema = {
  // this is the body json schema to validate the body request only
  bodySchema: [
    {

      $id: '/schemas/educative.io/users/login/body',

      title: 'Schema',

      type: 'object',

      additionalProperties: false,

      required: ['email', 'password'],

      properties: {

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
```

### Create Validator
At the end of the file like registration validator, Create one for login validator.

```js
 loginValidator: createValidator(loginValidationSchema),
```

### Apply The Validation Layer on The Login
Open the user route file and set the validation middleware on the `/login` route.

```js
router.post('/login', validationMiddleware(userValidation.loginValidator), async (req, res) => {
  const user = await userService.login(req.body);
  res.status(200).send({ user });
});
```

### Test login By Using Postman
1. Missing Data
![](/images/login-ajv-0.png)

2. Invalid Data

![](/images/login-ajv-1.png)

3. User does not exist

![](/images/login-ajv-2.png)

4. User password not correct.
![](/images/login-ajv-3.png)


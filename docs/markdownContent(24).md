Get user API, this API lets us get the users' information, we will create a validation layer for it in this lesson.

## Get User JSON Schema

At this API, users send query strings like: `limit`, `pagination`, `sortBy`, and `order`, so we need to create `paramsSchema`.

```js
const getUsersValidationSchema = {
  // this is the query string json schema to validate the query request only
  querySchema: [{
    $id: '/schemas/educative.io/users/login/body',
    type: 'object',

    properties: {
      page: {
        type: 'number',
        default: 1,
        minimum: 1,
      },

      limit: {
        type: 'number',
        minimum: 1,
        maximum: 100,
        default: 10,
      },

      sortBy: {
        type: 'string',
        default: 'createdAt',
      },

      order: {
        type: 'number',
        default: -1,
        enum: [-1, 1],
      },
    },
  }],
};
```

### Create Validator
Using create validator helper function, create a get user validator.

```js
getUsersValidator: createValidator(getUsersValidationSchema),
```
### Apply the Validation Schema On API

Open the user route and apply the validation middleware on the get users route.

```js
router.get('/', checkAuth, validationMiddleware(userValidation.getUsersValidator), async (req, res) => {
  const users = await userService.get(req.query);

  res.status(200).send({ users });
});
```
### Test Endpoint By Postman
1. Invalid Data
![](/images/get-user-json-schema-0.png)

2. Valid Data
![](/images/get-user-json-schema-1.png)
![](/images/me-get-users-apis.png)

## Me API Endpoint
Make a GET request to retrieve user information about the current user.

### Create Route
Open the `users.routes.js` file and add a new route `/me` with `get` method request.

```js
router.get('/me', async (req, res) => {
  res.status(200).send({ user: 'user' });
});
```

### Use Check Auth Middleware
If you remember, we set the user data on the request at the check auth middleware, we will require the `checkAuth` [middleware](https://expressjs.com/en/guide/using-middleware.html) at user routes and use it on the `/me` route.

```js
const checkAuth = require('../middleware/check-auth.middleware');
...
router.get('/me', checkAuth, async (req, res) => {
  res.status(200).send({ user: req.user });
});
```
### Test Me API
Now you can test the `/me` route on the postman.

> Note the check auth middleware need to send a **token** on the header

![](/images/me-get-users-apis-1.png)

## Get Users API Endpoint
Make a GET request to retrieve users' information.

![](/images/me-get-users-apis-2.png)

### Create Route

We will be creating an API to get users' information, that endpoint needs authentication, to get users' information.

```js
router.get('/', checkAuth, async (req, res) => {
  res.status(200).send({ users: 'users' });
});
```

> Remember that `checkAuth` is a middleware to check if the user is authenticated to get some data to need to be user authenticated, you can read more about [authentication vs authorization](https://www.okta.com/identity-101/authentication-vs-authorization/)

### Create Service
After we create the users' route let's create the users' service method to get users with pagination.

Open the `user.service.js` and write the following code
```js
/**
   * @param {Object} args
   * @param {Number} args.page
   * @param {Number} args.limit
   * @param {String} args.sortBy
   * @param {Number} args.order
   *
   * @return {Promise<Array<Object>>}
   */
  get(args) {
    const {
      page = 1,
      limit = 10,
      sortBy = 'createdAt',
      order = 'asc',
    } = args;

    return this.model.find({ isDeleted: false })
      .sort({ [sortBy]: order })
      .skip(+page * +limit - +limit)
      .limit(+limit)
      .lean();
  }
```
### Update Route & Use Check Auth Middleware

Right now you created the service, let us use the service at the route to get the users' information.

Open `user.route.js` and update this route '/' to be like:  

```js
router.get('/', checkAuth, async (req, res) => {
  const users = await userService.get(req.query);
  res.status(200).send({ users });
});
```
### Test Get Users API
let's make a request on `http://localhost:3000/users`, to get the users' information, this endpoint needs a token.

![](/images/me-get-users-apis-3.png)

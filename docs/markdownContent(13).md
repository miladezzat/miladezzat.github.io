![](/images/login-0.png)

## What is Login?

> In computer security, logging in is the process by which an individual gains access to a computer system by identifying and authenticating themselves. The user credentials are typically some form of a username and a matching password, and these credentials themselves are sometimes referred to as a login.

### Create The login API
On the `user.routes.js` create new route `/login`.

```js
router.post('/login', async (req, res) => {
  res.status(200).send({ user: 'user' });
});
```

### Create Login method Service
On the `user.service.js` create login method
```js
  /**
   *
   * @param {Object} args
   * @param {String} args.email
   * @param {String} args.password
   */
  async login(args) {
    const { email, password } = args;
    const user = await this.getByEmail(email);

    if (!user) {
      throw new Error('User not exists');
    }

    const passwordIsMatch = await compareHashedPassword(password, user.password);

    if (!passwordIsMatch) {
      throw new Error('Password not correct');
    }

    const token = createToken(_.omit(user, ['password']));

    return { ..._.omit(user, ['password']), token };
  }
```

### Update User Routes
```js
router.post('/login', async (req, res) => {
  const user = await userService.login(req.body);
  res.status(200).send({ user });
});
```

### Test login

Use postman to make a request on  `http://localhost:3000/users/login` 

![](/images/login-1.png)



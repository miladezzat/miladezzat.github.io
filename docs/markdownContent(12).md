
![](/images/registeration-0.png)

This API is used to create a new User and a User Registration in a single request. 

## What We Use To Create Registration API
1. [bcrypt package](https://www.npmjs.com/package/bcrypt), this for hashing passwords.
2. [jsonwebtoken package](https://www.npmjs.com/package/jsonwebtoken), this for An implementation of JSON Web Tokens, to authentication.

Install these packages on our project `npm i bcrypt jsonwebtoken`;

![](/images/registartion-1.png)

## Install body-parser package
> Node.js body parsing middleware. 
>Parse incoming request bodies in a middleware before your handlers, available under the req. body property.
> Note As req. body's shape is based on user-controlled input, all properties and values in this object are untrusted and should be validated before trusting. For example, req.body.foo.toString() may fail in multiple ways, for example, the foo property may not be there or may not be a string, and toString may not be a function and instead of a string or other user input. [Read More](https://www.npmjs.com/package/body-parser)

You can install it by `npm i body-parser`, after that you need to use it at app.js 
```js
const express = require('express');
const path = require('path');
const bodyParser = require('body-parser');
const uploadImagesRoutes = require('./upload-images/upload-image.routes');
const uploadFilesRoutes = require('./uploading-files/uploading-files.routes');

const app = express();

app.use(bodyParser.json({ limit: '50mb' }));
app.use(bodyParser.urlencoded({ limit: '50mb', extended: true, parameterLimit: 50000 }));
app.use(express.static(path.join(__dirname, '../uploads')));

app.use('/images', uploadImagesRoutes);
app.use('/files', uploadFilesRoutes);

module.exports = app;
```

### Create User Routes
Let's create user routes, open the user folder component and create new file `user.routes.js`
![](/images/registeration-2.png)

And write the following code:
```js
const express = require('express');

const router = new express.Router();

router.post('/register', async (req, res) => {
  res.status(200).send({ user: 'user' });
});

module.exports = router;
```

### Create Helper for Password and Token
At the helper, folder create two new files, `password.js` and `token.js` 

In the **passowrd.js**, we will create two method, one for hashing  password, one for compare the password
```js
const bcrypt = require('bcrypt');

const SALT_WORK_FACTOR = 10;

/**
 *
 * @param {String} password
 * @returns {Promise<String>}
 *
 * @description this for make password hashed
 */
const hashPassword = async (password) => {
  const salt = await bcrypt.genSalt(SALT_WORK_FACTOR);

  return bcrypt.hash(password, salt);
};

/**
 *
 * @param {String} plainPassword
 * @param {String} hashedPassword
 * @returns {Promise<Boolean>}
 *
 * @description compare plain text password and hashed password
 */
const compareHashedPassword = (plainPassword, hashedPassword) => bcrypt.compare(plainPassword, hashedPassword);

module.exports = {
  hashPassword,
  compareHashedPassword,
};
```

In the **token.js**, we will create one function called `createToken`

```js
const jwt = require('jsonwebtoken');
const { tokenPrivateSecreteKey, tokenSignOptions } = require('../../config/app-config');

const createToken = async (payload) => {
  const token = jwt.sign(payload, tokenPrivateSecreteKey, tokenSignOptions);
  return token;
};

module.exports = {
  createToken,
};
```

And you need to update the `app-config.js` to contain the `tokenPrivateSecreteKey`, and `tokenSignOptions`

```js
tokenPrivateSecreteKey: process.env.TOKEN_PRIVATE_SECRETE_KEY,
  tokenSignOptions: {
    expiresIn: process.env.TOKEN_EXPIRE_IN,
    algorithm: process.env.ALGORITHM,
    audience: process.env.AUDIENCE,
    issuer: process.env.ISSUER,
  },
```


### Create User Service

After we create the registration route, let's create the service, create a file called `user.service.js`
![](/images/regiseration-3.png)

And write the following code 
```js
const { hashPassword } = require('../helper/password');
const { createToken } = require('../helper/token');
const UserModel = require('./user.schema');

class UserService {
  constructor() {
    this.model = UserModel;
  }

  getByEmail(email) {
    return this.model.findOne({ email, isDeleted: false }).lean();
  }

  async register(args) {
    const { password, email } = args;

    const user = await this.getByEmail(email);

    if (user) {
      throw new Error('User already exists');
    }

    const hashedPassword = await hashPassword(password);

    const createdUser = await this.model.create({ ...args, password: hashedPassword });
    const token = createToken(createdUser.toObject());

    return { ...createdUser.toObject(), token };
  }
}

module.exports = new UserService();
```

### Update user routes and register user routes
Open the `user.routes.js` and require the user service and use it
```js
const express = require('express');
const userService = require('./user.service');

const router = new express.Router();

router.post('/register', async (req, res) => {
  const user = await userService.register(req.body);

  res.status(200).send({ user });
});

module.exports = router;
```

After that register the user routes on the app
```js
const express = require('express');
const path = require('path');
const bodyParser = require('body-parser');
const uploadImagesRoutes = require('./upload-images/upload-image.routes');
const uploadFilesRoutes = require('./uploading-files/uploading-files.routes');
const userRoutes = require('./user/user.routes');

const app = express();

app.use(bodyParser.json({ limit: '50mb' }));
app.use(bodyParser.urlencoded({ limit: '50mb', extended: true, parameterLimit: 50000 }));
app.use(express.static(path.join(__dirname, '../uploads')));

app.use('/images', uploadImagesRoutes);
app.use('/files', uploadFilesRoutes);
app.use('/users', userRoutes);

module.exports = app;
```

### Test the Register API By Postman

![](/images/regiseration-4.png)

![](/images/registeration-5.png)

You can find the complete repo on this [Link](https://github.com/miladezzat/educative-node-js-course).




![](/images/check-auth-0.gif)

## Authentication
Authentication is the act of proving an assertion, such as the identity of a computer system user. In contrast with identification, the act of indicating a person or thing's identity, authentication is the process of verifying that identity.

### Create Check Auth Middleware

First, what is the `Middleware`

Middleware is basically a function that will receive the Request and Response objects, just like your route Handlers do. As a third argument, you have another function that you should call once your middleware code is completed.

> Middleware is a type of computer software that provides services to software applications beyond those available from the operating system. It can be described as "software glue".

Second, Create a check auth middleware for our app like the following.

```js
const jwt = require('jsonwebtoken');
const { tokenPublicSecreteKey, tokenSignOptions } = require('../../config/app-config');

const checkAuth = (req, res, next) => {
  const token = req.header('x-auth-token') || req.header('token');

  if (!token) {
    throw new Error('authorization denied');
  }

  try {
    req.user = jwt.verify(token, tokenPublicSecreteKey, tokenSignOptions);

    next();
  } catch (error) {
    throw new Error('authorization denied');
  }
};
module.exports = checkAuth;

```

You need to update the app-config.js to contain a token public key.

> This is a good [website](https://travistidwell.com/jsencrypt/demo/) to create a pair of keys 

>Image result for what is private and public pair keys
The public key is used to encrypt messages and the private key is used to decrypt messages. ... Only the owner of the key pair knows the private key, but everyone can know the public key. The owner uses his private key this time, instead of someone's public key, to encrypt a message (c= md mod n).
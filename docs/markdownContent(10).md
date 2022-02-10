![](/images/mongoose-0.png)
## What is Mongoose?

Mongoose is a Node. js-based Object Data Modeling (ODM) library for MongoDB. It is akin to an Object Relational Mapper (ORM) such as SQLAlchemy for traditional SQL databases. The problem that Mongoose aims to solve is allowing developers to enforce a specific schema at the application layer.

> Mongoose is a JavaScript object-oriented programming library that creates a connection between MongoDB and the Express web application framework. [read more](https://mongoosejs.com/)

### Installing Mongoose 
Let's install the `mongoose` npm package to our project, `npm i mongoose`

![](/images/mongoose-1.png)

### Create User Schema

Let's start to create a `user` component firstly, and create a file called `user.schema.js` on it.

Open the `user.schema.js` and write the following code on it
```js
const mongoose = require('mongoose');

const UserSchema = new mongoose.Schema({
  firstName: {
    type: String,
  },

  lastName: {
    type: String,
  },

  email: {
    type: String,
    required: true,
  },

  password: {
    type: String,
    required: true,
  },

  isDeleted: {
    type: Boolean,
    default: false,
  },
}, { timestamps: true });

module.exports = mongoose.model('user', UserSchema);

```

By this way we created a user schema with the basic user information.


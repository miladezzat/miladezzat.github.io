![](/images/mongodb-0.png)

[Mongoose.js](https://mongoosejs.com/) connects your MongoDB clusters or collections with your Node.js app. It enables you to create schemas for your documents. Mongoose provides a lot of functionality when creating and working with schemas.

## How to Connect MongoDB to Node.js Using Mongoose

MongoDB is one of the most widely used No-SQL databases in the developer world today. No-SQL databases allow developers to send and retrieve data as JSON documents, instead of SQL objects. To work with MongoDB in a Node.js app, we can use Mongoose.

### Create Connection Instance
Firstly create a file called `connect-to-db.js` at the root of the project.
![](/images/mongodb-1.png)

Open the file and write the following code on it.

```js
const mongoose = require('mongoose');

const { databaseUrl, databaseAuth } = require('./config/app-config');

const connectToDB = async () => {
  mongoose.Promise = global.Promise;

  return mongoose.connect(
    `${databaseUrl}`,
    {
      auth: databaseAuth,
      useNewUrlParser: true,
      useUnifiedTopology: true,
    },
  )
    .then(() => console.log('MongoDB connected', { databaseUrl }))
    .catch((error) => console.error('Error to connect to mongodb', { error, message: error.message }));
};

module.exports = connectToDB;
```
`const { databaseUrl, databaseAuth } = require('./config/app-config');` this is the configuration of `MongoDB`
Open the `app-config.js` file and update it.

```js
module.exports = {
  appPort: process.env.PORT,
  appHost: process.env.APP_HOST,
  databaseUrl: process.env.DATABASE_URL,
  databaseAuth: process.env.DATABASE_AUTH
    ? JSON.parse(Buffer.from(process.env.DATABASE_AUTH, 'base64').toString('utf8'))
    : null,
};
```

> Notice that we encoded the database auth with based64 coded

> In computer programming, Base64 is a group of binary-to-text encoding schemes that represent binary data in an ASCII string format by translating the data into a radix-64 representation. The term Base64 originates from a specific MIME content transfer encoding

### Connection To Database
Open the `index.js` file and require the connect to database function and update the code like this:
```js
require('dotenv').config();
const app = require('./src/app');
const connectToDB = require('./connect-to-db');
const { appPort } = require('./config/app-config');

const boot = async() => {
  await connectToDB();
  app.listen(appPort, () => {
    console.log(`Server running on port ${appPort}`);
  });
};

boot();
```

> You can find the complete repo on [Link](https://github.com/miladezzat/educative-node-js-course)
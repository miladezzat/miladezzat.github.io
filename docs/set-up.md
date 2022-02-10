Let's start our journey with Node.js

## In this section, we will learn that:
1. Install express and create the app.
2. Use dotenv for environment varaibles.
3. Create a GitHub repo and push the code on it.

### Install Express.js and create the app
- Create a folder called `node.js-course`, or what you want, by command line `mkdir node.js-course`
- Move to the folder by command line `cd node.js-course`.
- Initialization the node.js project by `npm init`, and answer the question you see.
- Create a file called `index.js`,  a folder called `src`, and create a file `called app.js`.
- Install `express` by `npm i express`.

After you create the basic structure and install the main package let's start to play with code.

**Firstly** open the `app.js` and write the following code on it:
```js
const express = require('express');

const app = express();

app.use(express.json())
app.use(express.urlencoded({ extended: true }))

module.exports = app;
```

by this code, you create a node.js app and export it to use.

**Second** open the `index.js` and write the following code:
```js
const app = require('./src/app');

const boot = () => {
    app.listen(3000, () => {
        console.log(`Server running on port 3000`);
    })
}

boot();
```

by this, you create the server and make it ready to run on the post `3000`.

**Third** open the `package.json`, and on the **script** section delete the test script, and create a new one like: 
```js
 "scripts": {
    "start:dev": "nodemon index.js"
  },
```

>***nodemon*** is a utility depended on by over 1.5 million projects, that will monitor for any changes in your source and automatically restart your server. Perfect for development, [read more](https://nodemon.io/)

You need to install the `nodemon` as devDependencies by `npm i nodemon -D`

To run the app just rung `npm run start` you will see:
![](/api/collection/6505801670721536/6326353141432320/page/5503972802035712/image/5602769544675328?page_type=collection_lesson)


### Use dotenv for environment variables.

if you open the file called `index.js`, you will be noticed that we set the port as a magic value, didn't you ask yourself what if I want to change the port and didn't need to change on the codebase.

we will use the `dotenv` package for Storing configuration in the environment separate from code is based on [The Twelve-Factor](https://12factor.net/) App methodology. 

- Install the `dotenv` by `npm i dotenv`
- Require the `dotenv` to `index.js` file and it will be like:
```js
require('dotenv').config();
const app = require('./src/app');

const boot = () => {
    app.listen(3000, () => {
        console.log(`Server running on port 3000`);
    })
}
boot();
```

- Create a file called `.env`, and set `PORT=3000` on it
- Create a folder called `config` on the root of the project, and contain a file called `app-config.js`
> note: we use the kabab-case to create the files and folder name
- Open the app-config.js file and write the following code on it
```js
module.exports = {
    appPort: process.env.PORT
};
```
- Open the index file and require the app-config on it and change the constant port with the  `appPort`,  `const { appPort } = require('./config/app-config.js')`.
- Back to index.js and require the config file and replace the `3000` to be `appPort`

```js
const app = require('./src/app');
const { appPort } = require('./config/app-config.js');

const boot = () => {
    app.listen(appPort, () => {
        console.log(`Server running on port ${appPort}`);
    })
}
boot();
```

## Create a GitHub repo and push the code on it.
- Open your Github account and create new repo called `educative-node-js-course`.
- On your codebase create file `.gitignore` on  the project root and set the following on it
```
node_modules
.env
```
- Write the following command `git init`.
- Add the code to git stage and commit it, `git add .` and `git commit -m "set up the project"`.
- Add the remote GitHub repo to your local repo `git remote add origin <YOUR-REMTE-REPO-LINK>`
- Push your code to Github `git push -u origin master`


You will find the complete repo on this [Link](https://github.com/miladezzat/educative-node-js-course)
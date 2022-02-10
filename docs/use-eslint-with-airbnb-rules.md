## What is Eslint

![](/images/what-is-eslint.png)

> ESLint is a static code analysis tool for identifying problematic patterns found in JavaScript code. It was created by Nicholas C. Zakas in 2013. Rules in ESLint are configurable, and customized rules can be defined and loaded. ESLint covers both code quality and coding style issues. 

### Install Eslint
you can install it as global on your machine, or you can install it as devDependencies on your project.
- Install as global `npm install eslint -g`
- Install as devDependencies `npm install eslint -D`

### Setup on your project
After you install the **Eslint** on your project, you need to set up it.

> if you use the vscode, you can install the Eslint extension, which will help you to debug and do more things [Link](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

you must run the following commands to setup the eslint on your project
`eslint --init` as the following photo:

![](/images/install-eslint-1.png)

choose the choice three `To check syntax, find problems, and enforce code style` you will see as the following photo.

![](/images/install-eslint-2.png)

choose the second choice `CommoJS (require/exports)`, and you will see the following photo.

![](/images/install-eslint-3.png)

Choose the last choice `None of these`, and you will see the following photo.

Choose `No`, and you will see the following

![](/images/install-eslint-4.png)

Choose `node` option, and you will see the following

![](/images/install-eslint-5.png)

Choose the first choice `Use a popular style guid`, and you will see the following:

![](/images/install-eslint-6.png)

Choose the first choice ` Airbnb: https://github.com/airbnb/javascript`, and you will see the following:

![](/images/install-eslint-7.png)

Choose the last option `JSON`, and press enter to install dependencies.
**Congratulations** you setup the eslint on your project and you can see the new file on your project called `.eslintrc.json`

![](/images/install-eslint-8.png)


After that, we will add the new script to the package JSON file.

```js
  "scripts": {
    "start:dev": "nodemon index.js",
    "lint": "eslint"
  },
```

You can run the lint script by `npm run lint` to find the Eslint bugs and solve them

From more information about eslint you can visit the [official documentation ](https://eslint.org/)


![](/images/handling-error.png)

## What is Error Handling

It's a way to see bugs in your code. Following this logic, error handling is a way to find these bugs and solve them as quickly as humanly possible. ... Handling errors properly means developing a robust codebase and reducing development time by finding bugs and errors easily, read more about [Error Handling](https://sematext.com/blog/node-js-error-handling/)

### Using ExpressJS Async Errors

A dead-simple ES6 async/await support hack for ExpressJS
Shamelessly copied from express-yields
This has been slightly reworked to handle async rather than generators.

To install it on our project `npm i express-async-errors`, and require it on the `app.js` like:
```js
const express = require('express');
require('express-async-errors');
```

This package catches all the `async` errors and passes them to the error handler middleware. 

### Using Error Handler E2 package
This package contains some useful middlewares like `loggerMiddleware`, `notFoundMiddleware` and `errorHandlerMiddleware`, also contains some custom error like `BadRequestError`, also contains some HTTP status codes.

Install it `npm i error-handler-e2`; and use the middlewares `loggerMiddleware`, `notFoundMiddleware` and `errorHandlerMiddleware` on `app.js` like: 

```js
const {
  Middleware: {
    handleErrorMiddleware,
    loggerMiddleware,
    notFoundMiddleware,
  },
} = require('error-handler-e2');

...

// on the top of all middlewares
app.use(loggerMiddleware);
...

// on the bottom of all middlewares
app.use(notFoundMiddleware);
app.use(handleErrorMiddleware);
```

> Note that to use the `handleErrorMiddleware` middleware of this package you need to update all your native errors to use the custom errors on this package.

## Change All Native Throw Error
Search about all throw native errors and change them like this:
```js
const { Errors: { BadRequestError } } = require('error-handler-e2');

throw new BadRequestError('Data Validation Error', { error });
```




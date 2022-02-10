![](/images/set-multer.webp)

## What is Multer?
> Multer is a node.js middleware for handling multipart/form-data, which is primarily used for uploading files. It is written on top of busboy for maximum efficiency. [read more](https://www.npmjs.com/package/multer)

Whenever we submit a form on the client-side of any website, all the form data goes to the server-side. Usually, form-data gets encoded before we submit it to the server. We can do this by specifying the enctype attribute in the `<form>` tag in HTML. If we don’t specify it, form-data gets encoded with the default type.

This is usually the case when we are dealing with text-only data like name, email, and password, etc.

But, if we are uploading some kind of files, we need to specify the enctype attribute with the value multipart/form-data. This value is required when we are using forms that have a file input type element.

Multer is an npm package that makes it easy to handle file uploads. It does it very efficiently, thus it is quite popular. In this article, we will see how to use Multer to handle multipart/form-data using Node.js, Express, and MongoDB.

Before using Multer to handle the upload action of files, we need to understand a few things.

The actual files are never stored in the database. They are always stored someplace on the server. In our tutorial, we will store the uploaded files in the public folder.

This is because all the files that are in the public folder are meant to be available to the public at the front-end.

## Install and config multer 
`npm i multer`

- create a helper folder and contain a file called `multer.js`, and set the following code on it
```js
const multer = require('multer');
const path = require('path');

const createMulter = (extensions = [], message = '') => multer({
  storage: multer.memoryStorage(),
  fileFilter(req, file, callback) {
    const ext = path.extname(file.originalname);

    if (!extensions.includes(ext.toLowerCase())) {
      req.error = new Error(message);
    }

    callback(null, true);
  },

  limits: {
    fileSize: 25 * 1024 * 1024, // max file size will be 25MB
  },
});

module.exports = createMulter;

```
We will learn how to use the Multer in the next lesson.

For more information about Multer visit this [website](https://afteracademy.com/blog/file-upload-with-multer-in-nodejs-and-express)
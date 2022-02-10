![](/images/create-upload-image-api.jpeg)

When building APIs, the need to upload files is expected, which can be images, text documents, scripts, pdfs, among others. In the development of this functionality, some problems can be found, such as the number of files, valid file types, sizes of these files, and several others. And to save us from these problems we have the Multer library. Multer is a node.js middleware for handling multipart/form-data that is used to send files in forms, at this lesson we will create an API for uploading images

After we setup Multer let's start to create an API for uploading a single image

## Upload single Image API
we will use component structure style like `angular` structure,
create a folder called `upload-images`, that contains files
- `upload-image.routes.js`, this file will be containing all `routes` of images uploading.
- `upload-images.constants.js`, this file will be containing all `constants` of uploading images.

> notice that we use the `kabab-case` for naming the `files` and `folder`, and use the `camelCase` from `variables` and `functions` names, and `CAPITALIZATION` for `constants`.

we will be using the router function from Expressjs to create HTTP `post` request, you can read more about [router](https://expressjs.com/en/guide/routing.html)

### What is HTTP request
An HTTP request is made by a client, to a named host, which is located on a server. The aim of the request is to access a resource on the server. To make the request, the client uses components of a URL (Uniform Resource Locator), which includes the information needed to access the resource.

### Create POST Request For Uploading Single Image
Open `upload-image.routes.js` file and write the following code on it.

```js
const express = require('express');
const multer = require('../helper/multer');
const uploadImagesConstants = require('./upload-images.constants');

const router = new express.Router();

router.post('/single', multer(uploadImagesConstants.EXTENSIONS).single('image'), async (req, res) => {
  console.log(req.file);
  res.status(200).send({ url: 'url' });
});

module.exports = router;
```

**Explain the code**
- The first line, the require of Expressjs package
- The second line, the require of `multer` helper function that we created before
- `const uploadImagesConstants = require('./upload-images.constants');` this for the constants of the images, we will add the extensions of image on it later in this lesson.
- `const router = new express.Router();`, this is an instance of expressjs router
- `router.post('/single')`, this is the API for uploading a single image via this route
- `multer(uploadImagesConstants.EXTENSIONS).single('image')` this is the helper function middleware.

Open `upload-images.constants` open this file and create constant for the image extensions like:
```js
const EXTENSIONS = ['jpeg', 'png', 'webp', 'ipg'];

module.exports = {
  EXTENSIONS,
};
```
`EXTENSIONS`, these are the extensions of the images that we need to send to the Multer helper function. (these are extensions that we allowed the user to upload them)

At this point, we upload the image on the memory storage, at the next lesson we will use [sharp](https://sharp.pixelplumbing.com/) to optimize the image and set the image on local folder
![](/images/sharp-0.png)

## What is sharp?
Images are an important component of most applications that handle user-generated content. However, excessively large or unoptimized image files can negatively impact performance and user experience. A robust image processing solution can be invaluable for UGC management.

sharp is a high-performance image processing module for Node.js. This module assists with UGC management by offering an easy solution for reading, enhancing, and saving image files. sharp compresses images faster than most other Node.js modules, like ImageMagick, Jimp, or Squoosh, and produces high-quality results.

sharp converts large common image formats to smaller, web-friendly images. sharp can read JPEG, PNG, WebP, AVIF, TIFF, GIF, and SVG image formats. This module can produce images in JPEG, PNG, WebP, AVIF, and TIFF formats as well as uncompressed raw pixel data. if you want to learn more about sharp visit [the official documentation](https://sharp.pixelplumbing.com/)

## Use Sharp
we need to create a service for uploading images, on the folder `upload-images`, create a file called `upload-images.service.js`, and your file structure will be like this:

![](/images/sharp-1.png)

Open this file and write the following code on it 

```js
const path = require('path');
const sharp = require('sharp');
const { EXTENSIONS } = require('./upload-images.constants');
const { appHost } = require('../../config/app-config');

class UploadImages {
  constructor() {
    this.sharp = sharp;
  }

  async uploadSingleImage(args) {
    const {
      file,
      imageFormat = 'webp',
      imageWidth = null,
      imageHeight = null,
    } = args;

    const resizeOptions = {};

    if (imageFormat && !EXTENSIONS.includes(imageFormat)) {
      throw new Error('Error');
    }

    if (imageWidth) {
      resizeOptions.width = +imageWidth;
    }

    if (imageHeight) {
      resizeOptions.height = +imageHeight;
    }

    const fileExtension = path.extname(file.originalname);

    const imagePath = `${Math.random()}-${file.originalname.replace(fileExtension, '')}.${imageFormat}`;
    await this.sharp(file.buffer)
      .resize(resizeOptions)
      .toFormat(imageFormat).toFile(`./uploads/${imagePath}`);

    return `${appHost}/${imagePath}`;
  }
}

module.exports = new UploadImages();
```

We need to update the file `upload-image.routes.js`, require the image service and use it on the route like the following:

```js
const express = require('express');
const multer = require('../helper/multer');
const uploadImagesConstants = require('./upload-images.constants');
const uploadImagesService = require('./upload-images.service');

const router = new express.Router();

router.post('/single', multer(uploadImagesConstants.EXTENSIONS).single('image'), async (req, res) => {
  const url = await uploadImagesService.uploadSingleImage({ file: req.file, ...req.body });

  res.status(200).send({ url });
});

module.exports = router;
```

At this point, you will upload the image on the folder called uploads,
you need to create a folder called `upload` on the root of the project.

![](/images/sharp-2.png)


### Register the API and serve the static file
To register your images routes to the app, open the `app.js` file and write the following code on it.

```js
const uploadImagesRoutes = require('./upload-images/upload-image.routes');
...
app.use('/images', uploadImagesRoutes);
```
To serve your images on your app, you need to use `express.static` method, this method is used to serve static files on Expressjs. if you want to [read more](https://expressjs.com/en/starter/static-files.html)
```js
app.use(express.static(path.join(__dirname, '../uploads')));
```

The last version of the app.js 

```js
const express = require('express');
const path = require('path');
const uploadImagesRoutes = require('./upload-images/upload-image.routes');

const app = express();

app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(express.static(path.join(__dirname, '../uploads')));

app.use('/images', uploadImagesRoutes);

module.exports = app;
```

**Congratulation** This is the first endpoint to upload a single image, Now run the project and use this URL `http://localhost:3000/images/single` on the postman.

![](/images/sharp-3.png)

> Postman is an API platform for building and using APIs. Postman simplifies each step of the API lifecycle and streamlines collaboration so you can create better APIs—faster. [download it](https://www.postman.com/)
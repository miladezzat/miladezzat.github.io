![](/images/create-upload-file-api.jpeg)

In the last lesson, we create an API for uploading a single image, right now we will create an API for uploading `pdfs`, `txt`, and `docs`.

## Uploading Files
Let's take a look at what happens when you select a file and submit your form (I've truncated the headers for brevity):

```bash
POST /upload?upload_progress_id=12344 HTTP/1.1
Host: localhost:3000
Content-Length: 1325
Origin: http://localhost:3000
... other headers ...
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryePkpFF7tjBAqx29L

------WebKitFormBoundaryePkpFF7tjBAqx29L
Content-Disposition: form-data; name="MAX_FILE_SIZE"

100000
------WebKitFormBoundaryePkpFF7tjBAqx29L
Content-Disposition: form-data; name="uploadedfile"; filename="hello.o"
Content-Type: application/x-object

... contents of the file go here ...
------WebKitFormBoundaryePkpFF7tjBAqx29L--
```

Each boundary string must be prefixed with an extra `--`, just like at the end of the last boundary string. The example above already includes this, but it can be easy to miss.

Instead of URL encoding the form parameters, the form parameters (including the file data) are sent as sections in a multipart document in the body of the request.

In the example above, you can see the input `MAX_FILE_SIZE` with the value set in the form, as well as a section containing the file data. The file name is part of the Content-Disposition header. for [reading more](https://www.rfc-editor.org/rfc/rfc7578)

**Now** We will create a new `component` called `uploading-files`, that contain three files
- `uploading-files.routes.js`
- `uploading-files.constants.js`
- `uploading-files.service.js`

### Create Constants
This file contains all the file uploading constants.
open the file and write the following code on it
```js
const EXTENSIONS = ['pdf', 'txt', 'docx', 'dot', 'dotx', 'eml']; // these are the extensions we allowed to upload

module.exports = {
  EXTENSIONS,
};
```
### Create Route 
This file will contain all `routes` for uploading files

Open the file and write the following code
```js
const express = require('express');
const multer = require('../helper/multer');
const uploadFileConstants = require('./uploading-files.constants');

const router = new express.Router();

router.post('/single', multer(uploadFileConstants.EXTENSIONS).single('file'), async (req, res) => {
  res.status(200).send({ url: 'url' });
});

module.exports = router;
```
This is the route for uploading a single file.

### Create Service
This file will contain all the service methods of uploading images.
Open the file and write the following code on it
```js
const fs = require('fs');
const { appHost } = require('../../config/app-config');

class UploadImages {
  constructor() {
    this.fs = fs;
  }

  async uploadSingleFile(args) {
    const { file } = args;
    const filePath = `${Math.random()}-${file.originalname}`;

    await this.fs.writeFileSync(`./uploads/${filePath}`, file.buffer, { encoding: 'utf-8' });

    return `${appHost}/${filePath}`;
  }
}

module.exports = new UploadImages();
```

### Update Route & Register Route 
After we create a service for uploading files, we need to do two things

1. Update the route to use the service.
open the file of the routes `uploading-files.routes.js` and require the service and use it like the uploading image
```js
const express = require('express');
const multer = require('../helper/multer');
const uploadFileConstants = require('./uploading-files.constants');
const fileUploadingService = require('./uploading-files.service');

const router = new express.Router();

router.post('/single', multer(uploadFileConstants.EXTENSIONS).single('file'), async (req, res) => {
  const url = await fileUploadingService.uploadSingleFile({ file: req.file });

  res.status(200).send({ url });
});

module.exports = router;

```
2. Register the routes on the app
Open the `app.js` and require the file route like image route and register it
```js
const express = require('express');
const path = require('path');
const uploadImagesRoutes = require('./upload-images/upload-image.routes');
const uploadFilesRoutes = require('./uploading-files/uploading-files.routes');

const app = express();

app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(express.static(path.join(__dirname, '../uploads')));

app.use('/images', uploadImagesRoutes);
app.use('/files', uploadFilesRoutes);

module.exports = app;
```

You can check the last version of the repo on this [Link](https://github.com/miladezzat/educative-node-js-course)
---
layout: post
title:      "Quickly Set Up Multer For Node File Parsing"
date:       2021-07-10 23:29:00 +0000
permalink:  quickly_set_up_multer_for_node_file_parsing
---


Starting with plain HTML we use an input with `type='file'`

```html
<input type="file" name="image" id="image" />
```

However, files can not be parsed into `url-encoded` format directly from HTML so we need a middleware that will be able to handle the parsing of files. For that we’ll use `multer` and change the encoding type of the form to use `multipart/form-data` like so:

```HTML
<form
      class="product-form"
      action="/product"
      method="POST"
      enctype="multipart/form-data"
      >
      	<label for="title">Title</label>
        <input type="text" name="title" id="title">

    	  <label for="image">Image</label>
        <input type="file" name="image" id="image">
      </div>
</form>
```

Now we can tell express to use the `multer` middleware and configure it like so:

```js
// app.js
const multer = require("multer");

app.use(multer().single("image"));
```

When `multer` parses a file it will return it as a Buffer stream with no where to go, so if we give it somewhere to go, `multer` will store the the Buffer as binary in a specified folder.

```js
// app.js
const multer = require("multer");

app.use(multer({ dest: "images" }).single("image"));
```

This will create a directory named `images` and create a file with a long hashed string.

Adding a `.jpg` to the end of the given file name will allow you to view the binary data as an image.

In order to customize how `multer `handles file storages we can pass additional arguments or configurations for `multer `to use:

```js
// app.js
const multer = require("multer");

const fileStorage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, "images");
  },
  filename: (req, file, cb) => {
    cb(null, file.originalname);
  },
});

app.use(multer({ storage: fileStorage }).single("image"));
```

Its common to add something unique to the file name in cases where you have a plethora of files so you could add something like

```js
filename: (req, file, cb) => {
    cb(null, new Date().toISOString() + file.originalname);
  },
```

We can also filter by mime type to only allow specific types of images

```js
const fileFilter = (req, file, cb) => {
  if (["image/png", "image/jpg", "image/jpeg"].includes(file.mimetype)) {
    cb(null, true);
  } else {
    cb(null, false);
  }
};

app.use(
  multer({ storage: fileStorage, fileFilter: fileFilter }).single("image")
);
```

To save on memory we can send files to the browser in streams for the browser to download and reduce server load time and other potential errors.
It also would be worth looking into creating a pdf doc on-the-fly, with a package called `pdfkit` to dynamically create documents and send it as a stream to the browser to be downloaded or shown ‘inline’ on the same page.


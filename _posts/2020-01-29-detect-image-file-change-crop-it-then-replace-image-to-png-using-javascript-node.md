---
date: 2020-01-29T13:47:29.000+00:00
title: Detect Image File Change | Crop It Then Replace Image To .png Using Javascript
  Node
categories:
- Node
- Javascript
tags:
- jpg
- png
- image
- node
- javascript
image: assets/images/Screen Shot 2020-01-28 at 9.40.54 AM.png

---
The objective here is to grab an image that is recently been added to a particular folder. Give the user the ability to crop it with their own dimensions of choosing, then save that cropped image as a png. Lastly and optionally, delete the old image file from the system.

To give some context, I'm using Vue, but obviously, you could do this on react as well.

There are two libraries needed for this task:

* [Cropperjs](https://fengyuanchen.github.io/cropperjs/ "Cropperjs") - as the name suggests
* [Chokidar](https://github.com/paulmillr/chokidar "Chokidar") - use this to detect changes in the folder

First, we want to watch the `folder path` where our image belongs. In this case, I'm checking when an image is added to that folder. Only the image that is to be added to that folder, and not the existing contents that are already in the folder.

    this.watcher = chokidar.watch(folderPath.replace(/\\/g, '/'), {
      ignored: /(^|[\/\\])\../, // ignore dotfiles
      persistent: true,
      ignoreInitial: true
    }).on('add', (event, path) => {
      if (event.match(/\.(jpeg|jpg|gif|png)$/) != null) {
        this.imageUrl = "file://" + event
        this.imageUrlPath = event
        this.imageHTML = `
            <img id="image" src="${this.imageUrl}" style="display: block;max-width: 100%;height: 600px;">
      }
    })

To show the image to the DOM, we must stipulate that this image does not come from our app's static files folder by adding `file://` before the path of the image's location, which is the `event`. But we don't need that when we're saving or deleting the image file, hence a different variable was created for those purposes.

For some reason, we have to delay the cropper functionality from appearing, because inserting the img to our DOM probably took some time. But before all that, we must first have the cropper.min.js and cropper.min.css, which you'll find [here](https://cdnjs.com/libraries/cropperjs "Cropper CDN"), added to your HTML.

    setTimeout(() => {
      const image = document.getElementById('image');
      image.addEventListener('cropend', () => {
        var croppedImage = cropper.getCroppedCanvas().toDataURL()
        var base64Data = croppedImage.replace(/^data:image\/png;base64,/`, "");
    
        if (!!this.watcher) this.watcher.close().then(() => this.watcher = null )
        fs.writeFile(this.imageUrlPath.replace(/\.jpg$/i, ".png"), base64Data, 'base64', function (err) {
          if (err) {
            console.error(err)
          } else {
            console.log("Image has been saved")
          }
        });
      })
      const cropper = new Cropper(image);
    }, 100)

So this whole thing here becomes active when I show a view allowing the user to crop the image. I grab the image by its ID, and added a listener to it every time a crop has happened, otherwise, it will detect every event that manifests when you're still dragging the corners of the cropper. Get that base46 value of that image and assign it to `croppedImage`. To save it as a picture, you must first remove `data:image/png;base64,`.

Notice that `cropper` is initialized after the event listener, but somehow it just works inside the `addEventListener`.

Before saving it, we must first close the watcher to prevent it from detecting an image that we are about to add or save to that particular folder. Then we replace that image's extension to png, if it's a jpg, with our cropped image.

#### OPTIONAL

    fs.unlink(this.imageUrlPath, (err) => {
      if (err) {
        this.imageUrl = this.imageUrl.replace(/\.jpg$/i, ".png")
        console.error(err)
      } else {
        console.log("Image has been removed")
      }
    })

If we want to delete the old .jpg file, then we simply call this `unlink`. In my case, this is called when the view of where the cropping happens is put away.

Oh and don't forget to `const fs`**`=`**`require('fs')` before maneuvering files.
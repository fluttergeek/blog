---
date: 2021-01-06 09:16:46 +00:00
title: Displaying a Uint8List Image File From Firebase Storage and [Flutter Web] 2021
categories:
- Web
- Images
- Flutter
tags:
- images
- firebase storage
- uint8list
- flutter web
image: assets/images/5ae2a2cd4cc19f39a4e30b51_download-icon.png

---
Presently, I haven't found any source that would allow me to upload and download an image in Flutter web in a more structured way. I only found bits and pieces scattered throughout Stackoverflow. It's not all helpful but I found some snippets that could help.

I previously wrote an [article](https://fluttergeek.com/blog/flutter-web-uploading-an-image-using-firebase-storage/ "Flutter Web Uploading An Image Using Firebase Storage") on how to upload an image with Flutter web but it wasn't displaying the image uploaded the way it's supposed to.

The image codec, or whatever it's called, is uploaded as **Uint8List**. When I try to used **CachedNetworkImage**, it won't load the image URL. It keeps saying:

    ImageCodecException: Failed to load network image. Image URL:...

I realized it's not really a JPEG file. Although, when I use the URL and manually load it on a browser, it displays the image flawlessly. The putData only uploaded it as Uint8List, regardless of adding the **.jpeg** extension on the filename. That's when I knew, I just had to download it as a Uint8List first.

But before anything, let's allow downloading a file from a **firebase_storage** using Flutter web by configuring our CORS. There's a straightforward instruction for this: [CORS Configuration](https://firebase.google.com/docs/storage/web/download-files "CORS Configuration"). It's kinda tedious but trust me, it was smooth sailing; I didn't run into any problems configuring that. 

Now, we can start to download the image from **firebase_storage** without hiccups.

    http.Response response = await http.get(
      await _firestorageService.uploadPicture(file: file),
    );
    
    return response.bodyBytes;

The uploadPicture here uploads the image and returns the URL. We use that URL to get a response, and the **bodyBytes** here is the **Uint8List**.

Now, we display that **Uint8List** into **Image.memory(\[put_uint8list_here\])** and voila, that's how you display an image that isn't really an image codec.
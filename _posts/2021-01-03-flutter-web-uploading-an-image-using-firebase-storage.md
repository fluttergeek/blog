---
date: 2021-01-03 20:02:37 +00:00
title: Flutter Web Uploading An Image Using Firebase Storage
categories:
- Web
- Flutter
tags:
- firebase storage
- image upload
- flutter web
image: assets/images/1.jpg

---
Uploading an image on the Web with the use of **Flutter** might have been something unheard of until late 2020. It's not permitted to go through the File system in your browser with Javascript. I'm saying this because I initially planned on having a file system listener inside a local folder like I was able to do with Electron-Vue.

Even now, there is hardly a bulletproof solution around this; but a solution, there is.

There have been plenty of other articles written about how to upload an image using flutter web. It even entails adding a number of other dependencies. But here, you'll need the **firebase_storage** of course; the latest version you can find. Moreover, in my web/index.html, I use the firebase-storage.js version 7.22.1. The other dependency you need is **image_picker**.

There is one other dependency that I have imported but haven't used; **image_picker_for_web**.

I don't know why but the **image_picker** only works and doesn't give me this error:

    MissingPluginException(No implementation found for method pickImage on channel plugins.flutter.io/image_picker)

... if I have **image_picker_for_web** imported in the dart file where I'm using **image_picker**.

There is no need to initialize **firebase_storage** in index.html or in main.dart, just the use of FirebaseStorage.instance.

     final FirebaseStorage _firebaseStorage = FirebaseStorage.instance;

Caveat...

In my experience, I haven't been able to upload images when uploading an image thru the localhost. I've had to deploy the project to firebase hosting to steer clear of the access error.

The **image_picker** is pretty straightforward to use.

    final picker = ImagePicker();
    final PickedFile image = await picker.getImage(source: ImageSource.gallery);

Although we're using flutter web, we can still use **ImageSource.gallery** as the source of our image's location. However, the return type is not what you're actually expecting. **PickedFile** is not the same as **File** and there's no way you can directly convert it to **File** type, unless you do conversions.

In this article, the file type we're going to extract off of PickedFile is a **Uint8List**, other known as bytes. You'd need to import 'dart:typed_data'; for that.

    Uint8List bytes = await image.readAsBytes();
    
    Reference ref = _firestorage.ref().child('${DateTime.now()}.png');
    UploadTask uploadTask = ref.putData(bytes, SettableMetadata(contentType: 'image/png'));
    TaskSnapshot taskSnapshot = await uploadTask
    	.whenComplete(() => print('done'))
        .catchError((error) => print('something went wrong')));
    String url = await taskSnapshot.ref.getDownloadURL();

Now that you've learned to upload, downloading this file is kind of different from the way you're used to. I wrote another article about that; [Displaying a Uint8List Image File From Firebase Storage and \[Flutter Web\] 2021](/blog/displaying-a-uint8list-image-file-from-firebase-storage-and-flutter-web-2021/).
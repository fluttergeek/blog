---
date: 2020-12-16 22:49:08 +00:00
title: Encode/Decode "File" to JSON While Using Freezed and HydratedBloc
categories:
- Freezed
tags:
- json
- decode
- encode
- file
- base64
- flutter bloc
- freezed
- hydrated bloc
image: assets/images/d8mln-lxyaa7hnz-jpg_large.jpeg

---
You might have encountered an error when using a **HydratedBloc**. It turns out your Model that has a **File** member cannot be encoded or decoded to **JSON**. Well, this is a snippet on how you can work around making your model compatible with hydrated bloc.

This will give you an idea of how to make it compatible. You can even build your own idea on top of it. You can replace the now date that I did in fromJson to your preferred string just to make a File out of a **JSON**.

Caveat: The files I'm trying to encode or decode are images. Hence, I used "**.png**" as a file type. Change it to whatever file type your app requires.

    import 'dart:io';
    import 'dart:io' as Io;
    import 'dart:convert';
    
    import 'package:json_annotation/json_annotation.dart';
    import 'package:freezed_annotation/freezed_annotation.dart';
    
    part 'model.freezed.dart';
    part 'model.g.dart';
    
    @freezed
    abstract class Model with _$Model {
      const factory Model({
        @JsonKey(fromJson: Model._fromJsonFile, toJson: Model._toJsonFile)
            File file,
      }) = _Model;
    
      static Model fromDocument({DocumentSnapshot doc}) {
        return Model(file: null);
      }
    
      factory Model.fromJson(Map<String, dynamic> json) =>
          _$ModelFromJson(json);
    
      static File _fromJsonFile(Map<String, dynamic> data) {
        if (data == null) return null;
        final decodedBytes = base64Decode(data["img64"]);
    
        File file = Io.File("${DateTime.now()}.png");
        file.writeAsBytesSync(decodedBytes);
        return file;
      }
    
      static Map<String, dynamic> _toJsonFile(File file) {
        if (file == null) return null;
        try {
          final bytes = Io.File(file.path).readAsBytesSync();
          String img64 = base64Encode(bytes);
          final Map<String, dynamic> data = new Map<String, dynamic>();
          data["img64"] = img64;
          return data;
        } catch (e) {
          return null;
        }
      }
    }
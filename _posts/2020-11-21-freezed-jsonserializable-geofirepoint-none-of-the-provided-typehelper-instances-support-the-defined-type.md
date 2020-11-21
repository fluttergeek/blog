---
date: 2020-11-21 03:31:31 +00:00
title: Freezed | JsonSerializable | GeoFirePoint | None of the provided TypeHelper
  instances support the defined type.
categories:
- Flutter
- Bug
tags:
- geofirepoint
- json_annotation
- json_serializable
- freezed
- flutter
image: assets/images/1_xrvmy8qxakw_-sldq5jcta.jpeg

---
This is for when you create a class that has a member with an unusual type, like for example, GeoFirePoint.

    @freezed
    abstract class Owner with _$Owner {
      const factory Owner(
        String uid,
        List<Pet> pets,
        String name,
        String email,
        String loc,
        GeoFirePoint point,
        String profile_picture,
      ) = _Owner;
      factory Owner.fromJson(Map<String, dynamic> json) => _$OwnerFromJson(json);
     }

And when you try to run build_runner, this will go to shit, because it doesn't know GeoFirePoint or how to serialize that type into JSON.

I probably found this solution here: [https://stackoverflow.com/questions/59002699/dart-parse-to-from-json-a-library-class-using-json-serializable](https://stackoverflow.com/questions/59002699/dart-parse-to-from-json-a-library-class-using-json-serializable "Stackoverflow")

Although he wasn't using @freezed, it still works the same, but with a little bit of extra.

    @freezed
    abstract class Owner with _$Owner {
      const factory Owner(
        String uid,
        List<Pet> pets,
        String name,
        String email,
        String loc,
        @JsonKey(fromJson: Owner._fromJsonGeoFirePoint, toJson: Owner._toJsonGeoFirePoint)
        GeoFirePoint point,
        String profile_picture,
      ) = _Owner;
      
      factory Owner.fromJson(Map<String, dynamic> json) => _$OwnerFromJson(json);
      
      static GeoFirePoint _fromJsonGeoFirePoint(GeoFirePoint geoPoint) {
        return geoPoint;
      }
    
      static GeoFirePoint _toJsonGeoFirePoint(GeoFirePoint geoPoint) {
        return geoPoint;
      }
     }

Use the **JsonKey** annotation to set functions for (en)decode object, just like the one shown in Stackoverflow. However, it is imperative that you add the class name before "._fromJsonGeoFirePoint" and "._toJsonGeoFirePoint" because the generated g.dart class will also need to have access to those static functions.

You're set to generate the json_serializable after this.
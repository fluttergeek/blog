---
date: 2020-01-12 07:47:51 +00:00
title: 'FriendFace: Saving Nested Object In CoreData With SwiftUI'
categories:
- Tutorial
tags:
- save
- storing
- object
- nested
- coredata
- swiftui
image: assets/images/codable-json-swift-how-to-770x400-1.jpg

---
There's no `face` image in this challenge, from 100 days of SwiftUI. I will only be discussing my codes. Thanks to Peter Barclay for his [repo](https://github.com/websurgeon-hws/hws-FriendFace/tree/Day-61-End "Github"). I assure you I didn't follow through his logic, but it helped me construct my own logic. Let me start by first showing you the `User` model and the `Friend` model.

    struct User: Codable, Identifiable {
        var id: UUID
        var name: String
        var age: Int16
        var company: String
        var friends: [Friend]
     }

    struct Friend: Codable, Identifiable {
        var id: UUID
        var name: String
    }
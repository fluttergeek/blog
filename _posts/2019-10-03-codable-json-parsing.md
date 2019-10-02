---
title: Codable JSON Parsing
categories: Tutorial Swift
image: assets/images/codable-json-swift-how-to-770x400.jpg
tags:
- ''
- swift
- codable
- decode
- encode
- SwiftyJSON
- CodingKeys
---

One of the reasons why you should learn this is, first, because you're an iOS developer. You need to trim down the size of your app by avoiding 3rd party frameworks. Now, let's look at the code done by [Code Pro][cp].

```
let singleDictJSON = """
    {
        "foodName": "Banana",
        "calories": 100
    }
""".data(using: .utf8)!

class Food: Codable {
    let foodName: String
    let calories: Int
    
    init( foodName: String, calories: Int ){
        self.foodName = foodName
        self.calories = calories
    }
}
```

We are going to decode the json dictionary above. It is in UTF8 for a reason. That is, it allows us to decode it into an object that is an instance of the class `Food`. Another requirement for us to be able decode or encode is to make the class inherit `Codable`. Decode is just a fancy way of saying "from json to swift object". 

```
let foodDecoder = JSONDecoder()

do {
    let foodResult = try foodDecoder.decode(Food.self, from: singleDictJSON)
    print(foodResult.foodName)
    print(foodResult.calories)
} catch {
    print("Failed to decode the food: \(error.localizedDescription)")
}
```

Provided that singleDictJSON conforms to the `Food` class then it should not throw an error. However if you remove calories and its value, it will throw an error when decoding it into a `Food` object. If you stick to that, without a calories in the json, then you'd have to make the calories variable in your class optional, as well as the calories in the initializer. If this is the case, then `foodResult.calories` will have a value of nil and will not throw an error if the json does not contain calories.

```
let apple = Food(foodName: "Apple", calories: 80)
let jsonEncoder = JSONEncoder()
jsonEncoder.outputFormatting = .prettyPrinted

do {
    let jsonData = try jsonEncoder.encode(apple)
    if let jsonString = String(data: jsonData, encoding: .utf8) {
        print(jsonString)
    }
} catch {
    print("Failed to decode the food: \(error.localizedDescription)")
}
```

What you see is just the reverse of the previous code. It's pretty simple isn't it? Encode is just another way of saying "from object to json". `.prettyPrinted` is just a writing option that uses white space and indentation to make the output more readable in the console as opposed to a more compact string. If you have no plans on using print on the jsonString, then you don't have to make it pretty.

Here's a more elaborate code, also taken from Code Pro's [video tutorial][cp]. 

```
let dogOwnerJSONData = """
[
    {
        "name": "Jameson",
        "age": 35,
        "dogs": [
            {
                "dog_name": "Bryce",
                "breed": "Pug",
                "age": 7
            },
            {
                "dog_name": "Furby",
                "breed": "Golden Retriever",
                "age": 2
            }
        ]
    },
    {
        "name": "Liza",
        "age": 30
    }
]
""".data(using: .utf8)!
```

Woah! That's a bigger structure with a more complex pattern. Here, one of the owners don't actually own a dog. Not even one. That's insane!

```
struct Dog: Codable {
    let name: String
    let breed: String
    let age: Int
    
    private enum CodingKeys: String, CodingKey {
        case name = "dog_name"
        case breed
        case age
    }
}
```

Woah! That's even more insane. We're using CodingKeys now to map to each attributes available for this struct. FYI, structs can also inherit from Codable. Why are we doing the CodingKeys enum? Well, that's because the dog_name in our json isn't compliant with the struct's dog's name attribute. It will throw an error if we don't do anything about it. If you're using CodingKeys, you can't just map just the one with a different key in the json. Like I said, you have to map to each and every attribute in your struct.

```
struct Owner: Codable {
    let name: String
    let age: Int
    let dogs: [Dog]?
}
```

It's a bit toned down now. `Dogs` is now just an optional and it will do just fine if the owner doesn't have a dog. No error will throw. It's imperative to note that `dogs` is an array here.

```
let dogOwnerDecoder = JSONDecoder()

do {
    let dogOwners = try dogOwnerDecoder.decode([Owner].self, from: dogOwnerJSONData)
} catch {
    print("Failed to decode the dog owners: \(error.localizedDescription)")
}
```

This is familiar, but there's one tiny difference. We have more than one owner in our json, hence we need an array of `Owner` like this `[Owner].self`. So that's how you decode an array. 

[cp]: https://www.youtube.com/watch?v=Oj-2s0ALACE&list=WL&index=12&t=481s

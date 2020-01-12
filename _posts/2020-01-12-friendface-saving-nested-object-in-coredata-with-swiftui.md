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

The code below was placed inside my User struct:

    static func fetch() -> [User] {
            
            let cdUsers = User.fetchCoreData()
            
            var users: [User] = [User]() {
                didSet {
                    if cdUsers.count == 0 {
                    	// It is important to put this in the main thread,
                        // because I had an error without it.
                        DispatchQueue.main.async {
                            User.saveToCoreData(users: users)
                        }
                    }
                }
            }
            
            if cdUsers.count > 0 {
            	// Users fetched from CoreData
                users = cdUsers
            } else {
            	// Fetching users from a JSON
                let url = URL(string: "https://www.hackingwithswift.com/samples/friendface.json")
                let request = URLRequest(url: url!)
                
                let semaphore = DispatchSemaphore(value: 0)
                
                URLSession.shared.dataTask(with: request) { data, response, error in
                    if let error = error {
                        fatalError("Network error: " + error.localizedDescription)
                    }
                    guard let response = response as? HTTPURLResponse else {
                        fatalError("Not a HTTP response")
                    }
                    guard response.statusCode >= 200, response.statusCode < 300 else {
                        fatalError("Invalid HTTP status code")
                    }
                    guard let data = data else {
                        fatalError("No HTTP data")
                    }
                    
                    if let decodedUsers = try? JSONDecoder().decode([User].self, from: data) {
                        users = decodedUsers
                        semaphore.signal()
                    }
                }.resume()
                
                _ = semaphore.wait(wallTimeout: .distantFuture)
            }
            
            return users
        }

While Peter's solution was to show a List directly from a FetchedResults, I wanted to have one array of users to pull from both JSON and CoreData. This is how I fetched from CoreData without the property wrapper which is normally seen in the tutorials:

    static func fetchCoreData() -> [User] {
            let moc = (UIApplication.shared.delegate as! AppDelegate).persistentContainer.viewContext
            let request = NSFetchRequest<CDUser>(entityName: "CDUser")
            request.returnsObjectsAsFaults = false
            var users = [User]()
            
            moc.performAndWait {
                let results: [CDUser] = try! request.execute()
                for user in results {
                    var friends = [Friend]()
                    for friend in user.friends!.allObjects as! [CDFriend] {
                        friends.append(Friend(id: friend.id!, name: friend.name!))
                    }
                    
                    users.append(User(id: user.id!, name: user.name!, age: user.age, company: user.company!, friends: friends))
                }
            }
            return users
        }

This is the most crucial part of my code because it took me two days to finally arrive at an answer on how to save a relative of an object:

    static func saveToCoreData(users: [User]) {
            let moc = (UIApplication.shared.delegate as! AppDelegate).persistentContainer.viewContext
            moc.performAndWait {
                users.forEach { user in
                    let cdUser = CDUser(context: moc)
                    cdUser.id = user.id
                    cdUser.name = user.name
                    cdUser.age = Int16(user.age)
                    cdUser.company = user.company
                    
                    user.friends.forEach { friend in
                        let cdFriend = CDFriend(context: moc)
                        cdFriend.id = friend.id
                        cdFriend.name = friend.name
                        // This line saved me in this challenge
                        cdFriend.user = cdUser
                    }
                }
            }
            
            if moc.hasChanges {
                try? moc.save()
            }
        }

The problem was that I couldn't see the friends of `cdUser` if I did it this way for each iteration:

    cdUser.friends.adding(cdFriend)

or

    cdUser.friends = friends // an array of CDFriend

I looked into many Stackoverflow inquiries regarding this, but I don't think there's a specific question about how to go about it or maybe that I didn't type the correct keyword, but then I did find one and it happens to be the solution I just showed you right now.
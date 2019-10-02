---
title: A Better EmptyView In An Empty CollectionView
image: assets/images/71823045_2511297908958002_3393944923223883776_n.jpg
categories: Tutorial Improvise
tags:
- ''
- featured
- collection
- view
- empty
- swift
- table
---

One of the most lovely codes I've found is to turn an empty view into something more informative than showing nothing. As you can see, I have two labels in place of my collection view. It's simple to do, and you'll be surprised for it. I copied most of the code from Taha Sönmez [post][taha] on Medium. I believe he explains this in great detail. Although if you're an intermediate Swift programmer, you might not need the explanation anymore. He did it on a tableview, while this code I'm going to show you is done on a collection view. There's only 3 lines of code difference.

```
// EmptyView.swift
extension UICollectionView {
    func setEmptyView(title: String, message: String) {
        let emptyView = UIView(frame: CGRect(x: self.center.x, y: self.center.y, width: self.bounds.size.width, height: self.bounds.size.height))
        let titleLabel = UILabel()
        let messageLabel = UILabel()
        titleLabel.translatesAutoresizingMaskIntoConstraints = false
        messageLabel.translatesAutoresizingMaskIntoConstraints = false
        titleLabel.textColor = UIColor.black
        titleLabel.font = UIFont(name: "HelveticaNeue-Bold", size: 18)
        messageLabel.textColor = UIColor.lightGray
        messageLabel.font = UIFont(name: "HelveticaNeue-Regular", size: 17)
        emptyView.addSubview(titleLabel)
        emptyView.addSubview(messageLabel)
        titleLabel.centerYAnchor.constraint(equalTo: emptyView.centerYAnchor).isActive = true
        titleLabel.centerXAnchor.constraint(equalTo: emptyView.centerXAnchor).isActive = true
        messageLabel.topAnchor.constraint(equalTo: titleLabel.bottomAnchor, constant: 20).isActive = true
        messageLabel.leftAnchor.constraint(equalTo: emptyView.leftAnchor, constant: 20).isActive = true
        messageLabel.rightAnchor.constraint(equalTo: emptyView.rightAnchor, constant: -20).isActive = true
        titleLabel.text = title
        messageLabel.text = message
        messageLabel.numberOfLines = 0
        messageLabel.textAlignment = .center
        // The only tricky part is here:
        self.backgroundView = emptyView
    }
    func restore() {
        self.backgroundView = nil
    }
}
```

After extending the UICollectionView, go back to the implementation of your collection view. Add this code in your numberOfItemsInSection:

```
func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        if orders?.count ?? 0 == 0 {
            collectionView.setEmptyView(title: "You don't have any orders yet.", message: "Your orders will be in here.")
        }
        return orders?.count ?? 0
    }
```


[taha]: https://medium.com/@mtssonmez/handle-empty-tableview-in-swift-4-ios-11-23635d108409

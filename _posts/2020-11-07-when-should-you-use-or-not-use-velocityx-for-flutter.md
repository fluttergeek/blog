---
date: 2020-11-07 11:17:40 +00:00
title: When Should You Use or Not Use VelocityX For Flutter
categories:
- VelocityX
- Flutter
- Tutorial
tags:
- shortcuts
- widgets
- velocityx.dev
- velocity_x
image: assets/images/3b2c7e80-1e2f-11eb-839b-80bbcec5a803.png

---
This is one of the coolest flutter packages I've come across. I've had a little bit of experimenting with SwiftUI before, and some of its syntax is now brought here in Flutter. Although, not entirely similar, but it does take away plenty of lines of codes.

This is not one of those state management packages, nor does it help with state management related activities. It just helps shorten the code so much and I think it's both a blessing and a curse.

One of my main usage of this package is with texts.

    "Photos"
      .text
      .size(20)
      .bold
      .color(Palette.orange)
      .make()
      .pSymmetric(h: 16),

It is hella seductive. **TextStyle** will be a thing of the past. I like how straightforward this looks. It's more readable, however, the lines may be more when **VS Code** formats it. I think I'm fine with this whole thing being a one liner.

### No TextStyle?

    Text("Hello")
    vs
    "Hello".text.make()

There are times when you should probably not use **VelocityX**, such as in the example above. It's obviously longer than the typical one when you don't use a **TextStyle** on your text.

### Columns, Rows, and Stacks

    @override
      Widget build(BuildContext context) {
        return Scaffold(
          backgroundColor: Palette.yellow,
          body: [
            [
              20.heightBox,
              _customAppBar(),
              20.heightBox,
              ProfilePhoto(),
              40.heightBox,

As you can see in this example, I'm using VelocityX. However, at first glance, I don't know if what I did here was a column, a row, or a stack. This code goes way lower, and if I need to know what I did, then I'll have to find it at the bottom. This is the main disadvantage of using VelocityX when you wrap a very long line of code into it. You see, this setup also won't show you comments whether which widget you had used, unlike the typical:

    Column(children:[
    ]) // Column

But it's not all bad. If you have something short to wrap, then you'll still see what's going on.

    Positioned(
      bottom: 0,
      child: [
        // Age
        (_.feeds[index].age.toString() + " y/o")
            .text
            .color(Palette.purple)
            .make(),
        " / ".text.color(Colors.black12).make(),
        // Breed
        _.feeds[index].breed.text.color(Palette.purple).make(),
        " / ".text.color(Colors.black12).make(),
        // Sex
        _.feeds[index].sex.text.color(Palette.purple).make(),
      ].row(alignment: MainAxisAlignment.spaceAround).p8().box.withDecoration(BoxDecoration(
          color: Palette.purple.withOpacity(.15),
          borderRadius: BorderRadius.only(
            bottomLeft: Radius.circular(25),
            bottomRight: Radius.circular(25),
          ))).make().h(50),
    )

Here, I've wrapped a list of text widgets inside a row. This whole code doesn't make me have to look further down to grasp what I used. And as you can see, it also takes a little bit of getting used to when you've placed all the attributes below. It's much like SwiftUI, really. Also why SwiftUI was daunting at first when I first tried, because I had to know several possible things to attach to a particular widget.

There are plenty of other ways VelocityX can be applied to, but I'm just showing you the common ones.
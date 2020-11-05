---
date: 2020-11-05 06:12:11 +00:00
title: Collapsible Header And A List With Pull To Refresh [Flutter]
categories:
- Tutorial
- Flutter
tags:
- CustomScrollView
- Slivers
- Flutter
- NestedScrollView
- pull to refresh
- refreshindicator
image: assets/images/image_2020-11-05_141724.png

---
**Disclaimer**: We will not tackle the design part of the image above. If you wish to see a speed code video of that, go here: [https://www.youtube.com/watch?v=tlj2oNJkbjo](https://www.youtube.com/watch?v=tlj2oNJkbjo "Flutter Mail App UI Design Concept - SpeedCode - Protorix Code")

If you're reading this, there's a good chance you might not know what slivers are. You've come to the right place.

Slivers are widgets that you can put inside **CustomScrollView** or inside **NestedScrollView**. It allows you to make your widgets scrollable together. For example, the picture above has two major components. You may want to hide the big blue part when you're scrolling the bigger bottom half part. That's what I mean by scrolling them together. You can put to lists and grids to scroll together if they are wrapped in a sliver, each of them.

<iframe width="560" height="315" src="https://www.youtube.com/embed/k2v3gxtMlDE" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

This here is a great tutorial if you want to start learning about **CustomScrollView**. But the thing about **CustomScrollView** is that you can't add a **RefreshIndicator** if you're just using a simple **ListView.builder**. Maybe you can if you wrapped it in a **SliverToBoxAdapter**. So, I just recently found out about the **NestedScrollView**, much like **CustomScrollView**, it allows you to add slivers as well, but the bottom part, which is the "body", allows you to create regular widgets. I mean non-slivers.

    	NestedScrollView(
          headerSliverBuilder: (BuildContext ctx, bool boxIsScrolled) {
            return <Widget>[
              SliverToBoxAdapter(
                child: BigBlueBox(),
              )
            ];
          },
          body: BiggerBottomList(),
        );
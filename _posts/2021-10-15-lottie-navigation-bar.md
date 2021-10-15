---
title: "Lottie UINavigationBar"
last_modified_at: 2021-10-15T19:42:43+03:00
categories:
  - Blog
tags:
  - Swift
  - Lottie
---

## Adding Lottie Animation to UINavigationBar

this function return UIBarButtonItem to be added to UINavigationBar items

``` swift
  func createBarButtonItem(animationName: String, action: Selector) -> UIBarButtonItem {
      let customAnimationView = AnimationView(name: animationName)
      customAnimationView.frame = CGRect(x: 0, y: 0, width: 50, height: 50)
      customAnimationView.loopMode = .loop
      customAnimationView.backgroundBehavior = .pauseAndRestore
      customAnimationView.addGestureRecognizer(UITapGestureRecognizer(target: self, action: action))
      
      return UIBarButtonItem(customView: customAnimationView)
  }
```
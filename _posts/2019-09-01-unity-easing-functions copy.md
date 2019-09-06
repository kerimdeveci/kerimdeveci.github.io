---
title: "Easing Functions in Unity"
last_modified_at: 2016-03-09T16:20:02-05:00
categories:
  - Blog
tags:
  - unity
  - math
  - animations
---


Easing (or interpolation) equations are mostly used in animations to change a component value in a defined period of time.
You can move objects, change their colors, scales, rotations and anything you want simply using easing equations.

This is an example changing objects movement:

![animation-referance](https://kerimdeveci.github.io/assets/images/Animation-References-easing-equations.gif)

This process is often named “Tweening” and today I’d like to discover what’s under the hood and write about how to create your personal “Tween” class all by yourself.

## HOW IT WORKS

Easing functions are useful to change a value from A to B in X time, based on a mathematical function’s graph. We’ll tweak the “percentage” parameter in the Lerp method, looking at mathematical functions that are defined in the range [0,1] (in math the square brackets mean “included” so the functions have to exist in 0 and 1 too). We choose the range [0,1] because in our code we’ll always pass a percentage as a parameter, which represents the current time of our animation. For example if 5 of 10 seconds passed we’re going to pass “5/10“, so “0.5“. It probably seems hard at the beginning if you’re not familiar with math and so on, but I’ll talk about it step by step.

## MATHEMATICAL FUNCTIONS

If you don’t know what mathematical functions are here’s a really quick explanation, parallel with coding functions/methods. If you already know about them and you can jump on the Lerp section.

In our code we can have a method named “f”:

```csharp
float f  ( float x) {

}
```
---
title: "Easing Functions in Unity"
last_modified_at: 2016-03-09T16:20:02-05:00
categories:
  - Blog
tags:
  - Post Formats
  - readability
  - standard
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
 return x+4;
}
```

In math , we have f(x) = x+4

This means that f(6) = 6+4 = 10, or f(0) = 0+4 = 4.
The same value is returned by our method written above, if we give the same parameter.

“f” is the name of the function, could be “g” “h” or whatever you want. Just like our method names.
X is a placeholder, and it’s replaced by a value that you give. Just like parameters/variables in our codes.
After the “=” we have an equation, that returns a value based on the “x” that we give, just like inside our method.

Also, f(x) = x+4 or y=x+4 are the same thing.

A function defines how a set of input values correspond to a set of output values. One input can return only one output.

### FUNCTION GRAPH

To display this input-output relation we can use a (2D) cartesian coordinate system. At each X (input) corresponds a Y (output).

In our case we’re always using the example f(x) = x+4.
We can calculate a few points, such as f(-4) = 0, f(-2) = 2, f(0) = 4, f(2) = 6, and later we can connect them.
On the “X” axis we go on the value “-4” and draw a point (y=0). Then we go on the X value “-2”, go upper of 2 units (this way we go on the Y value “2” ) and we draw another point. After drawing all these points we can see that this is is a linear function and its graph is:

![animation-referance](https://kerimdeveci.github.io/assets/images/linear-graph-percentage.png)

Of course, a computer draws all the points perfectly thanks to an algorithm, we can’t draw non-linear functions using this method (we’ll need derivatives, we’ll have to study the domain, the symmetries, the sign and so on), but that’s still useful to understand how drawing a function works without going deeper.

You can use any website that draws the graph of a function to follow up here, without having to calculate each point and more complex things. I’m using this **[website](https://rechneronline.de/function-graphs/)**. here.

Now that we know what mathematical functions are and how to draw and represent them, we can go forward with the “Lerp method”.

## LERP

The method named “Lerp” stands for “Linear interpolation” and has always 3 parameters, a “start value“, an “end value” and a “percentage” (our current progress).

It’s useful to tweak a value “from point A to point B” based on the percentage (progress) parameter that you give. If we want to go from 0 to 4 and give 0.5 as percentage value, we’ll get 2 from this method (our middle value). If we give 0 we’ll get the start value and if we give 1 we’ll get the end value.

```csharp
/// <summary>
/// Linearly interpolates a value between two floats
/// </summary>
/// <param name="start_value">Start value</param>
/// <param name="end_value">End value</param>
/// <param name="pct">Our progress or percentage. [0,1]</param>
/// <returns>Interpolated value between two floats</returns>
static float Lerp(float start_value, float end_value, float pct)
{
    return (start_value + (end_value - start_value) * pct);
}
```

In Unity the method Mathf.Lerp is already written and ready to use. Documentation about **[Mathf.Lerp.](https://docs.unity3d.com/ScriptReference/Mathf.Lerp.html)**  

Now that we know what mathematical functions are and how to draw and represent them, we can go forward with the “Lerp method”.

## LERP IMPLEMENTATION

We can implement Lerp on our custom structs in mainly two different ways (I’m using the Vector3 struct as an example):

```csharp
 //This is the same as Unity's "Vector3.Lerp"
public Vector3 Lerp(Vector3 start_value, Vector3 end_value, float t)
{
    t = Mathf.Clamp01(t); //assures that the given parameter "t" is between 0 and 1

    return new Vector3(
        start_value.x + (end_value.x - start_value.x) * t,
        start_value.y + (end_value.y - start_value.y) * t,
        start_value.z + (end_value.z - start_value.z) * t
        );
}
```

```csharp
//This is the same as Unity's Vector3.LerpUnclamped
public Vector3 LerpUnclamped(Vector3 start_value, Vector3 end_value, float t)
{
    return new Vector3(
        start_value.x + (end_value.x - start_value.x) * t,
        start_value.y + (end_value.y - start_value.y) * t,
        start_value.z + (end_value.z - start_value.z) * t
        );
}
```
Lerp each value individually, that’s the main logic! If you have a Vector2 you can simply change the type and remove the line where there is the “z” calculation. The same thing applies for Colors, Lights and whatever!

The main difference between the two is that the Lerp assures that your “t” is always between 0 and 1, while you need to think by yourself if you want to use LerpUnclamped.

As said before, the percentage value “t” is always between the range [0,1]. (0.00f means 0%, 0.50f means 50% and 1.00f means 100%).

Documentation😕Vector3.Lerp, Vector3.LerpUnclamped, Math.Clamp01.

You can see here my personal performance tests about Lerp implementation.

## ANIMATE OUR COMPONENT

Now that we know about Lerp, we’d need a short code to see our progress! I’ll add a ball that moves from (0,0) to (0,1) in 1 second, but you can change whatever value you want! Light intensity, rotation, scale, color [….]

An abstract fragment of our code could be like this (to let you understand the logic):

```csharp
float time = 0; //Current time or progress
float duration = 4; //Animation time

while(time<=duration) //inside this loop until the time expires
{
    object.position.y = Lerp(0, 1, time/duration); //Interpolates the object's "y" value from 0 to 1

    //Wait 1 millisecond, depends on your language

    time += 1; //Adds one millisecond to the elapsed time
}
```
```csharp
/// <summary>
/// Changes the object's position "y" value in X time 
/// </summary>
/// <param name="transform">The object's transform</param>
/// <param name="y_target">"Y" target coordinate</param>
/// <param name="duration">The duration of the interpolation</param>
/// <returns></returns>
public static IEnumerator ChangeObjectYPos(Transform transform, float y_target, float duration)
{
    float elapsed_time = 0; //Elapsed time
 
    Vector3 pos = transform.position; //Start object's position
 
    float y_start = pos.y; //Start "y" value
 
    while (elapsed_time <= duration) //Inside the loop until the time expires
    {
        pos.y = Mathf.Lerp(y_start, y_target, elapsed_time / duration); //Changes and interpolates the position's "y" value
 
        transform.position = pos;//Changes the object's position
 
        yield return null; //Waits/skips one frame
 
        elapsed_time += Time.deltaTime; //Adds to the elapsed time the amount of time needed to skip/wait one frame
    }
}
 
 
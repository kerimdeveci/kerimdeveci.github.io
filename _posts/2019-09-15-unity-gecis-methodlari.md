---
title: "Easing Functions in Unity"
last_modified_at: 2016-03-09T16:20:02-05:00
categories:
  - Blog
tags:
  - Unity
  - turkce
  - matematik
---

Gecis metodlari genellikle bir komponentin icindeki degeri belli bir zaman araliginda anime ederek desgistirmede kullanilir.
Bu metodlarla objeleri hareket ettirebilir, renklerini degistirebilir, scla degerini ve rotasyon degerlerini hafifleterek gecislerini saglayabilirsiniz.

Bu grafikte basitce butun denklemlerin karsiligini gorebilirsiniz.

![animation-referance](https://kerimdeveci.github.io/assets/images/Animation-References-easing-equations.gif)

bu islem genllikle “Tweening” adi verilir. bu yazimizda isin arka planininin nasil calistigini gorecegiz. boylelikle kendimize ait bir tween kutuphanesi olusturabiliriz.


## NASIL CALISIR

Bir A Noktasindan B noktasina X sure icinde gecisimizi saglayan matematik fonksiyonlaridir.

Lerp metodunun icindeki yuzdelik kismini degistirecegiz ve tam bu noktaya tween ozelligini getiren matematik fonksiyonlarini yazacagiz. Bu fonksiyonlar 0 ve 1 arasindaki tanimlanan denklemlere goz atacagiz. Cunku kodda surekli 0 ve 1 arasindaki degerleri yuzdelik dilim parametresi olarak atayacagiz. bildigimiz gibi Matematik'de [0,1] 0 ve 1 dahil olan degerler kumesini ifade eder. 0 ve 1 degerleri fonksiyonumuzda ikiside dahil olmali . Parametremize ornek olarak ornegin 10 saniyeden 5 saniyesi gectiyse parametremize 5/10 yani 0,5f degerini yazacagiz.


## MATHEMATICAL FUNCTIONS


f isimli metodumuz girilen x parametresinin 4 fazlasini dondurur. matematikde bunun karsiligi f(x) = x+4 dir.

```csharp
float f  ( float x) {
 return x+4;
}
```

yani f(5) = 5+4 = 9, veya f(0) = 0+4 = 4.

"f" fonksiyon ismidir. g veya h da olabilir. tipki metod isimlerimiz gibi yukaridaki kod orneginde fonksiyon ismimiz yine f kullandik. x ise degisken, kodda karsiligi aynen degiskenler/parametreler.

zaten metod dedigimiz matematikdeki fonksiyonlardan pekde bir farki olmayan sey. peki fonksiyon grafikleri? bunun icin Lise 3 bilgilerimizi tazelemeye ve ozel tanimli fonksiyonlar konusuna davet ediyorum sizi.

### FONKSIYON GRAFIKLERI

To display this input-output relation we can use a (2D) cartesian coordinate system. At each X (input) corresponds a Y (output).

Asagida y=x fonksiyonun grafigini goruyorsunuz. bu dogrusal bir fonksiyon. 

![animation-referance](https://kerimdeveci.github.io/assets/images/linear-graph-percentage.png)

Dogrusal (lineer) olmayan fonksiyon grafiklerini turev ile ciziyorduk hatirlarsaniz.

burada grafigi cizmek icin Geo Gebra Programini kullandim. Bu programin online versiyonu var . baska bir alternatif isterseniz ise **[bu siteye](https://rechneronline.de/function-graphs/)** basvurabilirsiniz.


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
 ```

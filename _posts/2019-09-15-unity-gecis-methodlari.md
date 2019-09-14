---
title: "Unity Gecis Fonksiyonlari"
last_modified_at: 2016-03-09T16:20:02-05:00
categories:
  - Blog
tags:
  - Unity
  - turkce
  - matematik
---

Gecis metodlari genellikle bir komponentin icindeki degeri belli bir zaman araliginda anime ederek degistirmede kullanilir.
Bu metodlarla objeleri hareket ettirebilir, renklerini degistirebilir, Scale degerini ve rotasyon degerlerini hafifleterek gecislerini saglayabilirsiniz. Bu islem genllikle “Tweening” adi veriliyor

simdi isin arka planininin nasil calistigini gorecegiz. Biraz matematik ile sinirsizi sayida guzel gecis efektleri yapabilir ve kendimize ait bir tween kutuphanesi olusturabiliriz.

Ya da ben hic ugrasamam bunlarla diyorsaniz DoTween i kullanabilirsiniz oldukca basit bir kullanimi var.

## NASIL CALISIR

Unity icindeki Lerp Methodunun son parametresi olan yuzdelik dilimi icerisinde bunu gerceklestirecegiz.

Peki Lerp nedir?

## LERP

Lerp methodu acilimi Linear interpolation yani dogrusal interpolasyon. Lineer interpolasyon, lineer polinomlar kullanarak, dataların bilindiği noktalardan yeni dataların üretilmesini sağlayan bir eğri uydurma metodudur.

4 adet parametre alan Lerp metodu basitce soyle calisir. Asagidaki ornektede gorulecegi uzere icine girilen start_value degeri ile end_value degeri arasindaki bir degeri daima 0 ile 1 arasinda olacak olan percentage degeri sayesinde bulmaya yarar. Yani percentage degeri ne kadar 0'a yaklasirsa dondurulen deger o kadar start_value'ya yaklasir. tam tersi deger ne kadar 1'e yaklasirsa dondurulen deger o kadar end_value'ya yaklasir. ) veya 1 degerlerini verdigimizde ise start_value veya end_value degerlerinin kendisini verir. Ornegin;

```csharp
Mathf.Lerp (0, 100, 0.5)
```
yazarsak fonksiyonumuz bize 0 ile 1 in tam ortasinda olan 0.5 degerini girdigimizden 0 ile 100 arasinda tam orta nokta olan 50 yi verecektir.

```csharp
Mathf.Lerp (10, 50, 0.7)
```

bu ornekte ise 0.7 sayisi 1 e daha yakin oldugundan dondurulen deger 50 ye daha yakin olacaktir. boylelikle fonksiyonumuz 10+(50-10)*0.7 = 38 sonucunu verektir.

```csharp
/// <summary>
/// Linearly interpolates a value between two floats
/// </summary>
/// <param name="start_value">Start value</param>
/// <param name="end_value">End value</param>
/// <param name="pct">Our progress or percentage. [0,1]</param>
/// <returns>Interpolated value between two floats</returns>
static float Lerp(float start_value, float end_value, float percentage)
{
    return (start_value + (end_value - start_value) * percentage);
}
```

Basitce soyle anlatalim sahnede bir objemiz var ve bunun simdiki X pozisyonu 10. Bunu baslangic degeri olarak ele alalim ve A degerimiz 15 olsun. Bu objemizin X ekseninde 50 konumuna karsilik gelmesini istiyoruz . ancak bu degere gelirken yavasca bir gecis yapmasini istiyoruz ve X degerini Mathf.Lerp ile Update fonksiyonu icinde cagirip degirtirisek basitce dogrusal olarak bir yavas gecis yakalabiliriz.

Konu ile ilgilili dokumantasyonu buraya birakalim. **[Mathf.Lerp.](https://docs.unity3d.com/ScriptReference/Mathf.Lerp.html)**  


Biz burada iste tam buradaki Lerp metodunun icindeki yuzdelik kismini degistirecegiz ve tam bu noktaya tween ozelligini getiren matematik fonksiyonlarini yazacagiz. 
Bu deger herzaman 0 ve 1 arasinda olmak zorunda oldugundan tanimlayacagimiz fonksiyonlarin tanim kumesi 0 ve 1 araliginda olmali. Cunku kodda surekli 0 ve 1 arasindaki degerleri yuzdelik dilim parametresi olarak atayacagiz. bildigimiz gibi Matematik'de [0,1] 0 ve 1 dahil olan degerler kumesini ifade eder. 0 ve 1 degerleri fonksiyonumuzda ikiside dahil olmali.


## MATEMATIKSEL FONKSIOYONLAR

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

Asagida y=x fonksiyonun grafigini goruyorsunuz. bu dogrusal bir fonksiyon. 

![animation-referance](https://kerimdeveci.github.io/assets/images/linear-graph-percentage.png)

Dogrusal (lineer) olmayan fonksiyon grafiklerini turev ile ciziyorduk hatirlarsaniz.

burada grafigi cizmek icin Geo Gebra Programini kullandim. Bu programin online versiyonu var . baska bir alternatif isterseniz ise **[bu siteye](https://rechneronline.de/function-graphs/)** basvurabilirsiniz.

Burada animasyonun nasil olabilecegini su sekilde gozlemleyebilirsiniz. Ben GeoGebra Kullandim. kullandigim dosyanin online haline buradan erisip kendiniz f fonksoyonuna istediginiz degeri vererek animasyon fonksiyonu olusturabilirsiniz.

[Easing-Functions](https://www.geogebra.org/m/nve6x5ze)

![ease-in](https://kerimdeveci.github.io/assets/images/ease-in^2.gif)
![ease-in](https://kerimdeveci.github.io/assets/images/ease-in^3.gif)
![ease-in](https://kerimdeveci.github.io/assets/images/ease-in^4.gif)
![ease-out](https://kerimdeveci.github.io/assets/images/ease-out.gif)
![ease-out](https://kerimdeveci.github.io/assets/images/ease-out2.gif)
![ease-out](https://kerimdeveci.github.io/assets/images/ease-out3.gif)

## LERP IMPLEMENTATION

Unity icinde Vektorlere de 2 cesit sekilde Lepr uygulayabiliriz.

```csharp
 //Unity nin  "Vector3.Lerp fonksiyonunu aynisi"
public Vector3 Lerp(Vector3 start_value, Vector3 end_value, float t)
{
    t = Mathf.Clamp01(t); //t degeri herzaman 0 ve 1 araliginda olmasini saglar

    return new Vector3(
        start_value.x + (end_value.x - start_value.x) * t,
        start_value.y + (end_value.y - start_value.y) * t,
        start_value.z + (end_value.z - start_value.z) * t
        );
}
```

```csharp
//Unity nin Vector3.LerpUnclamped fonksiyonunu aynisi
public Vector3 LerpUnclamped(Vector3 start_value, Vector3 end_value, float t)
{
    return new Vector3(
        start_value.x + (end_value.x - start_value.x) * t,
        start_value.y + (end_value.y - start_value.y) * t,
        start_value.z + (end_value.z - start_value.z) * t
        );
}
```
Temel mantik butun degerlere ayri ayri Lerp uygulamak. Bu Color classi icinde gecerli.

ikisinin arasindaki temel fark Lerp 0 ile 1 arasinda olmasini garantilerken Lerp.Unclamped garantilemez ve bu degeri kendimizin ayarlamasi beklenir.

Bahsettigimiz gibi percentage degeri olan t herzaman [0,1] araliginda olmali. (0.00f -> 0%,
0.50f -> 50%
1.00f -> 100%) anlamina gelir

## KOMPONENTLERI ANIME ETMEK

bu ornekte mantigini anlamak icin objemizin y pozisyonunu Lerp uyguluyoruz  


```csharp
float time = 0; //Simdiki zaman veya ilerleme oranimiz
float duration = 4; //toplama anime suresi

while(time<=duration) //dongunun icinde zaman gecinceye kadar beklememiz lazim
{
    object.position.y = Lerp(0, 1, time/duration); //objenin y degerini 0 dan 1 Interpole edecegiz

    //1 mili saniye bekle

    time += 1; //gecen zamana 1 milisaniye ekle
}
```

simdi mantigi anladiktan sonra Unity de Coroutine icinde calistiralim

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
    float elapsed_time = 0; //gecen zaman

    Vector3 pos = transform.position; //objenin baslangic pozisyonu

    float y_start = pos.y; //baslangic y degeri

    while (elapsed_time <= duration) //zaman doluncaya kadar dongude  kal
    {
        pos.y = Mathf.Lerp(y_start, y_target, elapsed_time / duration); //pozisyon y degerine Lerp islemi

        transform.position = pos;//objenin ppozisyonunu atiyoruz

        yield return null; //bir kare bekle

        elapsed_time += Time.deltaTime; //bir karenin bitmesi icin gerekli olan sureyi toplam gecen sureye ekle
    }
}
 ```

 ## ORNEKLER



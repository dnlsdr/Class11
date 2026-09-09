# 1. Típusok osztályozása

A Java típusrendszere egy erősen típusos rendszer, amely két fő kategóriára osztja a változókat:
* primitív típusok
* referencia típusok

A primitív típusok olyan egyszerű adatokat reprezentálnak, mint a számok vagy a logikai értékek. Egy primitív változó értéke maga a tárolt információ.
A referencia típusok olyan objektumokat reprezentálnak. (Az OOP-ról későbbiekben részletesen lesz szó.) Egy referencia változóban a tárolt érték maga a referencia (kb cím) a hivatkozott objektumra.

* Primitív típusok esetében, ha egyiket egyenlővé tesszük a másikkal, akkor érték szerint lesznek egyenlők, de különböző változók különböző memóriaterületen. 
  ```java
  a = b;



* Ugyanez esetben referencia típusok esetén nem lesz új változó, csak hivatkozás (`a`) (referencia) az eredetire (`b`).


---

### 1.1. Primitív típusok

Primitív típusból 8 van mindössze, ezek a következők

* Előjeles egészek:
* `byte`, `short`, `int`, `long` (8, 16, 32, 64 bit)


* Lebegőpontos számok:
* `float`, `double` (32, 64 bit)


* Unicode karakter:
* `char` (16 bit)


* Logikai:
* `boolean` (1 bit)



#### Használatuk

```java
int a;
int b = 1;

```

---

### 1.2. Primitív típusok kód

Próbáljuk ki a következő kódot

```java
int i = 10;
float f = 10.0f;
long l = 0xFA;
long octal = 071;

boolean b = (f == i);

System.out.println(l);
System.out.println(b);

```

Figyeljük meg, hogy mi a boolean értéke, illetve hogy a nyolcas számrendszerben megadott számként értelmezi a 0-val kezdődő számsort.

---

### 1.3. Referencia típusok - tömbök

Minden más (ami nem primitív) az referencia típus.
A tömbök is speciális referencia típusok, az indexelés nullával kezdődik

```java
int[] i = new int[10];

```

A tömb tehát egy tároló, amelyben azonos típusú elemek sorozata helyezhető el. Létrehozás után a tömb mérete nem változtatható meg.

#### Példa

```java
int[] anArray;
anArray = new int[10];
anArray[0] = 100;
anArray[1] = 200;
anArray[2] = 300;
anArray[3] = 400;
anArray[4] = 500;
anArray[5] = 600;
anArray[6] = 700;
anArray[7] = 800;
anArray[8] = 900;
anArray[9] = 1000;

System.out.println("Element at index 0: " + anArray[0]);
System.out.println("Element at index 1: " + anArray[1]);
System.out.println("Element at index 2: " + anArray[2]);
System.out.println("Element at index 3: " + anArray[3]);
System.out.println("Element at index 4: " + anArray[4]);
System.out.println("Element at index 5: " + anArray[5]);
System.out.println("Element at index 6: " + anArray[6]);
System.out.println("Element at index 7: " + anArray[7]);
System.out.println("Element at index 8: " + anArray[8]);
System.out.println("Element at index 9: " + anArray[9]);

```

Próbáljuk ki, hogy mi történik, ha nem inicializálunk egy értéket!
Próbáljuk ki, hogy mi történik, ha nem hozzuk létre a tömböt!

#### Inicializálás egyszerűbben

```java
int[] anArray = { 100, 200, 300, 400, 500, 600, 700, 800, 900, 1000 };

```

---

### 1.4. Referencia típusok - párok

A primitíveknek létezik előre definiált referencia változata

* Ez általában nagy kezdőbetűvel írandó, például:
```java
Double - double

```


* De van, ami kicsit más:
```java
Character - char, Integer - int

```


* Karakterlánc - Unicode alapú
* Fontos, hogy ennek nincs primitív párja


```java
String

```



#### Példák

```java
Double a = 5.0;
String s = "alma";  // literállal történik az értékadás
String s2 = new String("alma"); //new kulcsszó használata, új objektumpéldány jön létre

```

---

### 1.5. Referencia típusok példa

Nézzük az alábbi példát!

```java
String s = new String("Valami string ") + "példa";
Double d = 1d/3;

System.out.println(s);
System.out.println(d);

```

Figyeljük meg, hogy ha a d változó értékadásakor nem jelezzük, hogy az 1 konstans literál az double, akkor egész értékként veszi figyelembe a Java és az egészekre vonatkozó operátort használja. (Emlékezzünk, hogy a típus az az értékek és rajtuk végezhető műveletek egysége.)

---

### 1.6. Csomagoló osztályok

A csomagoló osztályok (primitívek osztály párjai) használata során nem kell ügyelnünk arra, hogy ez osztály.

* A Java automatikusan kicsomagolja és becsomagolja nekünk
* auto-boxing
* auto-unboxing


* Azaz értékül adhatunk egy Integert egy intnek és fordítva

A beépített csomagoló osztályok, amik primitív típusokat reprezentálnak objektumként, értékei nem megváltoztathatóak 

* immutable osztályok

Amikor látszólag megváltoztatjuk, új objektumpéldány jön létre az új értékkel, lecserélve az eredeti példányt

```java
Double d = 5.0;
d = 6;

```

---

### 1.7. Csomagoló osztályok - Próbáljuk ki

Ezt ki lehet egyszerűen próbálni az alábbi kóddal:

```java
double d1 = 10d;
Double d2 = d1;
Double d3 = d2;

d1 = 20d;
System.out.println((d1 + " " + d2 +" " + d3));
d2 = 40d;
System.out.println((d1 + " " + d2 +" " + d3));

```

A `d1` változót becsomagoljuk, majd az objektumra egy második referenciát is állítunk.
Amint a d1 megváltozik az nincsne hatással a két objektum állapotára, valamint az immutable objektum megváltoztatása új példányt eredményez és három adat lesz a memóriában.

Változókat a memóriában meg tudjuk nézni, ha debug módban indítjuk el a programot.

---

# 2. Változók használata

Előzőleg láthattuk, hogy hogyan deklarálhatunk változókat és hogyan adhatunk értékeket nekik.
További lehetőség Java esetén a var kulcsszó használata, lokális változó esetén amennyiben az értékadás során a változó típusa egyértelműen meghatározható úgy nem kell megadni a típust.

* Type inferencing a neve
* Java 10 óta lehetséges
* Hasznos hosszabb típusmegadás esetén


```java
var d = 5.0;
var ll = new LinkedList<TreeMap<String, String>>();

```

---

### 2.1. Típuskonverzió

Kétféle konverziós módszer

* Implicit – ekkor automatikusan megtörténik a konverzió
* Például int a = 5; double b = a;


* Explicit – ekkor saját magunk határozzuk meg a konverziót
* Például double a = 5.0; int b = (int) a;



#### Példa

```java
(double) 11 / 3 = 3.666;
11 / 3 = 3;

```

A konverzió a csomagoló és a primitív között automatikus

```

```

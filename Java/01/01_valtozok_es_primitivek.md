### 1. Változók és Primitív típusok

Pythonban elég volt leírni, hogy `kor = 25`. A Python kitalálta, hogy ez egy szám. Javában neked kell megmondanod, ráadásul a memóriatakarékosság miatt többféle szám is létezik (ezek a primitív típusok).

* **Egész számok:** `byte`, `short`, `int`, `long`. (Leggyakrabban az `int`-et használjuk).
* **Tört számok:** `float`, `double`. (Alapértelmezetten a `double`-t használjuk).
* **Logikai:** `boolean` (Pythonban `True/False`, Javában kisbetűs `true/false`).
* **Karakter:** `char` (Egyetlen betű, szigorúan szimpla idézőjelben: `'A'`).

**Példa:**

```java
// Python: kor = 25
int kor = 25; 

// Python: aktiv = True
boolean aktiv = true; 

```

### 2. Operátorok

A matematikai műveletek nagyon hasonlítanak a Pythonra, de van pár fontos különbség:

* **Aritmetikai (`+`, `-`, `*`, `/`, `%`):** Javában két egész szám osztása egész számot ad. (`5 / 2` eredménye `2`, nem `2.5`).
* **Értékadó (`=`, `+=`, `-=`):** Ugyanúgy működnek, mint Pythonban.
* **Egyoperandusú (`++`, `--`):** Pythonban ilyenek nincsenek. A `kor++` pontosan ugyanazt jelenti, mint a `kor += 1`.
* **Relációs (`==`, `!=`, `<`, `>`):** Teljesen megegyezik a Pythonnal.

### 3. Szövegek tárolása (Strings)

Javában a szöveg nem primitív típus, hanem egy objektum (ezért kezdődik nagybetűvel). Szigorúan **dupla idézőjelet** kell használni hozzá (a szimpla a `char` kiváltsága).

**Példa:**

```java
// Python: nev = "Anna" vagy nev = 'Anna'
String nev = "Anna";

```

### 4. Típuskonverzió (Casting)

Pythonban függvényeket használtál erre: `int(3.14)`. Javában zárójelbe tesszük az új típust a változó elé.

* **Implicit (automatikus):** Ha kisebb típust teszel nagyobba (pl. `int` -> `double`), a Java megcsinálja magától.
* **Explicit (kézi):** Ha nagyobbat teszel kisebbe, adatvesztés lehet (pl. a tizedesjegyek levágása), ezért ezt neked kell csinálni (kasztolás).

**Példa:**

```java
double pi = 3.14;
int egeszPi = (int) pi; // Eredmény: 3 (a tizedesjegyek elvesznek)

```

### 5. Tömbök (Arrays)

A Python listája (`[1, 2, 3]`) dinamikus: bármit beletehetsz, és bármikor növelheted a méretét.
A Java tömbje viszont **merev**: létrehozáskor meg kell mondani a típusát és a pontos méretét, ami utána soha nem változhat meg!
#### 5.1 Létrehozás és fix méret

Amikor Javában létrehozol egy tömböt, a memóriában lefoglalódik egy fix méretű hely. Ha betelt, nem tudsz hozzáadni új elemet (nincs `.append()` metodus, mint Pythonban).

**Pythonban:**

```python
# Csinálunk egy üres listát, majd pakolunk bele
szamok = []
szamok.append(10)
szamok.append(20)

```

**Javában:**

```java
// Előre meg kell mondanunk, hogy 3 darab egész számot (int) fog tárolni.
int[] szamok = new int[3]; 

szamok[0] = 10;
szamok[1] = 20;
szamok[2] = 30;

// Ha ezt megpróbálod: szamok[3] = 40;
// A program összeomlik egy "ArrayIndexOutOfBoundsException" hibával, 
// mert a 4. rekesz (3-as index) már nem létezik!

```

#### 5.2 Elemek lekérdezése és a tömb hossza

Pythonban a `len()` függvényt használtad a méret lekérdezésére. Javában a tömböknek van egy beépített tulajdonsága (property), amit `length`-nek hívnak.

**Pythonban:**

```python
hossz = len(szamok)
elso_elem = szamok[0]

```

**Javában:**

```java
int hossz = szamok.length; // Figyeld meg: nincsenek zárójelek a length után!
int elsoElem = szamok[0];

```

#### 5.3 Ciklusok és tömbök (Iteráció)

A tömbök bejárására két gyakori módszert is használunk Javában. Az egyik nagyon hasonlít a Pythonra, a másik egy klasszikus, indexes megközelítés.

**Pythonban (for-each stílus):**

```python
nevek = ["Anna", "Béla", "Cecil"]
for nev in nevek:
    print(nev)

```

**Javában ugyanez (For-each ciklus):**

```java
String[] nevek = {"Anna", "Béla", "Cecil"};

// Olvasd így: "Minden String típusú 'nev' elemre a 'nevek' tömbben..."
for (String nev : nevek) {
    System.out.println(nev);
}

```

**Javában klasszikus indexelt for ciklussal:**

```java
String[] nevek = {"Anna", "Béla", "Cecil"};

// Létrehozunk egy 'i' változót 0-tól kezdve; megyünk amíg el nem érjük a tömb hosszát; 
// és minden lépésben növeljük az 'i'-t eggyel (i++).
for (int i = 0; i < nevek.length; i++) {
    System.out.println("A(z) " + i + ". indexű elem: " + nevek[i]);
}

```

#### Mi van, ha mégis Python-szerű listát akarok?

Mivel a fix méretű tömbök a való életben sokszor rugalmatlanok, a Javában is létezik dinamikus, növelhető lista, amit `ArrayList`-nek hívnak, de erről majd később.

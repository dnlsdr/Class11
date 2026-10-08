**Palindrom-ellenőrző és Szövegcenzúrázó**

Készíts egy szövegfeldolgozó segédosztályt, amely két független feladatot lát el: megállapítja egy szóról, hogy oda-vissza ugyanaz-e (palindrom), illetve kicsillagozza a megadott tiltott szavakat egy mondatból.

**Lépések:**

**1. Az osztály létrehozása**

* Hozz létre egy `TextProcessor` nevű osztályt!

**2. A palindrom vizsgáló metódus (`isPalindrome`)**

* Készíts egy `public static boolean isPalindrome(String word)` metódust!
* **Bemenet:** Egy tetszőleges szó (pl. `"Radar"`).
* **Feladat:** Döntse el, hogy a szó visszafelé olvasva is megegyezik-e önmagával.
* **Szabályok és tippek a megvalósításhoz:**
* A kis- és nagybetűk ne számítsanak (az `"Anna"` és az `"anna"` is számítson palindromnak)! Első lépésként alakítsd a bemenetet kisbetűssé!
* A szó megfordításához hozz létre egy `StringBuilder` objektumot a kisbetűs szóból, és használd a `.reverse()` metódust!
* Az eredeti kisbetűs szót és a megfordított szót az `.equals()` metódussal hasonlítsd össze (ne feledd a `StringBuilder`-t visszaalakítani `String`-gé ehhez)!


* **Kimenet:** `true`, ha a szó palindrom, különben `false`.

**3. A cenzúrázó metódus (`censorText`)**

* Készíts egy `public static String censorText(String text, String[] forbiddenWords)` metódust!
* **Bemenetek:** Egy teljes mondat (`text`), és egy tiltott szavakat tartalmazó szöveges tömb (`forbiddenWords`).
* **Feladat:** Keresse meg a tömbben lévő összes tiltott szót a mondatban, és mindegyiket cserélje le három csillagra (`"***"`).
* **Szabályok és tippek a megvalósításhoz:**
  - Használj egy `for` ciklust, ami végiglépked a tiltott szavak tömbjén!
  - A szövegcseréhez használd a `String` osztály `.replace()` metódusát!
  - *Fontos:* Mivel a `String` megváltoztathatatlan (immutable), a `.replace()` nem az eredeti változót írja felül, hanem egy újat hoz létre. Gondoskodj róla, hogy a lecserélt értéket minden lépésben visszamentsd a `text` változóba (pl. `text = text.replace(...)`)!


* **Kimenet:** A kész, cenzúrázott mondat.

**4. Tesztelés a `Main` osztályban**
* A `Main` osztály `main` metódusában teszteld a két funkciót!
* **Palindrom teszt:** Hozz létre egy tömböt tesztszavakkal (pl. `{"Radar", "Java", "Görög", "Alma"}`). Egy ciklus segítségével hívd meg mindegyikre az `isPalindrome` metódust, és írasd ki az eredményt (pl. `"Radar palindrom? true"`).
* **Cenzúra teszt:** Hozz létre egy mondatot (pl. `"A buta macska felmászott a fára, mert megijedt a kutyától."`) és egy tiltott szavakat tartalmazó tömböt (pl. `{"buta", "kutya"}`). Hívd meg a `censorText` metódust, majd írasd ki az eredeti és a cenzúrázott mondatot is!
* kimenet:
```
--- Palindrom teszt ---
Radar palindrom? true
Java palindrom? false
Görög palindrom? true
Alma palindrom? false

--- Cenzúra teszt ---
Eredeti: A buta macska felmászott a fára, mert megijedt a kutyától.
Cenzúrázott: A *** macska felmászott a fára, mert megijedt a kutyától.
```

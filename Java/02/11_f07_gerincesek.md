### Feladat: Gerincesek Rendszertana és Mozgásformái (Öröklődés + Interfészek)

**Cél:** Az absztrakt osztályok, a többszintű öröklődés (`extends`), a láthatósági szintek (`protected`, `private`), a `final` módosítószó, valamint a képességeket leíró interfészek (`implements`) gyakorlása.

#### 1. Az absztrakt ősosztály: `Gerinces`

* Hozz létre egy `Gerinces` osztályt, amelyet nem lehet példányosítani!
* **Adattagok és láthatóság:**
   - Minden állatnak legyen neve, amelyet a konstruktorban kap meg, és futás közben nem lehet átkeresztelni (`final`).
   - Tárold el a csigolyák számát és a lábak számát (a porcos halaktól most tekintsünk el).
   - Mindhárom tulajdonság legyen lekérdezhető getter metódusokkal!


* **Műveletek:**
  - Legyen egy `halad()` metódusa, amely kiírja az állat haladásának módját.
  - Írd felül a `toString()` metódust úgy, hogy kiírja az állat nevét, osztályát, a csigolyái és a lábai számát.



#### 2. Képességek leválasztása interfészekkel

Mivel nem minden madár repül (pl. a pingvin), és nem csak a halak tudnak úszni, a mozgásformákat ne kényszerítsük bele mereven a rendszertani öröklődési fába! Hozz létre három interfészt:

* `Uszo`: egy `void uszik();` metódussal.
* `Repulo`: egy `void repul();` metódussal.
* `Futo`: egy `void fut();` metódussal.

#### 3. A gerincesek öt fő osztálya

Egyszerűsítsük le a biológiai nevezéktant, és hozd létre az alábbi példányosítható osztályokat a `Gerinces` leszármazottaiként:

* **`Hal`:**
  - Tárold el és tedd lekérdezhetővé az uszonyok számát. A lábak száma legyen mindig `0`!
  - Implementálja az `Uszo` interfészt (`uszik()`), és a `halad()` metódusa ezt hívja meg!


* **`Keteltu`:**
  - Tárold el és tedd lekérdezhetővé, hogy megeszik-e a franciák (`boolean`). A `toString()` metódus ezt egész mondatban jelenítse meg!
  - Implementálja az `Uszo` és a `Futo` interfészt is (`uszik()` és `fut()`), a `halad()` pedig mindkettőt hajtsa végre!


* **`Hullo`:**
  - Tárold el és tedd lekérdezhetővé, hogy mérgező-e (`boolean`). A `toString()` metódus ezt is egész mondatban írja ki!
  - A `halad()` metódusa írja ki, hogy csendesen siklik.


* **`Madar`:**
  - Tárold el és tedd lekérdezhetővé a szárnyak számát is.
  - Alapértelmezés szerint a `halad()` metódusa írja ki, hogy a két lábán lépeget (mivel nem minden madár tud repülni!).


* **`Emlos`:**
  - Tárold el és tedd lekérdezhetővé a fogak számát.
  - A `halad()` metódusa írja ki, hogy mászik.



#### 4. Speciális leszármazottak (Madarak és Emlősök)

* **Madarak alosztályai (mindkettő legyen végleges – `final`):**
  - **`Sas`:** A `Madar` osztályból származik (2 láb, 2 szárny), és implementálja a `Repulo` interfészt!
  - **`Pingvin`:** Szintén a `Madar` osztályból származik (2 láb, 2 szárny), de a `Repulo` helyett az `Uszo` interfészt implementálja! A `halad()` metódusa írja ki, hogy a szárazföldön totyog, a vízben pedig úszik (`uszik()`).


* **Emlősök alosztályai (mindkettő legyen végleges – `final`):**
  - **`Kutya`:** Az `Emlos` osztályból származik. Tárold el, hogy ugat-e (`boolean`), és írj hozzá egy `harapE()` függvényt, ami ez alapján dönt (a közmondás szerint: amelyik ugat, az nem harap). A `halad()` metódusa írja ki, hogy farokcsóválva szalad.
  - **`Macska`:** Az `Emlos` osztályból származik. Tárold el szövegként a szőre színét. A `halad()` metódusa írja ki, hogy sündörög.



#### 5. Demonstrációs program (`Main` osztály)

Írj egy tesztprogramot, amely bemutatja:

1. **A rendszertani polimorfizmust:** tárold az összes létrehozott példányt egy közös `Gerinces[]` tömbben, és egy ciklussal írasd ki mindegyik adatait (`toString()`) és haladási módját (`halad()`).
2. **A képesség alapú polimorfizmust:** hozz létre külön egy `Uszo[]` tömböt (amelybe bekerül a `Hal`, a `Keteltu` és a `Pingvin` is!) és egy `Repulo[]` tömböt, majd hívd meg a specifikus metódusaikat (`uszik()`, `repul()`)!

---

#### Extra / Szorgalmi feladat: A kivételek az emlősök között

Bővítsd ki az `Emlos` hierarchiát két további, végleges (`final`) osztállyal, amelyek megmutatják, miért előnyös az interfészek használata a merev öröklődéssel szemben:

* **`Denever`:** Az `Emlos` osztályból származik (vannak fogai, csigolyái), de a többi emlőssel ellentétben implementálja a `Repulo` interfészt (`repul()`), és a `halad()` metódusa ezt hívja meg! Vedd fel a `Main` osztály `Repulo[]` tömbjébe is a `Sas` mellé!
* **`Balna`:** Szintén az `Emlos` osztályból származik, lábainak száma mindig `0` (akárcsak a halaknál!), és implementálja az `Uszo` interfészt (`uszik()`). Vedd fel a `Main` osztály `Uszo[]` tömbjébe is a `Hal`, a `Keteltu` és a `Pingvin` mellé!

### Feladat: Geometriai Síkidomok Hierarchiája

**Cél:** Az absztrakt osztályok, a többszintű öröklődési láncok, a láthatósági szintek (`protected`), a polimorfizmus, valamint a kód-újrafelhasználás (metódusok minimalizálása specializáció során) gyakorlása.
#### 0. Felépítés:
```
Sikidom (absztrakt; protected kerulet, terulet; kerulet(), terulet(), toString(); protected abstract keruletSzamit(), teruletSzamit())
├── Kor             (sugar; @Override keruletSzamit(), teruletSzamit(), toString())
└── Trapez          (protected a, b, c, d; @Override keruletSzamit(), teruletSzamit(), toString())
    └── Paralelogramma  (protected szog; super(a, b, a, b); @Override teruletSzamit(), toString())
        └── Teglalap    (super(a, b, 90.0); @Override toString())
            └── Negyzet (super(a, a); @Override toString())
```
#### 1. Az ősosztály: `Sikidom`

- Készítsd el a `Sikidom` absztrakt osztályt!
- **Tulajdonságai (mezői):** `kerulet` és `terulet` (`double` típusúak, a leszármazottak számára elérhető láthatósággal).
- **Absztrakt műveletei:** `keruletSzamit()` és `teruletSzamit()` – legyenek csak a leszármazottak számára elérhetőek (`protected`).
- **Publikus műveletei:** `kerulet()`, `terulet()`, `toString()`.
- **Tervezési szempont – Mikor történjen a számítás?**
  - A `kerulet()` és `terulet()` metódusok hívják meg a `keruletSzamit()` és `teruletSzamit()` függvényeket, tárolják el az eredményt a megfelelő mezőben, majd adják vissza azt!
  - *Fontos:* **Ne** az ősosztály konstruktorában hívd meg a `keruletSzamit()` és `teruletSzamit()` metódusokat, mert példányosításkor az ős konstruktora fut le előbb – ekkor a leszármazott oldalhosszai vagy a sugár még `0.0` értéken állnak!
- A `toString()` írja ki az alakzat kerületét és területét.

#### 2. A `Kor` osztály

- A `Sikidom` közvetlen leszármazottja, saját mezője a `sugar` (`double`).
- Készíts hozzá egyparaméteres konstruktort!
- Valósítsd meg a `keruletSzamit()` ( $2 \cdot r \cdot \pi$ ) és a `teruletSzamit()` ( $r^2 \cdot \pi$ ) metódusokat!
- A `toString()` írja le az osztály állapotát: az alakzat nevét ("Kör"), a sugarat, valamint (az ős `toString()` metódusát is felhasználva) a kerületet és a területet.

#### 3. A `Trapez` osztály

- A `Sikidom` másik közvetlen leszármazottja, 4 paraméteres konstruktorral.
- **Mezői:** `a, b, c, d` oldalak (mivel leszármazottai lesznek, válaszd a legmegfelelőbb láthatósági szintet!).
- **Képletek:**
  - A kerület az oldalak összege: $a + b + c + d$
  - A területhez feltételezzük, hogy $a$ és $c$ a két párhuzamos alap, $b$ és $d$ pedig a két szár. Ekkor a terület (ha $a \neq c$ ):

$$T = \frac{a + c}{\lvert a - c \rvert} \cdot \sqrt{(s - a)(s - c)(s - a - b)(s - a - d)}$$

  ahol $s = \frac{a + b + c + d}{2}$ a félkerület.
- A `toString()` írja ki az alakzat nevét, a 4 oldalt, valamint a kerületet és a területet.

#### 4. A `Paralelogramma` osztály

- A `Trapez` leszármazottja (speciális trapéz, amelynek szemközti oldalai egyenlőek: $c = a$ és $d = b$ ).
- **Bővítés:** Vegyen fel egy `szog` mezőt (fokban megadva), és legyen 3 paraméteres konstruktora (`a, b, szog`), amely a `super(...)` segítségével inicializálja az őst!
- **Tervezési szempont – Kódminimalizálás:**
  - Gondold át: szükséges-e felüldefiniálni a `keruletSzamit()` metódust, vagy a trapéz képlete erre is tökéletesen működik?
  - A `teruletSzamit()` metódust definiáld felül a szög segítségével: $T = a \cdot b \cdot \sin(\text{szög})$ (figyelj arra, hogy a `Math.sin()` radiánt vár, így a `Math.toRadians()` függvénnyel át kell váltani a fokot!).
- Írd meg a saját `toString()` metódusát is!

#### 5. A `Teglalap` és `Negyzet` osztályok

- **`Teglalap`:** A `Paralelogramma` gyermeke, 2 paraméteres konstruktorral (`a, b`).
- **`Negyzet`:** A `Teglalap` gyermeke, 1 paraméteres konstruktorral (`a`).
- **Tervezési szempont – Maximális öröklés:**
  - Minimalizáld a megírandó metódusokat! Használd ki, hogy a téglalap egy $90^\circ$ -os szögű paralelogramma, a négyzet pedig egy olyan téglalap, amelynek két szomszédos oldala egyenlő. Így egyik osztályban sem kell újraírni sem a `keruletSzamit()`, sem a `teruletSzamit()` metódust – elég a megfelelő `super(...)` konstruktorhívás és a `toString()` felüldefiniálása!


#### 6. Tesztelés (`Main` osztály)

- Hozz létre egy `Sikidom[]` tömböt, helyezz el benne mindegyik osztályból egy-egy példányt, majd egy ciklussal írasd ki az összes alakzat adatait a konzolra!
- *Megjegyzés:* Ez a **polimorfizmus** (többalakúság) gyakorlati megvalósítása – a `Sikidom` típusú referencián keresztül kezelünk különböző konkrét leszármazott objektumokat, és futási időben (dinamikus kötéssel) mindig az adott objektum saját, felüldefiniált `toString()`, `keruletSzamit()` és `teruletSzamit()` metódusa hívódik meg.

```
Kör (sugár: 5.00) -> kerület: 31.42, terület: 78.54
Trapéz (a=10.00, b=5.00, c=4.00, d=5.00) -> kerület: 24.00, terület: 28.00
Paralelogramma (a=6.00, b=4.00, szög=30.0°) -> kerület: 20.00, terület: 12.00
Téglalap (a=5.00, b=8.00) -> kerület: 26.00, terület: 40.00
Négyzet (a=4.00) -> kerület: 16.00, terület: 16.00
```


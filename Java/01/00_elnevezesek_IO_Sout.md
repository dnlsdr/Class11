## `IO` vs. `System.out` a Javában

A Java legújabb verzióiban megjelent `IO` modul pontosan azért jött létre, hogy a Pythonhoz hasonló egyszerűséget hozzon a kezdők számára.

**Hasonlóságok:**

* Mindkettő a konzollal való kommunikációt (szöveg kiírását) szolgálja.
* Mindkettő ugyanúgy használható a `println()` (kiírás és soremelés) és `print()` (kiírás soremelés nélkül) metódusokkal.

**Különbségek:**

* **IO rövidebb** A `System.out.println()` a klasszikus megoldás, ahol a `System` osztály `out` objektumát hívjuk. Az `IO.println()` egy modern, lerövidített segédmetódus.
* **IO-ba beolvasás be van építve:** A `System.out` csak kiír. A beolvasáshoz a `System.in`-t be kellett csomagolni egy `Scanner`-be. Az új `IO` osztály viszont tud beolvasni is az `IO.readln("Írj be valamit: ")` paranccsal. Ez szinte egy az egyben a Python `input()` függvényének felel meg!
* az új `IO` osztály **nem** tud mindent, amit a `System.out`:
  - **`System.out`**: Ez egy nagyon régi és robusztus `PrintStream` objektum. Rengeteg metódusa van, köztük a C nyelvből örökölt `printf()` (formázott kiíratás), a `format()`, vagy a `flush()`.
  - **`IO` (az új, modern megközelítés)**: Ezt kifejezetten kezdőknek, az oktatás megkönnyítésére találták ki (a Python mintájára). Szándékosan minimalista. Csak a legszükségesebb alapokat tartalmazza: `print()`, `println()`, és a beolvasáshoz a `readln()`. **Az `IO`-ban nincs `printf`.** Ha bonyolultabb formázásra van szükség, a modern Javában inkább új technológiákat (pl. String formázást) vagy a klasszikus `System.out.printf()`-et használjuk.
---

## Elnevezési konvenciók: Java vs. Python

A Python a szavakat aláhúzással választja el (`snake_case`), míg a Java a "tevepúpos" megoldást (`camelCase`) preferálja, ahol a szavakat szóköz nélkül egybeírjuk, és a második szótól kezdve minden új szó első betűje nagy.

| Elem | Java konvenció | Python konvenció | Példa (Java) | Példa (Python) |
| --- | --- | --- | --- | --- |
| **Osztályok** | PascalCase | PascalCase | `FoProgram`, `Kutya` | `FoProgram`, `Kutya` |
| **Változók / Mezők** | camelCase | snake_case | `beolvasottSzam` | `beolvasott_szam` |
| **Függvények (Metódusok)** | camelCase | snake_case | `atlagotSzamol()` | `atlagot_szamol()` |
| **Konstansok (`final`)** | UPPER_SNAKE | UPPER_SNAKE | `MAX_MERET` | `MAX_MERET` |
| **Csomagok (Mappák)** | csupa kisbetű | kisbetű_aláhúzás | `statisztika` | `statisztika_modul` |

**Fontos Java szabályok a Python után:**

* **Metódusok és változók:** Mindig kisbetűvel kezdődnek! (pl. `nev`, `getAtlag()`).
* **Fájlnevek:** Javában a fájlneveknek szigorúan meg kell egyezniük a bennük lévő publikus osztály nevével (tehát nagybetűvel kezdődnek, pl. `Kutya.java`). Pythonban a modulok fájlnevei többnyire kisbetűsek.



---

### Kód összehasonlítás: Elnevezési stílusok

Nézzük meg ugyanazt a logikát egymás mellett, hogy vizuálisan is rögzüljenek a konvenciók! Figyeld meg a kis- és nagybetűk (snake_case vs. camelCase) használatát!

**Python (snake_case)**

```python
class Kutya:
    # Konstans (csupa nagybetű, aláhúzás)
    MAX_ELETKOR = 15

    def __init__(self, nev):
        # Változó / Mező (kisbetű, aláhúzás)
        self.kutya_neve = nev

    # Függvény (kisbetű, aláhúzás)
    def ugat_es_fut(self):
        print(f"{self.kutya_neve} fut és ugat!")

# Példányosítás
mozi_kutya = Kutya("Rex")
mozi_kutya.ugat_es_fut()

```

**Java (camelCase)**

```java
// Osztály (Nagybetűvel kezdődik)
class Kutya {
    // Konstans (csupa nagybetű, aláhúzás)
    public static final int MAX_ELETKOR = 15;

    // Változó / Mező (kisbetűvel kezdődik, a következő szó nagybetűs)
    private String kutyaNeve;

    public Kutya(String nev) {
        this.kutyaNeve = nev;
    }

    // Metódus (kisbetűvel kezdődik, a következő szó nagybetűs)
    public void ugatEsFut() {
        System.out.println(this.kutyaNeve + " fut és ugat!");
    }
}

// Példányosítás valahol a main-ben:
// Kutya moziKutya = new Kutya("Rex");
// moziKutya.ugatEsFut();

```


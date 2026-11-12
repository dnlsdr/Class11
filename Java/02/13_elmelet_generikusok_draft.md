### 1. Altípusos vs. Parametrikus Polimorfizmus

A polimorfizmus (többalakúság) lehetővé teszi, hogy különböző típusú entitások egységes interfészen keresztül viselkedjenek. Java-ban a két legfontosabb megnyilvánulása a futási időben feloldott öröklődés, illetve a fordítási időben biztosított generikusság.

| Szempont | Altípusos polimorfizmus (Subtyping) | Parametrikus polimorfizmus (Generics) |
| --- | --- | --- |
| **Mechanizmus** | Öröklődés (`extends`) és interfész-megvalósítás (`implements`). | Típusparaméterek (`<T>`) átadása osztályoknak vagy metódusoknak. |
| **Döntési pont** | **Futási időben (Runtime):** Késői kötés (dinamikus diszpécser / `vtable`) alapján hívódik meg a konkrét objektum metódusa. | **Fordítási időben (Compile-time):** A fordító ellenőrzi a típusbiztonságot, futáskor típusradírozás történik. |
| **Kód viselkedése** | Azonos hívásra **különböző viselkedések** futnak le az alosztályok szerint. | **Ugyanaz az algoritmus/adatszerkezet** fut le, függetlenül attól, milyen típusra alkalmazzuk. |
| **Bájtkód szint** | `invokevirtual` vagy `invokeinterface` utasítás. | Típusradírozás után sima `Object` (vagy korlát) és `checkcast` utasítások. |

```java
// ALTÍPUSOS: Különböző viselkedések futási időben
abstract class Allat {
    abstract void hangotAd();
}
class Kutya extends Allat { void hangotAd() { System.out.println("Vau"); } }
class Macska extends Allat { void hangotAd() { System.out.println("Miau"); } }

// Használat: A virtuális metódustábla alapján futáskor dől el, mi szólal meg
Allat a = new Kutya();
a.hangotAd(); // "Vau"

// PARAMETRIKUS: Ugyanaz a tárolási logika bármely típusra
class Doboz<T> {
    private T tartalom;
    public void set(T t) { this.tartalom = t; }
    public T get() { return tartalom; }
}

// Használat: Nincs kódduplikáció, a típusellenőrzést a fordító végzi
Doboz<String> d1 = new Doboz<>();
d1.set("adat");
String s = d1.get(); // Nincs szükség kézi (String) castolásra

```

---

### 2. Generikus típusok és algoritmusok létrehozása

#### Mikor szükséges generikust használni?

1. **Adatszerkezeteknél:** Ha egy osztály elemeket tárol vagy rendez, de a tárolás módja teljesen független a tárolt elemek típusától (pl. verem, láncolt lista, fa, fastruktúra).
2. **Általános algoritmusoknál:** Ha egy művelet lépései azonosak különböző típusokra (pl. tömbelemek cseréje, keresés, minimumkiválasztás).
3. **API biztonság:** Ha el akarjuk kerülni a futásidejű `ClassCastException` hibákat és a felesleges `(Object)` típuskonverziókat.

#### Generikus osztály és korlátozott típusparaméter (Bounded Type)

Ha az ismeretlen típuson műveleteket is szeretnénk végezni (például összehasonlítást), felső korlátot (`extends`) kell megadni:

```java
// Generikus osztály korlátozással: csak összehasonlítható típusokat fogad el
public class Par<T extends Comparable<T>> {
    private final T elso;
    private final T masodik;

    public Par(T elso, T masodik) {
        this.elso = elso;
        this.masodik = masodik;
    }

    // Generikus metódus: a típusparaméter közvetlenül a visszatérési érték előtt van
    public T nagyobb() {
        return elso.compareTo(masodik) >= 0 ? elso : masodik;
    }
}

```

#### Önálló generikus algoritmus (Utility metódus)

Egy metódus anélkül is lehet generikus, hogy az őt tartalmazó osztály az lenne:

```java
public class Tombok {
    // Generikus algoritmus: megcserél két elemet egy tetszőleges tömbben
    public static <E> void swap(E[] tomb, int i, int j) {
        E temp = tomb[i];
        tomb[i] = tomb[j];
        tomb[j] = temp;
    }
}

```

---

### 3. A Java Collections Framework gerince

A kollekciók két független gyökérből indulnak ki: az `Iterable` $\rightarrow$ `Collection` fából és a kulcs-érték alapú `Map` hierarchiából.

```text
               Iterable<E>
                    │
              Collection<E>
        ┌───────────┼───────────┐
      List<E>     Set<E>     Queue<E> / Deque<E>
        │           │           │
   ArrayList     HashSet    ArrayDeque
  LinkedList     TreeSet   PriorityQueue
                 (SortedSet)

            Map<K, V> (Különálló interfész!)
           ┌───────────┐
        HashMap     TreeMap (SortedMap)

```

#### A legfontosabb típusok összehasonlítása

#### A legfontosabb típusok összehasonlítása

| Típus | Belső adatszerkezet | Műveletigény | Rendezés és egyediség | Mikor válaszd? |
| --- | --- | --- | --- | --- |
| **`ArrayList`** | Dinamikusan átméretezett tömb (`Object[]`). | Index szerinti elérés: **O(1)**; keresés (`contains`): **O(N)**; beszúrás a végére: amortizált **O(1)**; beszúrás vagy törlés középen: **O(N)**. | Indexelt, megtartja a beszúrási sorrendet; az ismétlődő elemek megengedettek. | Általános célú listához, főleg ha sok az olvasás és az index szerinti elérés. |
| **`LinkedList`** | Kétszeresen láncolt lista (`Node` objektumok). | Beszúrás vagy törlés az elején és a végén: **O(1)**; index szerinti elérés és keresés: **O(N)**. Egy adott helyre beszúrni csak akkor **O(1)**, ha már van ott iterátor; a hely megkeresése **O(N)**. | Indexelt, megtartja a beszúrási sorrendet; az ismétlődő elemek megengedettek. | Ha `List`-ként használva gyakran kell a széleken beszúrni vagy törölni. Sorhoz vagy veremhez általában az `ArrayDeque` célszerűbb. |
| **`HashSet`** | A `HashMap`-hez hasonló hash-tábla; az elemeket kulcsként tárolja. | `add`, `remove`, `contains`: átlagosan **O(1)**, kedvezőtlen esetben lassabb. | Egyedi elemeket tartalmaz; nincs garantált bejárási sorrend. Az egyediséget az `equals` és `hashCode` határozza meg. | Egyediség biztosítására és gyors tagságvizsgálatra (`contains`). |
| **`TreeSet`** | A `TreeMap`-re épülő kiegyensúlyozott keresőfa. | `add`, `remove`, `contains`: **O(log N)**. | Egyedi, rendezett elemek. A sorrendet a természetes rendezés vagy a megadott `Comparator` határozza meg; az egyediséget a rendezés szerinti összehasonlítás. | Ha rendezett halmazra, szélső elemekre vagy tartományi lekérdezésekre (`subSet`) van szükség. |
| **`HashMap`** | Hash-tábla. Az ütközéseket láncolással kezeli; a kellően nagy vödrök Java 8-tól bizonyos feltételek mellett fával is tárolhatók. | `get`, `put`, `remove`: átlagosan **O(1)**, ha a hash-kódok jól oszlanak el; kedvezőtlen esetben lassabb. | Egyedi kulcsok, tetszőleges értékek; a bejárási sorrend nem garantált. A kulcsok az `equals` és `hashCode` alapján azonosulnak. | Általános célú kulcs–érték tárolóhoz és gyors kulcsalapú kereséshez. |
| **`TreeMap`** | Kiegyensúlyozott keresőfa. | `get`, `put`, `remove`: **O(log N)**. | Egyedi, rendezett kulcsok; a sorrendet a természetes rendezés vagy a megadott `Comparator` határozza meg. | Ha a kulcsok rendezett bejárására vagy tartományi lekérdezésekre van szükség. |
| **`ArrayDeque`** | Körkörös, szükség szerint átméretezett tömb. | Beszúrás és törlés mindkét végén: amortizált **O(1)**. | Nincs index szerinti hozzáférés; használható veremként (LIFO) vagy sorként (FIFO). `null` elemet nem enged. | Veremhez vagy sorhoz; többnyire jobb választás, mint a `Stack` vagy a `LinkedList`. |

Az **O(1)**-es hash-műveletek átlagos, nem minden esetben garantált futási időt jelentenek. `HashSet` és `HashMap` esetén az elemek egyenlőségi szabályainak megfelelő `equals`–`hashCode` megvalósítás különösen fontos.


---

### 4. `Comparable<T>` vs. `Comparator<T>`

A Java két felületet kínál objektumok összehasonlítására és rendezésére:

#### `Comparable<T>`: A természetes belső rendezés

* **Lényege:** Az osztály **önmagát** teszi összehasonlíthatóvá egy másik azonos típusú példánnyal (`this` vs. a paraméter).
* **Elhelyezkedés:** Magában az osztályban kell implementálni (`class Diak implements Comparable<Diak>`).
* **Egyetlen rendezési elv:** Csak egyetlen természetes sorrendet lehet vele definiálni egy típushoz.
* **Metódusa:** `int compareTo(T other)`

#### `Comparator<T>`: A külső, testreszabható rendezés

* **Lényege:** Egy **külső stratégia**, amely két független objektumot hasonlít össze.
* **Elhelyezkedés:** Külön osztályként, névtelen belső osztályként, vagy leggyakrabban **lambda kifejezésként / metódusreferenciaként** adjuk át.
* **Tetszőleges számú rendezési elv:** Bármennyi különféle szempont szerint rendezhetünk (pl. név szerint növekvő, életkor szerint csökkenő, fizetés szerint).
* **Metódusa:** `int compare(T o1, T o2)`

#### Az összehasonlítás szerződése (Contract)

Mind a `compareTo`, mind a `compare` metódusnak egy egész számot kell visszaadnia:

* **Negatív szám ($< 0$):** ha az első elem kisebb, mint a második.
* **Nulla ($= 0$):** ha a két elem egyenlő (szorosan ajánlott, hogy ha `compareTo == 0`, akkor az `equals()` is `true` legyen).
* **Pozitív szám ($> 0$):** ha az első elem nagyobb, mint a második.

#### Kódpélda a gyakorlati megvalósításra:

```java
import java.util.*;

// 1. COMPARABLE: Természetes rendezés azonosító (id) alapján
class Hallgato implements Comparable<Hallgato> {
    private final int id;
    private final String nev;
    private final double atlag;

    public Hallgato(int id, String nev, double atlag) {
        this.id = id;
        this.nev = nev;
        this.atlag = atlag;
    }

    public int getId() { return id; }
    public String getNev() { return nev; }
    public double getAtlag() { return atlag; }

    @Override
    public int compareTo(Hallgato masik) {
        // Egész számoknál tilos a (this.id - masik.id) a túlcsordulás (integer overflow) veszélye miatt!
        return Integer.compare(this.id, masik.id);
    }

    @Override
    public String toString() {
        return nev + " (" + id + ", átlag: " + atlag + ")";
    }
}

public class Rendezestesztek {
    public static void main(String[] args) {
        List<Hallgato> lista = new ArrayList<>(List.of(
            new Hallgato(103, "Anna", 4.5),
            new Hallgato(101, "Béla", 3.8),
            new Hallgato(102, "Cecil", 4.9)
        ));

        // 1. Rendezés Comparable szerint (természetes sorrend: id alapján)
        Collections.sort(lista);
        // Eredmény: Béla (101), Cecil (102), Anna (103)

        // 2. COMPARATOR: Rendezés név szerint (lambda vagy metódusreferencia)
        lista.sort((h1, h2) -> h1.getNev().compareTo(h2.getNev()));
        // Vagy elegánsabban: lista.sort(Comparator.comparing(Hallgato::getNev));

        // 3. COMPARATOR: Összetett rendezés (átlag szerint csökkenő, holtversenyben név szerint)
        lista.sort(
            Comparator.comparingDouble(Hallgato::getAtlag).reversed()
                      .thenComparing(Hallgato::getNev)
        );
    }
}

```

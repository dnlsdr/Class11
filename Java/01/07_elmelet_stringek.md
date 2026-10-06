## A `String` osztály és a szövegek létrehozása

A Javában a szöveges adatok (`String`) nem primitív típusok (mint az `int` vagy a `boolean`), hanem **objektumok**, amelyek a `String` osztályból származnak. Egy `String` objektumot kétféleképpen hozhatunk létre:

1. **String literállal (idézőjelek között):** `String s1 = "Hello";` – Ez a leggyakoribb és leginkább javasolt módszer. (Fontos: a szimpla idézőjel `'a'` csak egyetlen karakterre, `char` típusra használható, szövegre nem!)
2. **A `new` kulcsszóval (konstruktorral):** `String s2 = new String("Hello");` – Ritkábban használjuk, mert memóriakezelési szempontból kevésbé hatékony.

A szövegeket összefűzhetjük (**konkatenáció / concatenation**) a `+` operátorral vagy a `.concat()` metódussal.

---

## Hasznos metódusok a `String` osztályon

Mivel a `String` egy osztály, rengeteg beépített metódussal rendelkezik, amelyekkel lekérdezhetjük vagy átalakíthatjuk a szöveget. A karakterek számozása (indexelése) – akárcsak a tömböknél – **0-tól indul**!

```java
public class StringMethodsDemo {
    public static void main(String[] args) {
        String a = "Learning Java is so much fun";
        
        // 1. charAt(index): Visszaadja az adott indexen lévő karaktert.
        // L(0) e(1) a(2) r(3) n(4) i(5) n(6) g(7) [szóköz](8) J(9)
        char output = a.charAt(9); // Eredmény: 'J'
        
        // 2. indexOf(karakter/szöveg): Megkeresi, hányadik indexen szerepel először.
        int index = a.indexOf("Java"); // Eredmény: 9
        
        // 3. length(): Visszaadja a szöveg hosszát (karakterek számát).
        int len = a.length(); // Eredmény: 28
        
        // 4. trim(): Készít egy másolatot, amelyből eltávolítja a szöveg ELEJÉN (leading) 
        // és VÉGÉN (trailing) lévő szóközöket (whitespace). A szavak közöttieket NEM bántja!
        String dirty = "   Hello World!   ";
        String clean = dirty.trim(); // Eredmény: "Hello World!"
        
        // 5. Kis- és nagybetűssé alakítás, részszöveg kivágása:
        System.out.println(clean.toUpperCase());   // "HELLO WORLD!"
        System.out.println(clean.toLowerCase());   // "hello world!"
        System.out.println(a.substring(9, 13));    // "Java" (9-től 13-ig, a 13. már nincs benne)
    }
}

```

---

## String Immutability (Megváltoztathatatlanság) és a Memória

A Java memóriakezelése két fő területre oszlik: a **Stack**-ben tárolódnak a lokális változók és a **referenciák** (memóriacímek), míg a **Heap**-ben élnek maguk a konkrét **objektumok**.

### Mit jelent az, hogy a `String` *immutable* (megváltoztathatatlan)?

Azt jelenti, hogy **miután egy `String` objektum létrejött a Heap memóriában, az értéke soha többé nem módosítható**.

Amikor úgy tűnik, hogy módosítasz egy szöveget (például új értéket adsz a változónak, vagy hozzáfűzöl valamit), a háttérben az eredeti objektum érintetlen marad. Ehelyett **egy teljesen új `String` objektum jön létre** a memóriában az új értékkel, a változód pedig egyszerűen átáll (átirányítódik), hogy ennek az új objektumnak a memóriacímét (referenciáját) tárolja.

**Példa objektumon belüli változásra:**
Ha van egy osztályod egy szöveges mezővel (pl. `String description`), és példányosítod (`new`), a `description` kezdetben `null` értékre mutat (vagy egy kezdő szövegre). Amikor frissíted (`update`) ezt a mezőt egy új szöveggel, a `description` referencia elengedi a régi objektumot, és egy újonnan létrejött `String` objektumra fog mutatni a Heap-en.

```java
String text = "Java";
text = text + " 21"; // Nem a "Java" objektum íródik át! 
// Létrejön egy új "Java 21" objektum, és a 'text' változó mostantól erre mutat.

```

### Miért jó ez nekünk? (Előnyök)

1. **Szálbiztosság (Thread-safety):** Mivel a `String` objektum értéke garantáltan nem változhat meg, több párhuzamosan futó folyamat (thread) is biztonságosan olvashatja ugyanazt a `String`-et anélkül, hogy aggódni kellene amiatt, hogy egy másik szál menet közben belepiszkál és átírja az értéket.
2. **Memóriaspórolás (String Pool):** Csak azért működhet a String Pool (lásd lejjebb), mert a szövegek megváltoztathatatlanok.

---

## Szövegek összehasonlítása (`==` vs. `equals()`) és a String Pool

Amikor két primitív értéket (pl. `int a = 5; int b = 5;`) hasonlítunk össze a `==` operátorral, maga az érték kerül összehasonlításra. **Objektumoknál azonban a `==` operátor a memóriacímeket (referenciákat) hasonlítja össze**, vagyis azt nézi meg, hogy a két változó pontosan ugyanarra a fizikai objektumra mutat-e a memóriában!

### Mi az a String Pool?

Mivel a programokban rengeteg szöveget használunk, a JVM (Java Virtual Machine) fenntart egy speciális memóriaterületet a Heap-en belül, amit **String Pool**-nak hívunk.

* Amikor **literállal** hozol létre egy szöveget (`String s1 = "Alma";`), a JVM először megnézi, van-e már `"Alma"` a String Poolban. Ha nincs, beteszi oda.
* Ha ezután létrehozol egy másik változót ugyanazzal a literállal (`String s2 = "Alma";`), a JVM látja, hogy már létezik ilyen a Poolban, ezért **nem hoz létre új objektumot**, hanem az `s2`-nek is pontosan ugyanazt a memóriacímet (referenciát) adja oda, amire az `s1` mutat.
* Ha viszont a **`new` kulcsszót** használod (`String s3 = new String("Alma");`), kikényszeríted a JVM-et, hogy a String Poolon kívül, a sima Heap területen hozzon létre egy vadonatúj, különálló objektumot, új memóriacímmel.

```java
public class StringComparison {
    public static void main(String[] args) {
        String s1 = "Cat";               // Bemegy a String Pool-ba
        String s2 = "Cat";               // A JVM megtalálja a Pool-ban, ugyanazt a címet kapja!
        String s3 = new String("Cat");   // Új objektum jön létre a Pool-on kívül, új címmel!

        // 1. Összehasonlítás == operátorral (Referencia / memóriacím vizsgálata):
        System.out.println(s1 == s2); // true  -> Mert mindkettő ugyanarra a Pool-beli példányra mutat!
        System.out.println(s1 == s3); // false -> Mert a memóriacímük különböző!

        // 2. Összehasonlítás .equals() metódussal (Tartalom / érték vizsgálata):
        System.out.println(s1.equals(s2)); // true -> A szöveg tartalma mindkettőben "Cat"
        System.out.println(s1.equals(s3)); // true -> Nem számít, hogyan jött létre, a tartalom "Cat"!
    }
}

```

> **Fontos:** az előbb felsorolatak miatt `String`-eket (és általában objektumokat) **mindig az `.equals()` metódussal hasonlítunk össze**, mert az a tényleges tartalmat (értéket) vizsgálja, függetlenül attól, hogy a memóriában hol és hogyan jöttek létre!

---

## `StringBuilder` és `StringBuffer` (Módosítható szövegek)

Mi történik, ha egy ciklusban 1000-szer fűzöl hozzá egy-egy szót egy `String`-hez? Mivel a `String` *immutable*, a Java 1000 darab különálló objektumot fog legyártani a memóriában, amiből 999 azonnal felesleges szemétté válik. Ez rendkívül lassú és memóriapazarló.

Ha gyakran kell módosítanunk egy szöveget, **mutable (módosítható)** osztályokat használunk: a **`StringBuilder`**-t vagy a **`StringBuffer`**-t. Ezeknél a módosítások (hozzáfűzés, törlés, beszúrás) **helyben, az eredeti objektumon belül** történnek meg, új objektumok létrehozása nélkül.

### `StringBuilder` vs. `StringBuffer` – Mi a különbség?

| Tulajdonság | `String` | `StringBuilder` | `StringBuffer` |
| --- | --- | --- | --- |
| **Módosíthatóság** | **Immutable** (Nem módosítható) | **Mutable** (Módosítható) | **Mutable** (Módosítható) |
| **Szálbiztosság (Thread-safe)** | Igen (az immutabilitás miatt) | **Nem** (Not thread-safe) | **Igen** (Thread-safe, szinkronizált) |
| **Sebesség** | Lassú (sok módosításnál) | **A leggyorsabb** | Lassabb, mint a StringBuilder |

* A **`StringBuilder`** a modern, alapértelmezett választás a legtöbb esetben, mert nem tartalmazza a szálbiztossághoz szükséges extra ellenőrzéseket, így **sokkal gyorsabb**, mint a `StringBuffer`.
* A **`StringBuffer`** csak akkor kell, ha több szál (multi-threading) egyszerre próbálja módosítani ugyanazt a szövegépítő objektumot.

### Példa a `StringBuilder` használatára

```java
public class BuilderDemo {
    public static void main(String[] args) {
        // Példányosítunk egy módosítható StringBuilder objektumot
        StringBuilder sb = new StringBuilder("Learning");

        // Az append() metódussal ugyanazt az objektumot bővítjük (nem jön létre új!)
        sb.append(" Java");
        sb.append(" is fun!");

        // Beszúrás adott indexre (insert), vagy megfordítás (reverse):
        sb.insert(8, " modern"); // "Learning modern Java is fun!"

        // Ha a végén újra sima String-re van szükségünk, a toString()-gel alakítjuk át:
        String finalResult = sb.toString();
        System.out.println(finalResult);
    }
}
```

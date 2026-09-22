## Primitívek vs. Objektumok

A Javában az adatok két nagy táborra oszthatók, ezek a következők.

* **Primitívek:** Ezek az építőkockák (pl. `int`, `boolean`, `double`). Csak és kizárólag egyetlen nyers adatot tárolnak, semmi mást. Nincsenek metódusaik (beépített függvényeik). A primitívek **immutable** (megváltoztathatatlan) tulajdonságúak: magát a memóriában lévő értéket (például a `7`-es számot) nem tudod módosítani, legfeljebb a változóba egy teljesen új értéket írsz.
* **Objektumok:** Komplex adatszerkezetek. Nemcsak több adatot tárolhatnak egyszerre, hanem metódusokat (viselkedéseket) is, amikkel ezeket az adatokat manipulálhatják. Alapértelmezetten **mutable** (módosítható) állapotúak, tehát a létrehozásuk után megváltoztathatjuk a belső tulajdonságaikat.
* *Kivétel:* A **String** (szöveg) Javában egy objektum, de kivételes módon **immutable**. Ha egyszer létrehozol egy String objektumot, azt soha többé nem módosíthatod. Ha hozzáfűzöl egy betűt, a Java valójában a háttérben egy teljesen új String objektumot hoz létre.



## Primitíveket tartalmazó objektumok

Amikor egy osztályt definiálsz, az objektum belső állapotát (mezőit) nagyon gyakran egyszerű primitívek alkotják. Az objektum mintegy becsomagolja ezeket a nyers adatokat.

```java
class Diak {
    int kor = 16;          // Primitív típus
    boolean aktiv = true;  // Primitív típus
}

```

Ebben az esetben a `Diak` objektum egyetlen egységként kezeli ezt a két primitív értéket.

## Objektumokat tartalmazó objektumok

Ezt a koncepciót hívják **Object Composition**-nek (objektum kompozíció). Egy komplex rendszer modellezéséhez a nagy objektumokat kisebb, önálló objektumokból építjük fel. Egy objektum mezője (tulajdonsága) simán lehet egy másik objektum.

```java
class Motor {
    int loero = 150;
}

class Auto {
    String marka = "Ford"; // String objektum
    Motor autoMotor = new Motor(); // Kompozíció: az Auto tartalmaz egy Motor objektumot
}

```

Itt az `Auto` nem maga valósítja meg a motor működését, hanem "birtokol" egy `Motor` objektumot.

## A Stack és a Heap

A Java memóriakezelése két fő területre oszlik, amelyeknek eltérő a feladatuk.

* **Stack (Verem):** Ez felel a program futásának aktuális végrehajtásáért. Gyors, rendezett, és **LIFO** (Last In, First Out – Utolsóként be, elsőként ki) elven működik. Képzeljünk el egy halom egymásra pakolt tányért: mindig a legfelsőt veszed le, és az újat is a tetejére teszed.
* A Stack tárolja a lokális változókat, a metódushívások sorrendjét, a **primitív értékeket**, és az **objektumok memóriacímeit** (referenciáit).


* **Heap (Kupac):** A Heap egy nagy, dinamikus memóriaterület. Ide kerülnek maguk a **példányosított objektumok** (amiket a `new` kulcsszóval hozunk létre). Mivel az objektumok túl nagyok és változó méretűek lehetnek, nem férnének el hatékonyan a Stack-en. A Stack-en lévő memóriacímek (referenciák) ide mutatnak.

## A "Pass by Value" jelentése Javában

**A Java KIZÁRÓLAG "pass by value" (érték szerinti) paraméterátadást használ.** Amikor egy változót átadsz egy függvénynek, a Java mindig **másolatot** készít az eredeti értékről, és a függvény ezen a másolaton dolgozik.

De nem mindegy, hogy *mit* másol le:

* **Primitívek esetén:** Magát a konkrét értéket (pl. a `42`-t) másolja le. Ha a függvény módosítja a másolatot `43`-ra, az eredeti változó érintetlen marad (továbbra is `42`).
* **Objektumok esetén:** Mivel az objektumot tároló változó a Stack-en valójában csak egy **memóriacímet** (referenciát) tartalmaz, a Java ezt a memóriacímet másolja le! Az eredmény: az eredeti és a lemásolt cím is pontosan ugyanarra a Heap-ben lévő objektumra mutat. Ha a függvényen belül a másolt címen keresztül módosítod az objektumot, az eredeti objektum is megváltozik (hasonlóan a C++ "pass by reference" logikájához, de mechanikájában ez szigorúan a cím másolata).

**Az öltözőszekrény példa:**
Képzeld el, hogy egy objektum egy öltözőszekrény a Heap-ben. A memóriacím az a kulcs, ami kinyitja. Amikor egy objektumot átadsz egy metódusnak (pass by value), az olyan, mintha **másoltatnál egy teljesen új kulcsot**, és odaadnád a barátodnak. Most már két embernek van két *különböző* (lemásolt) kulcsa, de mindkét kulcs pontosan *ugyanazt* az öltözőszekrényt nyitja. Ha a barátod a saját kulcsával kinyitja a szekrényt, és beletesz egy almát, te a saját kulcsoddal kinyitva is ott fogod találni azt az almát.

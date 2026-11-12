## 1. Interfész általános fogalma

Az interfész (interface) egy felület, amely meghatározza és általánosítja egy osztály struktúráját, de magát a megvalósítást (implementációt) nem szabja meg. Jellemzően konstansokat és tagfüggvények szignatúráit tartalmazza.

### 1.1. Java építőelemek

A Java nyelvben az alábbi alapvető építőelemeket különböztetjük meg:

* **Class (osztály):** A hagyományos építőelem, melynek közös őse a `java.lang.Object`.
* **Interface (interfész):** Teljesen absztrakt osztályként viselkedik, amely "használati utasításként" vagy sablonként szolgál az osztályokhoz. Nem rendelkeznek közös őssel, és csak konstansokat, valamint tagfüggvények szignatúráit tartalmazzák.
* **Enum (felsorolás):** Olyan speciális osztály, amelynek csak véges számú példánya létezik.



### 1.2. Többszörös öröklődés problémái

A többszörös öröklődés (amit például a C++ támogat) elvezethet a "gyémánt problémához" (diamond problem): ha két ősosztály is tartalmaz egy azonos szignatúrájú függvényt, nem egyértelmű, hogy a közös leszármazott melyik megvalósítást örökölje. A Java megoldása erre az, hogy osztályok szintjén **nincs többszörös öröklődés**, viszont egy osztály több interfészt is megvalósíthat.

### 1.3. Java interfészek

A Java interfészek célja, hogy azt definiálják: „Mit tud az osztály?”, és ne azt, hogy „Hogyan?”. Ezért a belső szerkezet meghatározására nem alkalmasak, és csak publikus elemeket tartalmazhatnak, nehogy feltörjék az osztályok enkapszulációját.

### Mindez a gyakorlatban

Az interfészek arra jók, hogy garantáljanak egy adott funkcionalitást (az implementáló osztálynak kötelező megvalósítania a tagfüggvényeket), és több, egymáshoz hasonló osztály kezelőfelületét egységesítsék. Mivel az interfész statikus típusként is használható, az azt megvalósító különböző osztályok objektumai „egy kalap alá vehetők” és egységesen kezelhetők.

## 2.1. Hogyan működik?

Egy interfészen belül a módosítószavakat bizonyos esetekben nem kötelező kiírni, mert alapértelmezetten érvényesülnek:

* **Tagfüggvények:** Minden megadás nélkül `public abstract` láthatóságúak és viselkedésűek. (Nincs értelme private vagy protected tagfüggvényeknek egy publikus szerződésben.)
* **Konstansok:** Alapértelmezetten mindegyik `public static final`.
* **Statikus metódusok:** Lehetőség van `static` metódusok létrehozására is az interfészen belül, ilyenkor az implementációt is meg kell adni.



### 2.2. Interfészek megvalósítása

Az interfészeket az osztályok az `implements` kulcsszóval valósítják meg. Ha egy osztály megvalósít egy interfészt, kötelező az összes benne lévő metódust felüldefiniálnia (implementálnia), ellenkező esetben magát az osztályt is `abstract`-nak kell jelölni. Ha az osztály örököl is egy másik osztályból, a szintaktikai sorrend szigorú: először az `extends`, majd az `implements` következik (pl. `public class Apple extends Fruit implements Food`).

### 2.3. Öröklés

Bár az osztályoknál a Java tiltja a többszörös öröklődést, **az interfészek között megengedett a többszörös öröklődés** az `extends` kulcsszóval. A leszármazott interfész örökli az összes ős konstanst és tagfüggvényt. Ha azonos nevű konstansokat örököl különböző ősöktől, névütközés lép fel, és a leszármazotton keresztül nem lehet egyértelműen hivatkozni rájuk.
Egy osztály bármennyi interfészt implementálhat, és egy interfészt bármennyi osztály megvalósíthat.

## 3. Kódoljunk!

### 3.1. A kód

Az alábbi példa bemutatja az interfészek definiálását és osztályok általi implementálását, többszörös megvalósítással egybekötve:

```java
// Interfész definíciók
public interface Food {
    String whatToEat();
}

public interface Delicious extends Food {
    int howDelicious();
}

// Interfészek megvalósítása osztályokban
public class Apple implements Delicious {
    @Override 
    public String whatToEat() {
        return "Apple";
    }
    
    @Override 
    public int howDelicious() {
        return 10;
    }
}

public class Chocolate implements Delicious {
    @Override 
    public String whatToEat() {
        return "Chocolate";
    }
    
    @Override 
    public int howDelicious() {
        return 100;
    }
}

```

## 4. Hogy kell ezt használni?

Amikor osztályokat példányosítunk, az interfész nevét használhatjuk statikus típusként a hivatkozáshoz (pl. `Food myFood = new Apple();`). Ezáltal a kód általánosabb lesz.

### 4.1. Beépített interfészek

A Java szabványos könyvtára számos beépített interfészt biztosít, amelyek egységesítik az osztályok képességeit:

* `Comparable`: Lehetővé teszi az objektumok összehasonlítását (rendezését).
* `Cloneable`: Ún. marker (jelölő) interfész, amely engedélyezi az `Object.clone()` használatát.
* `Serializable`: Marker interfész, amely engedélyezi az objektum sorosítását, így azt bájtsorozattá (pl. fájlba) lehet alakítani és visszaállítani.
* `Runnable`: Azt jelzi, hogy az osztály egy külön szálon (Thread) futtatható.



## 5. Interfészek módosítása

Ha egy már meglévő, sok osztály által használt interfészbe utólag egy új, megvalósítás nélküli tagfüggvényt teszünk, a kód "lerokkan", mert a meglevő implementáló osztályok nem fognak lefordulni. Erre a problémára a Java két megoldást kínál.

### 5.1. Példák

**1. Új interfész létrehozásával (ha az új metódus nem kell mindenkinek):**
Készítünk egy új interfészt, ami örököl az eredetiből, és csak ehhez adjuk hozzá az új funkcionalitást.

```java
public interface Food {
    String whatToEat();
}

public interface FoodWithCalories extends Food {
    Integer howManyCalories();
}

```

**2. Alapértelmezett (`default`) metódus használatával (ha a bővítés mindenkire vonatkozik):**
A `default` kulcsszóval magában az interfészben is adhatunk implementációt, így a régi osztályok lefordulnak, mert megkapják ezt az alapértelmezett működést, bár továbbra is felüldefiniálhatják azt.

```java
public interface Food {
    String whatToEat();
    
    default Integer howManyCalories() {
        return null;
    }
}

```

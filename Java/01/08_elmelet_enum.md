Az `enum` (enumeráció) a Javában egy speciális típus, amely előre definiált konstansok rögzített halmazát képviseli. Akkor használjuk, amikor pontosan tudjuk az összes lehetséges értéket már a program írásakor (például a hét napjai, égtájak, vagy egy rendelés státuszai). Használatukkal a kód sokkal olvashatóbbá és típusbiztossá válik.

### 1. Alapvető Enumok létrehozása és használata

A legegyszerűbb enumok egy listát tartalmaznak az értékekről. Konvenció szerint az enum konstansokat nagybetűvel írjuk. A `.values()` metódussal a Java automatikusan egy tömböt ad vissza az összes lehetséges értékről, amin végigiterálhatunk.

**Példa: A játék állapotai és a hét napjai**

```java
// 1. Enum létrehozása a játék állapotainak
enum GameStatus {
    NOT_STARTED, IN_PROGRESS, PAUSED, COMPLETED
}

// 2. Enum létrehozása a hét napjainak
enum Weekday {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

public class BasicEnumsExample {
    public static void main(String[] args) {
        // Változó létrehozása az enum típusával és értékadás
        GameStatus currentStatus = GameStatus.PAUSED;
        System.out.println("A játék jelenlegi állapota: " + currentStatus);

        System.out.println("\n--- A hét napjai ---");
        // Ciklus az enum értékein a .values() segítségével
        for (Weekday day : Weekday.values()) {
            System.out.println(day);
        }
    }
}

```

### 2. Hogyan működnek az Enumok a háttérben?

Amikor definiálsz egy enumot, a Java fordító a háttérben egy normál osztályt generál, amely a `java.lang.Enum` osztályból származik.

Az enumon belül felsorolt értékek (pl. `MONDAY` vagy `PAUSED`) valójában ennek a rejtett osztálynak a `public static final` példányai (objektumai). Mivel az enum garantálja, hogy csak ezek a konkrét példányok létezhetnek, az enumokból **nem lehet a `new` kulcsszóval új objektumot létrehozni** futási időben. Csak a memóriában már ott lévő, előre legyártott példányokra hivatkozhatsz. Emiatt az enumok összehasonlítására biztonságosan használható a `==` operátor is az `.equals()` mellett.

### 3. Enumok tagokkal (Mezők, Konstruktorok, Metódusok)

Az enumok sokkal többet tudnak egy egyszerű listánál: viselkedhetnek úgy, mint a normál osztályok. Lehetnek saját belső állapotokat tároló mezőik, konstruktoruk és metódusaik.

Az enum konstruktora speciális: **mindig `private`** (még ha nem is írod ki a kulcsszót), hiszen csak maga az enum használhatja arra, hogy a kód legelején legyártsa a saját konstansait.

**Példa: Bolygók a Naprendszerben kiegészítő adatokkal**

```java
enum Planet {
    // 1. Az enum konstansok meghívják a belső konstruktort
    MERCURY("Merkúr", 0.39),
    EARTH("Föld", 1.0),
    MARS("Mars", 1.52),
    JUPITER("Jupiter", 5.20);

    // 2. Privát mezők az adatok tárolására (érdemes final-re állítani őket)
    private final String name;
    private final double distanceFromSun; // Csillagászati egységben (AU)

    // 3. Konstruktor (automatikusan private)
    Planet(String name, double distanceFromSun) {
        this.name = name;
        this.distanceFromSun = distanceFromSun;
    }

    // 4. Publikus getterek, hogy a program többi része elérje az adatokat
    public String getName() {
        return name;
    }

    public double getDistanceFromSun() {
        return distanceFromSun;
    }
}

public class EnumsWithFieldsExample {
    public static void main(String[] args) {
        System.out.println("Bolygók és távolságuk a Naptól:");
        
        // Végigiterálunk a bolygókon, és kiolvassuk a bennük tárolt extra adatokat
        for (Planet planet : Planet.values()) {
            System.out.println(planet.getName() + " - " + planet.getDistanceFromSun() + " AU");
        }
    }
}
```
# 4. Enumhasználat előnyei

A "varázsszámok" (magic numbers) és "varázsszövegek" (magic strings) olyan a kódba fixen beégetett értékek, amelyeknek a jelentése kontextus nélkül nem egyértelmű, és könnyű velük hibázni, mert a Java fordítója (compiler) nem tudja ellenőrizni a logikai helyességüket.

### 1. A probléma: "Varázsszámok" és "Varázsszövegek" használata

Tegyük fel, hogy egy rendelésnek három állapota lehet: Fogadva, Sütés alatt, Kiszállítás alatt.

**A rossz megoldás számokkal (`int`):**

```java
public class PizzaOrder {
    int status; // 0 = Fogadva, 1 = Sütés alatt, 2 = Kiszállítás alatt

    public void setStatus(int newStatus) {
        this.status = newStatus;
    }
}

```

**Mi ezzel a baj?**
Ha fél év múlva valaki használja ezt a kódot, honnan fogja tudni, mit jelent az `1`-es szám? Ráadásul semmi sem akadályozza meg, hogy valaki meghívja a `order.setStatus(99)` metódust. Mivel a `99` egy érvényes `int`, a program lefordul, de futás közben váratlan hibákat fog okozni.

**A rossz megoldás szöveggel (`String`):**

```java
public void setStatus(String newStatus) {
    this.status = newStatus;
}

```

**Mi ezzel a baj?**
Bár már olvashatóbb (`order.setStatus("SUTES_ALATT")`), de sérülékeny. Ha elgépelsz egy betűt, például `order.setStatus("SUTES_ALAT")`, a fordító ezt is simán elfogadja (hiszen ez egy érvényes `String`), de a programod, ami a pontos szöveget várja, nem fogja tudni helyesen kezelni.

### 2. A megoldás: Típusbiztosság (Type Safety) Enumokkal

Ha bevezetünk egy Enumot, a fenti problémák megszűnnek:

```java
enum OrderStatus {
    RECEIVED, BAKING, DELIVERING
}

public class PizzaOrder {
    OrderStatus status;

    public void setStatus(OrderStatus newStatus) {
        this.status = newStatus;
    }
}

```

Mostantól, ha be akarod állítani a státuszt, **kizárólag az enum konstansait használhatod**: `order.setStatus(OrderStatus.BAKING)`.
Ha megpróbálod azt írni, hogy `order.setStatus(99)` vagy `order.setStatus("BAKING")`, **a kód el sem indul (fordítási hiba történik)**.
Szóval még korán észre veszed a hibát.

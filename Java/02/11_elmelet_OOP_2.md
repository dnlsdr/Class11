Az öröklődés (inheritance) az objektumorientált programozás egyik legfontosabb eszköze, amely lehetővé teszi, hogy meglévő osztályok alapján hozzunk létre újakat a kód megismétlése nélkül.

---

## 1. Motiváció és az Öröklés Alapjai Javában

### Miért használunk öröklődést?

Amikor egy meglévő osztály funkcióit szeretnénk bővíteni, vagy bizonyos működését az új igényeknek megfelelően átalakítani, az öröklés segít elkerülni a **kódismétlést (redundanciát)**. Ezáltal a kódunk rövidebb, olvashatóbb és sokkal könnyebben karbantartható lesz.

### Az `extends` kulcsszó és az „is-a” reláció

Javában az öröklődést az `extends` (kiterjeszteni) kulcsszóval adjuk meg:

```java
class Child extends Parent { 
    /* A leszármazott (Child) törzse */ 
}

```

* **Altípus képzés (is-a reláció):** A leszármazott osztály a szülőosztály egy speciális fajtája (altípusa) lesz. Például: a *Taxi* egyben *Autó* is (`Taxi extends Car`), a *Négyzet* pedig egy speciális *Téglalap* (`Square extends Rectangle`).
* **Fontos korlát:** A tartalmazási (*has-a*, pl. az autónak *van* motorja) kapcsolatot **nem** lehet öröklődéssel kifejezni.
* **Mit örököl a leszármazott?** A szülőosztály **minden** tulajdonságát (adattagját) és metódusát örökli, a láthatóságtól függetlenül! A szülő `private` adattagjai is ott lesznek a leszármazott objektumban a memóriában, csupán a leszármazott kódjából **nem férhetünk hozzájuk közvetlenül** (ehhez `protected` vagy `public` láthatóság kell). A konstruktorok **nem** öröklődnek.

### Osztályhierarchia és az `Object` ősosztály

* **Egyszeres öröklődés:** Javában egy osztálynak közvetlenül csak egyetlen szülője lehet, így az osztályhierarchia egy **fa** struktúrát alkot.
* **Az `Object` osztály:** A `java.lang.Object` az „univerzális ősosztály”. Ha egy osztály definíciójánál nem írunk `extends`-et, a fordító automatikusan az `Object` leszármazottjává teszi (minden nem-primitív típus belőle származik). Ebből örököljük például a `toString()`, `equals()` és `hashCode()` metódusokat.

---

## 2. Konstruktorok és Inicializálás Öröklődéskor

### A `super` kulcsszó és a konstruktor delegálás

Mivel a konstruktorok nem öröklődnek, a leszármazott osztálynak saját konstruktort kell írnia. Minden konstruktor első sorában (explicit vagy implicit módon) meghívódik egy másik konstruktor.

* Ha nem írunk semmit az első sorba, a Java **automatikusan (implicit módon) meghívja a szülő paraméter nélküli konstruktorát** (`super()`).
* **Gyakori hiba:** Ha a szülőosztályban (pl. `Car`) írtunk paraméteres konstruktort, akkor a fordító már nem generál hozzá paraméter nélkülit. Ilyenkor a leszármazottban (`Taxi`) **kötelező explicit módon** meghívni a szülő megfelelő konstruktorát a `super(...)` utasítással!

**Példa a `Taxi` osztály létrehozására és konstruktorára:**

```java
public class Taxi extends Car {
    protected double balance; // A taxis pénztárcája
    protected double kmCost;  // Kilométerdíj

    public Taxi(String type, double consumption, int capacity, double kmCost) {
        // Explicit módon meghívjuk a szülő (Car) 3 paraméteres konstruktorát:
        super(type, consumption, capacity); 
        this.kmCost = kmCost;
        this.balance = 0;
    }
}

```

### Konstruktorok és inicializáló blokkok lefutási sorrendje

Amikor példányosítunk egy leszármazott osztályt (pl. *Előbb le kell gyártani magát az autót, és csak utána lehet taxinak lefóliázni*), a lefutási sorrend szigorúan a következő:

1. **Ősosztály** `static` inicializáló blokkja
2. **Leszármazott osztály** `static` inicializáló blokkja
3. **Ősosztály** példány inicializáló blokkja
4. **Ősosztály** konstruktora
5. **Leszármazott osztály** példány inicializáló blokkja
6. **Leszármazott osztály** konstruktora

**Példa a lefutási sorrendre:**

```java
public class A {
    static { System.out.println("1. A static blokk"); }
    { System.out.println("3. A példány blokk"); }
    A() { System.out.println("4. A konstruktor"); }
}

public class B extends A {
    static { System.out.println("2. B static blokk"); }
    { System.out.println("5. B példány blokk"); }
    B() { System.out.println("6. B konstruktor"); }

    public static void main(String[] args) {
        B obj = new B(); // A számok sorrendjében írja ki az üzeneteket!
    }
}

```

---

## 3. Az Öröklés Lehetőségei

Egy leszármazott osztályban többféleképpen alakíthatjuk a szülőtől kapott elemeket:

| Művelet | Mit jelent? | Szabályok / Megjegyzések |
| --- | --- | --- |
| **Bővítés** | Új adattagok és új metódusok felvétele. | Speciális viselkedéssel ruházza fel az altípust (pl. `transfer()` a taxiban). |
| **Felüldefiniálás (Override)** | A szülő egy példánymetódusának átírása **ugyanazzal a szignatúrával**. | • A visszatérési típusnak egyeznie kell (vagy kovariáns altípusnak kell lennie).<br>

<br>• A láthatóság **nem szűkíthető** (pl. `public`-ból nem lehet `protected`).<br>

<br>• Ellenőrzött kivételek köre **nem bővíthető**, csak szűkíthető.<br>

<br>• A szülő eredeti metódusa a `super.metodusNev()` hívással továbbra is elérhető.<br>

<br>• *Szabály:* Konstruktorból csak `final` (nem felüldefiniálható) metódust szabad hívni! |
| **Túlterhelés (Overload)** | Ugyanazzal a névvel, de **eltérő paraméterlistával** írunk új metódust. | Csak a visszatérési típus megváltoztatásával nem lehet túlterhelni! A paraméterezés dönti el, melyik fut le. |
| **Elfedés (Hiding)** | A szülővel azonos nevű **adattag** vagy azonos szignatúrájú **statikus (osztály) metódus** deklarálása. | Ritkán szükséges, elrejti a szülő azonos nevű elemét. |

### Példa: Felüldefiniálás és Bővítés a `Taxi` osztályban

*(Ehhez a szülő `Car` osztályban a `fuel`, `fuelPrice` és `capacity` változókat `private`-ról `protected`-re kell állítani, hogy a leszármazott közvetlenül elérje őket!)*

```java
public class Taxi extends Car {
    protected double balance;
    protected double kmCost;

    public Taxi(String type, double consumption, int capacity, double kmCost) {
        super(type, consumption, capacity);
        this.kmCost = kmCost;
        this.balance = 0;
    }

    // --- FELÜLDEFINIÁLÁSOK (@Override) ---

    @Override
    public double cost(double km) {
        // Meghívjuk a szülő költségszámítását (benzinár), és hozzáadjuk a km díjat
        return super.cost(km) + kmCost * km; 
    }

    @Override
    public void refuel(double liter) {
        this.fuel += liter;
        this.balance -= liter * Car.fuelPrice; // A tankolást a taxis a pénztárcájából fizeti
    }

    @Override
    public String toString() {
        return super.toString() + ", a fuvarozó pénze: " + this.balance;
    }

    // --- ÚJ FUNKCIÓK (Bővítés) ---

    // Fuvarozás: ha van elég benzin (a szülő go() metódusa alapján), növeli a bevételt
    public double transfer(double km) {
        if (go(km) == km) {
            this.balance += this.cost(km);
            return km;
        } else {
            return 0;
        }
    }

    // Egy főre jutó költség kiszámítása kivételkezeléssel
    public double costPerPerson(double km, int num) throws Exception {
        if (num > this.capacity + 1) {
            throw new Exception("Több utas, mint férőhely!");
        }
        return cost(km) / num;
    }
}

```

### Példa: Elfedés (Adattag és statikus metódus elfedése)

```java
class Apple {
    protected static int kgPrice = 200;
    public static int getKgPrice() { return kgPrice; }
}

class GreenApple extends Apple {
    protected static int kgPrice = 500; // Elfedi az Apple kgPrice változóját
    public static int getKgPrice() { return kgPrice; } // Elfedi az Apple statikus metódusát
}

```

---

## 4. A `final` kulcsszó és az Osztályhierarchia Bővítése (`Bus`)

### A `final` kulcsszó 3 szerepe:

1. **Adattag esetén:** Konstans (értéke az inicializálás után nem változtatható meg).
2. **Metódus esetén:** Végleges metódus, a leszármazottakban **nem definiálható felül** (pl. a `Car` osztály `go()` és `fuelNeed()` metódusai, mivel a fizikai haladás minden autónál ugyanúgy működik).
3. **Osztály esetén:** Végleges osztály, **nem lehet belőle tovább örökölni** (nem lehet gyereke).

### Példa: A `Bus` osztály (További leszármaztatás, túlterhelés és `final`)

A `Bus` a `Taxi` osztályból származik le, de végleges (`final`) osztályként. Itt fix vonaljegyár van, a bevétel pedig a jegyárakból mínusz a fenntartási/sofőr költségből (`cost`) áll össze. *(A `Car` osztályban a `carType` mezőt is `protected`-re kell állítani).*

```java
package vehicles;

public final class Bus extends Taxi { // final osztály: nem lehet belőle tovább örökölni
    private static final int ticketPrice = 350; // Konstans osztályváltozó (jegyár)

    public Bus(String busType, double consumption, int capacity, double kmCost) {
        super(busType, consumption, capacity, kmCost);
    }

    // 1. Felüldefiniálás: alapértelmezett fuvar 1 utassal
    @Override
    public double transfer(double km) {
        return transfer(km, 1); // Meghívja a túlterhelt, 2 paraméteres változatot
    }

    // 2. Túlterhelés (Overload): ugyanaz a név, de 2 paramétere van!
    public double transfer(double km, int num) {
        if (go(km) == km) {
            this.balance += profit(km, num);
            return km;
        } else {
            return 0;
        }
    }

    // 3. Felüldefiniálás: a buszon az egy főre jutó költség mindig a fix jegyár
    @Override
    public double costPerPerson(double km, int num) {
        return Bus.ticketPrice;
    }

    // 4. Új metódus: a vállalat profitjának kiszámítása
    public double profit(double km, int num) {
        return num * Bus.ticketPrice - cost(km);
    }

    public static double getTicketPrice() {
        return ticketPrice;
    }

    @Override
    public String toString() {
        return "A busz típusa: " + carType + ", a vállalat pénze: " + balance;
    }
}

```

---

## 5. Generalizálás (Általánosítás) és Absztrakt Osztályok

Amikor észrevesszük, hogy több osztályunk (pl. `Car` és egy új `Bicycle`) közös tulajdonságokkal rendelkezik (típus, kilométeróra, férőhely, haladás), a specializálással ellentétes lépést teszünk: **generalizálunk (általánosítunk)**, és kiemeljük a közös részeket egy közös ősosztályba (`Vehicle`).

### Mi az az Absztrakt Osztály (`abstract`)?

* Egy **hiányosan definiált osztály**, amely a tervezést segíti.
* Tartalmazhat **absztrakt metódusokat** (olyan metódusokat, amelyeknek csak a fejléce van megadva, törzse nincs, mert még nem tudjuk, hogyan fog működni – pl. máshogy halad egy autó, mint egy bicikli).
* **Szabályok:**
* Ha egy osztályban van legalább egy `abstract` metódus, magát az osztályt is **kötelező** `abstract`-nak jelölni.
* Absztrakt osztályt **nem lehet példányosítani** (`new Vehicle()` fordítási hibát ad).
* A nem-absztrakt leszármazott osztály **köteles megvalósítani (implementálni)** az ős összes absztrakt metódusát!


* *IDEA tipp:* Meglévő osztályból a *Refactor -> Extract/Introduce -> Superclass* menüponttal emelhetünk ki könnyen ősosztályt, a *Refactor -> Rename* segítségével pedig biztonságosan átnevezhetjük a változókat (pl. `carType` helyett `type`).

### Példa: A `Vehicle` absztrakt ősosztály és a `Bicycle` leszármazott

**1. Az absztrakt ősosztály (`Vehicle`):**

```java
public abstract class Vehicle {
    protected String type;
    protected double odometer = 0;
    protected int capacity;

    public Vehicle(String type, int capacity) {
        this.type = type;
        this.capacity = capacity;
    }

    // Absztrakt metódus: nincs törzse (kapcsos zárójelek helyett pontosvessző zárja)
    // Minden konkrét járműnek kötelező lesz megírnia a saját 'go' megvalósítását!
    public abstract double go(double km);

    @Override
    public String toString() {
        return type + ", " + odometer + " km, férőhely: " + capacity;
    }
}

```

*(Megjegyzés: Innentől a `Car` osztály definíciója is `public class Car extends Vehicle` lesz, és a közös mezőket onnan kitöröljük, mert örökli őket a `Vehicle`-től).*

**2. Az új, végleges leszármazott (`Bicycle`):**

```java
public final class Bicycle extends Vehicle {

    // 3 paraméteres konstruktor, ami a szülő (Vehicle) konstruktorát hívja
    public Bicycle(String type, int capacity, double odometer) {
        super(type, capacity);
        this.odometer = odometer;
    }

    // 1 paraméteres konstruktor, ami a saját 3 paraméteresének delegál (1 férőhely, 0 km)
    public Bicycle(String type) {
        this(type, 1, 0);
    }

    // KÖTELEZŐ megvalósítani a Vehicle absztrakt go() metódusát!
    @Override
    public double go(double km) {
        this.odometer += km;
        System.out.println("Megtettünk " + km + " km utat biciklivel.");
        return km;
    }
}

```

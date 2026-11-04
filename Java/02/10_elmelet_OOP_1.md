**1. Az Objektumorientált Programozás (OOP) alapjai**

* Az objektumorientált programozás (OOP) alapötlete, hogy a tervezés során a tárgyi világ fogalmait használja fel.
* A feladatok megoldása a felelősségi köröket ellátó objektumok interakciójából áll össze.
* Minden objektum rendelkezik bizonyos tulajdonságokkal (a kód szintjén ezek a változók) és képes bizonyos tevékenységekre (ezek a metódusok).
* Az OOP egy tervezési módszertan, amelyet a legtöbb népszerű nyelv támogat, de anélkül is alkalmazható (például GTK+ esetében), bár úgy nehezebb.

**2. Osztályok és Objektumok a Javában**

* A Java nyelvben egy osztály definíciója és deklarációja (az eljárások feje és megvalósítása) nem választható szét.
* Egy objektum (egyed) egy osztály példányosításával jön létre, melynek során lefoglalásra kerül a memóriaterület és beállításra kerül a referencia.
* A példányosításkor lefut a konstruktor, amely beállítja a kezdőértékeket.
* Inicializálatlan példányváltozók automatikusan `null` értéket kapnak, míg az inicializálatlan lokális változók használata fordításidejű hibát eredményez.
* *Példa objektum létrehozására:* `Car c = new Car();`

**3. Változók és Metódusok**

* Egy osztálynak lehetnek példányváltozói, osztályváltozói (statikus), valamint példány- és osztálymetódusai.
* A változók deklarációja a típus és a név megadásával történik, és inicializálhatók a konstruktoron kívül is.
* A műveleteket az objektumokhoz kapcsolt alprogramokkal, metódusokkal valósítjuk meg.
* *Példa változókra:*
`static private int fuelPrice = 480;` (osztályváltozó) és `private double fuel = 0;` (példányváltozó).
* *Példa metódusra (tankolás):*
```java
public void refuel(double amount) {
  this.fuel += amount;
}

```



**4. A Konstruktor és a `this` kulcsszó**

* A konstruktor neve mindig megegyezik az osztály nevével, és nincsen visszatérési értéke.
* Ha a programozó nem ír konstruktort, a fordító automatikusan létrehoz egy paraméter nélküli változatot.
* Túlterheléssel több konstruktor is létrehozható, illetve a `this()` segítségével az egyik konstruktor meghívhatja egy másik tartalmát (konstruktor delegáció).
* A `this` kulcsszó a példánymetódusokon belül az adott objektumra hivatkozik, és leggyakrabban a lokális változók és példányváltozók megkülönböztetésére használjuk.
* *Példa konstruktorra:*
```java
public Car(String carType, double consumption, int capacity) {
  this.carType = carType;
  this.consumption = consumption;
  this.capacity = capacity;
}

```
* *Példa konstruktor delegációra:*

```java
public class Car {
    String carType;
    double consumption;
    int capacity;

    // 1. Fő konstruktor (mindhárom mezőt beállítja)
    public Car(String carType, double consumption, int capacity) {
        this.carType = carType;
        this.consumption = consumption;
        this.capacity = capacity;
    }

    // 2. Delegáló konstruktor (csak a kocsitípust várja, a többihez alapértéket rendel)
    public Car(String carType) {
        // Itt történik a delegálás: meghívja a fenti, 3 paraméteres konstruktort
        this(carType, 6.5, 5); 
    }
}

```
* Példakód magyarázata

  - Ha egy új autót hozunk létre csak a típusa megadásával (pl. `Car myCar = new Car("Opel Astra");`), a program a **2. konstruktort** futtatja.
  - Ez a konstruktor a `this(carType, 6.5, 5)` hívással továbbadja (delegálja) a munkát az **1. konstruktornak**, belehelyezve az alapértelmezett `6.5`-ös fogyasztást és `5`-ös férőhelyet.
  - **Fontos szabály:** Ha a `this(...)` delegálást használjuk, annak **mindig a konstruktor első sorában** kell szerepelnie!


**5. Adatelrejtés: Getter és Setter műveletek**

* A belső változókat a `private` módosítószóval rejtjük el, hogy védjük az integritást, vagy függetlenítsük a felhasználókat az osztály belső struktúrájától.
* Az elrejtett változók elérését a getter és setter függvények biztosítják; ha egy privát változóhoz nem írunk settert, akkor az írásvédett lesz.
* *Példa getter és setter használatára (beépített ellenőrzéssel):*

```java
// az osztályban deklarálva van egy mező, ami privát
private double consumption;
//...//
// erre a consumption nevű változóra a getter-setter páros
public double getConsumption() {
  return consumption;
}
public void setConsumption (double newConsumption) {
  if (newConsumption <= 0.0) {
    // Hibaüzenet
  } else {
    this.consumption = newConsumption;
  }
}

```



**6. A `toString()` metódus**

* A Javában az `Object` ősosztályból minden objektum örökli a `String toString()` függvényt, amely az objektum szöveges reprezentációját adja meg.
* Ez a metódus felüldefiniálható, és ezt használja például a `System.out.println()` is az objektumok kiíratásához.
* *Példa felüldefiniálásra:*
```java
public String toString() {
  return carType + ", benzin: " + getFuel() + ", km: " + getOdometer();
}

```

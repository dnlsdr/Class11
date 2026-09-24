A Javában a kódunkat meghatározott feladatokat elvégző, újrahasznosítható blokkokba szervezzük. Fontos terminológiai különbség más nyelvekhez képest, hogy Javában hivatalosan csak **metódusok (methods)** léteznek, önálló függvények (functions) nincsenek, mivel minden utasításblokk kötelezően egy osztályhoz (class) tartozik.

### A metódusok felépítése és a visszatérési érték

Egy metódus deklarációja meghatározza annak hozzáférhetőségét, visszatérési típusát, nevét és bemeneti paramétereit. A visszatérési típus (Return Type) mutatja meg, milyen adatot ad vissza a metódus a feladat elvégzése után.

Ha a metódus csupán végrehajt egy cselekvést, de nem generál visszaadandó eredményt, a `void` kulcsszót használjuk.

```java
// Egy egyszerű, visszatérési érték nélküli (void) metódus
public void helloWorld() { 
    System.out.println("Hello World!"); 
}

```

Ha a metódus nem `void`, akkor a kódblokk végén kötelező használni a `return` utasítást a megfelelő típusú (pl. `int`, `String`) adat visszaadására.

### Paraméterek vs. Argumentumok

Bár a két fogalmat a köznyelvben gyakran keverik, programozáskor éles a határvonal:

* **Paraméterek (Parameters):** Ezeket a metódus *definiálásakor* adjuk meg a zárójelek között. Minden paraméternél kötelezően meg kell adni az **adattípust** és a **nevet**. Ezek a változók fogadják be a külső adatokat, amikkel a metódus dolgozni fog.
* **Argumentumok (Arguments):** Ezek a tényleges, konkrét értékek, amelyeket a metódus *meghívásakor* passzolunk át neki.

```java
public class Calculator {
    // Itt az 'a' és 'b' (int típusúak) a paraméterek
    public static int add(int a, int b) {
        return a + b;
    }
}

```

### Metódusok hívása (Invoking/Calling)

Egy metódust a nevével és az azt követő zárójelekkel hívunk meg a kód egy másik pontjáról. Ha a metódus paramétereket vár, a zárójelek között adjuk meg a konkrét argumentumokat.

```java
// Metódus hívása egy másik osztályból. A 34 és az 5 a tényleges argumentum.
int eredmeny = Calculator.add(34, 5);

```

### Fájlszerkezet, Osztályok és a Belépési Pont

Mivel a metódusok osztályokon belül élnek, a Java szigorú szabályokat támaszt a fájlok szerkezetére és az indító metódusra vonatkozóan:

* **Fájl és osztály névkonvenció:** Egy `.java` fájl legfeljebb egyetlen `public` (nyilvános) osztályt tartalmazhat (top-level class). Ennek a nyilvános osztálynak a neve hajszálpontosan meg kell egyezzen a fájl nevével (pl. a `Calculator` osztály a `Calculator.java` fájlban kell legyen).
* **Csomagok (Packages):** Ha több osztály ugyanabba a csomagba tartozik, hozzáférhetnek egymás csomagszintű (package-private) adataihoz. Azonban ez nem jelenti azt, hogy összevonhatod őket egyetlen fájlba; ha mindkettő `public`, külön fájlokban kell élniük.
* **A `main` metódus:** Egy Java program standard belépési pontja (ahol a futás elindul) kötelezően a `public static void main(String[] args)` szignatúrával rendelkezett. Az újabb verziókban elég a `void main()` is, a JVM elindítja azt is ha nem talál klasszikus belépési pontot. 

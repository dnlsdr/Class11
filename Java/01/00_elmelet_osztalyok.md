A legfontosabb különbség, hogy a Java **erősen típusos** (mindennek előre meg kell mondani a típusát) és **sokkal kötöttebb a formátuma** (behúzások helyett kapcsos zárójeleket `{}` használunk).


---

### 1. Osztályok létrehozása

Pythonban egy osztály létrehozása nagyon egyszerű, csak a `class` kulcsszót használod. Javában is a `class` kulcsszót használjuk, de a láthatóságot (pl. `public`) is meg kell adni, és a blokkokat kapcsos zárójelek jelölik.
**Fontos szabály Javában:** Ha az osztályod neve `Kutya`, akkor a fájl nevének kötelezően `Kutya.java`-nak kell lennie!

**Pythonban:**

```python
class Kutya:
    pass # Üres osztály

```

**Javában:**

```java
public class Kutya {
    // Ide jön az osztály tartalma
}

```

*Magyarázat:* A `public` azt jelenti, hogy ez az osztály bárhonnan elérhető a programodban. A kódblokkot a `{` és `}` fogja közre a Python-féle kettőspont és behúzás helyett.

---

### 2. Osztályok használata objektumok létrehozására

Ezt hívjuk példányosításnak. Ilyenkor az "osztály" nevű tervrajzból egy konkrét memóriabeli objektumot csinálunk. Pythonban csak úgy hívtuk az osztályt, mint egy függvényt. Javában bejön a képbe a **`new` kulcsszó**, és a változó típusát is meg kell adnod.

**Pythonban:**

```python
bodri = Kutya()

```

**Javában:**

```java
Kutya bodri = new Kutya();

```

*Magyarázat:*

1. `Kutya bodri`: Megmondod, hogy a `bodri` nevű változóba csak és kizárólag `Kutya` típusú dolgot lehet tenni.
2. `= new Kutya()`: A `new` kulcsszó utasítja a Javát, hogy foglaljon memóriát, és hozza létre az új objektumot.

---

### 3. Az alkalmazás strukturálása csomagokkal

Ahogy nő a programod, nem tarthatsz mindent egy mappában. Pythonban a fájlokat (modulokat) mappákba teszed, és `import`-tal hivatkozol rájuk. Javában ezt a mappaszerkezetet **Package**-nek (csomagnak) hívják.

**Pythonban:**
Volt egy `allatok` mappád, benne a `kutya.py` fájllal. Máshol ezt írtad: `from allatok.kutya import Kutya`.

**Javában:**
A fájl legelső sorában **kötelező** deklarálni, hogy ő melyik csomagban (mappában) van. A mappaszerkezetnek pontosan egyeznie kell a csomagnévvel!

A `Kutya.java` fájl tartalma (ami az `allatok` mappában van):

```java
package allatok; // Ez mondja meg, hol vagyunk

public class Kutya {
}

```

Ha egy másik mappából akarod használni:

```java
import allatok.Kutya; // Így importálod be

public class Allatkert {
    Kutya bodri = new Kutya();
}

```

---

### 4. Osztálytagok hozzáadása

Az osztálytagok a **mezők** (változók, amik az adatokat tárolják) és a **metódusok** (függvények, amik csinálnak valamit).
Pythonban a `__init__` függvényben hoztad létre a változókat a `self`-fel. Javában a változókat az osztály legelején kell deklarálni (típussal együtt!), és a `__init__` megfelelője a **konstruktor** (ami egy olyan függvény, aminek pontosan ugyanaz a neve, mint az osztálynak). Nincs kötelező `self` paraméter sem!

**Pythonban így csináltad:**

```python
class Kutya:
    def __init__(self, nev):
        self.nev = nev # Adat (mező)

    def ugat(self):    # Viselkedés (metódus)
        print("Vau, a nevem " + self.nev)

```

**Javában így fogod:**

```java
public class Kutya {
    // 1. Mezők (adattagok) deklarálása
    String nev;

    // 2. Konstruktor (a Python __init__ megfelelője)
    public Kutya(String nev) {
        this.nev = nev; // A 'this' a Java megfelelője a 'self'-nek
    }

    // 3. Metódus (figyeld meg a 'void' szót: ez jelzi, hogy nem tér vissza értékkel)
    public void ugat() {
        System.out.println("Vau, a nevem " + this.nev); 
        // A System.out.println a Java "print"-je.
    }
}

```


### 5. Az alkalmazás szerkezetének megértése

Pythonban, ha írtál egy print parancsot a fájl közepére behúzás nélkül, a Python simán lefuttatta, amikor elindítottad a fájlt. Vagy használtad az `if __name__ == '__main__':` trükköt.

Javában **SEMMI nem létezhet osztályon kívül**. Nincsenek "szabadon lógó" függvények vagy változók. És a program elindításához pontosan egy darab, nagyon specifikus formátumú belépési pontra van szükség. Ezt hívják **main metódusnak**.

**A Java programod "váza" és elindulása mindig így néz ki:**

```java
public class FoProgram { // A fájl neve FoProgram.java

    // Ez a belépési pont. A Java mindig ezt a sort keresi, hogy elinduljon.
    //
    public static void main(String[] args) {
        
        System.out.println("Elindult a program!");
        
        // Itt hozzuk létre (példányosítjuk) az objektumot az előző leckéből
        Kutya bodri = new Kutya("Bodri"); 
        
        // Itt használjuk a metódusát
        bodri.ugat(); 
    }
}

```

*Magyarázat a main sorhoz (röviden):*

* `public`: Bárki lefuttathatja (a Java futtató környezet).
* `static`: Nem kell létrehozni a `FoProgram`-ból egy példányt (`new FoProgram()`) ahhoz, hogy elinduljon.
* `void`: Nem ad vissza eredményt (nincs return).
* `main`: Ez a neve, a Java konkrétan ezt a szót keresi.
* `String[] args`: Ha parancssorból indítod a programot argumentumokkal, azok ide kerülnek (mint Pythonban a `sys.argv`).

Íme a rövid, lényegretörő összefoglaló arról, hogyan alakult át a Java belépési pontja:

**A modern, új `main`:**
Nincs szükség osztálydeklarációra, és elmarad a sok bonyolult kulcsszó.

```java
void main() {
    IO.println("Hello World!");
}

```

* A változtatás a **Java 21**-ben indult el az *Implicitly Declared Classes* bevezetésével. A kódban látható `IO.println` pedig a legfrissebb, **Java 23**-as újítás. A IntelliJ-ben már alapértelmezetté váltak kezdő projekteknél.

---

### Mi történik a háttérben?

A Java valójában **nem változtatta meg a nyelv alapvető, objektumorientált működését**.

Amikor megírod a fájlba, hogy `void main() { ... }` és rányomsz a futtatásra, a Java fordító (compiler) a háttérben **automatikusan generál köré egy úgynevezett névtelen (vagy implicit) osztályt**.

Tehát a diák nem látja, de a gép számára a háttérben ez a kód pontosan ugyanúgy egy osztályon belüli metódusként fordul le (bájtkóddá), mint a klasszikus verzió.

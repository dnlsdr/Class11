## Hozzáférési módosítók (Access Modifiers)

A hozzáférési módosítók olyan kulcsszavak, amelyekkel szabályozhatjuk az osztályok és az osztálytagok (class members) **láthatóságát (visibility)** és **elérhetőségét (accessibility)** a program többi része számára.

### Mire alkalmazhatók és mire nem?

* **Alkalmazhatók:** Osztályokra (classes), mezőkre/változókra (fields), metódusokra (methods) és konstruktorokra (constructors).
* **NEM alkalmazhatók:** Lokális változókra (amiket egy metóduson belül hozol létre) és konkrét példányokra/objektumokra.
* **Top-level (felső szintű) osztályok megkötése:** Egy önálló `.java` fájlban lévő külső osztály vagy interfész **nem lehet `private`** (és `protected` sem), hiszen akkor kívülről senki sem tudna hozzáférni, így teljesen használhatatlan lenne.

### Enkapszuláció (Egységbe zárás)

Amikor el kell döntened, melyik módosítót használd, az általános szabály az, hogy **mindig a lehető legszigorúbb (most restrictive) hozzáférési módosítót válaszd**, ami az adott feladathoz még megfelelő. Ezzel véded az objektumaid belső állapotát a véletlen vagy illetéktelen külső módosításoktól.

---

## A négy hozzáférési szint (Szigorúsági sorrendben)

A legmegengedőbbtől a legszigorúbbig haladva a sorrend: **`public` ➔ `protected` ➔ `default` (package-private) ➔ `private`**. 

| Módosító | Saját osztályon belül | Ugyanabban a csomagban (Package) | Leszármazott osztályban (másik csomagban) | Bárhol a programban (másik csomagban is) |
| --- | --- | --- | --- | --- |
| **`public`** | Igen | Igen | Igen | Igen |
| **`protected`** | Igen | Igen | **Igen*** *(csak örökléssel)* | Nem |
| **`default`** *(nincs kulcsszó)* | Igen | Igen | Nem | Nem |
| **`private`** | Igen | Nem | Nem | Nem |

### 1. `public` (Nyilvános)

A legkevésbé szigorú szint. A `public` elemek a programon belül bárhonnan elérhetők: ugyanabból az osztályból, ugyanabból a csomagból (package), és teljesen más csomagokból is.

### 2. `protected` (Védett) 
A `protected` mezők és metódusok elérhetők a saját osztályukban, valamint **ugyanazon a csomagon (package) belül bárki számára**. Csomagon kívülről azonban kizárólag a **leszármazott osztályok (subclasses)** férhetnek hozzájuk öröklődés (`extends`) útján.

###
#### 1.  A szülőosztály (`Person.java` az `accessmodifiers` csomagban)



```java
package accessmodifiers;

public class Person {
    protected String name;
    private String secret;

    protected void sayHi() {
        System.out.println("Hello, I'm " + name);
    }

    private void tellSecret() {
        // Csak ezen az osztályon belül érhető el
    }
}

```

#### 2. : A gyerekosztály egy másik csomagban (`App.java`)



```java
import accessmodifiers.Person;

public class App extends Person {
    public static void main(String[] args) {
        Person p = new Person();
        p.name = "Bob"; // HIBA! (Fordítási hiba más csomagból)
        p.sayHi();      // HIBA! (Pirossal aláhúzva a képen)
    }

    public void greeting() {
        sayHi();        // MŰKÖDIK! (Örökölt metódus saját példányon)
        name = "Linda"; // MŰKÖDIK! (Örökölt mező saját példányon)
    }
}

```

---

#### Miért történik ez? 

* **A kiindulási helyzet:** A `Person` osztály az `accessmodifiers` csomagban található, benne a `protected String name;` mezővel és a `protected void sayHi()` metódussal. Az `App` osztály egy **másik csomagban** van (ezt jelzi a fájl tetején lévő `import accessmodifiers.Person;` utasítás), és megörökli a `Person` osztályt (`public class App extends Person`).


* **Miért működik a `greeting()` metódus?** Az `App` osztály `public void greeting()` metódusában közvetlenül meghívjuk a `sayHi();` metódust és beállítjuk a `name = "Linda";` értéket. Ez azért hibátlan, mert az `App` (mint gyerekosztály) megörökölte ezeket a `protected` elemeket, és a saját példányán belül úgy kezeli őket, mintha a sajátjai lennének.


* **Miért hibás a `main`-ben lévő kód?** A `main` metódusban példányosítunk egy szülő objektumot (`Person p = new Person();`), majd megpróbáljuk elérni a `p.name = "Bob";` mezőt és a `p.sayHi();` metódust, amit a fejlesztőkörnyezet hibaként jelez. **A szabály:** Másik csomagban lévő leszármazott osztályból **nem** nyúlhatsz bele közvetlenül egy szülő típusú (`Person`) példány `protected` elemeibe! Csak az öröklésen keresztül (saját magadon vagy egy gyerek, azaz `App` típusú példányon) érheted el őket.



#### Hogyan lehetne kijavítani a `main` metódust?

Ha a `main` metódusban nem a szülőt (`Person`), hanem magát a leszármazottat (`App`) példányosítjuk, a hozzáférés máris szabályossá válik, hiszen az `App` már birtokolja a megörökölt tulajdonságokat:

```java
    public static void main(String[] args) {
        App a = new App(); // Person helyett a gyerekosztályt (App) példányosítjuk!
        a.name = "Bob";    // Így már MŰKÖDIK!
        a.sayHi();         // Így már MŰKÖDIK!
    }

```



### 3. `default` / Package-Private (Alapértelmezett / Csomagszintű)

Ha nem írsz semmilyen kulcsszót a mező vagy metódus elé (pl. `int age;`), akkor az a `default`, másik nevén **Package-Private** láthatóságot kapja. Ez szigorúbb a `protected`-nél: az adott elemhez kizárólag az **ugyanabban a csomagban (package)** lévő osztályok férhetnek hozzá, más csomagból még a leszármazott osztályok sem.

### 4. `private` (Privát) és a Getterek/Setterek

A legszigorúbb szint. A `private` elemek kizárólag abban az egyetlen osztályban láthatók, ahol definiálták őket (ahogy az első képen a `secret` mező és a `tellSecret()` metódus is csak a `Person` osztályon belül létezik).

**Hogyan érhetünk el mégis egy `private` elemet egy másik osztályból?**

1. **Publikus Getterek és Setterek segítségével:** A privát mezőkhöz az osztályon belül írunk `public` olvasó (getter) és módosító (setter) metódusokat.
2. **Publikus közvetítő metódussal:** Egy `private` metódust (mint a `tellSecret()`) meghívhatunk ugyanahhoz az osztályhoz tartozó `public` metóduson belülről.



```java
public class One {
    private String secret = "1234"; // Csak a 'One' osztály látja

    // Public Getter: kívülről ezzel olvasható a privát mező
    public String getSecret() {
        return secret;
    }

    // Public Setter: kívülről ezzel módosítható (ellenőrzötten) a privát mező
    public void setSecret(String newSecret) {
        this.secret = newSecret;
    }

    private void tellSecret() {
        System.out.println("A titok: " + secret);
    }

    // Public metódus, ami belsőleg meghívja a privát metódust
    public void reveal() {
        tellSecret(); // Osztályon belül vagyunk, így ez teljesen szabályos!
    }
}

```

---

## A `static` módosító (Statikus vs. Példányszintű elemek)

A `static` kulcsszó alapjaiban változtatja meg egy mező vagy metódus életciklusát: a `static` tagok **magához az osztályhoz (a tervrajzhoz) tartoznak**, nem pedig az egyes létrehozott objektumokhoz (példányokhoz).

* **Nincs szükség példányosításra:** A statikus metódusok és mezők használatához nem kell objektumot létrehozni (`new` kulcsszóval).
* **Közös (megosztott) adat:** Egy `static` mező (static field) értékén az osztály összes példánya közösen osztozik. Ha az egyik módosítja, az összes többi számára is megváltozik.

### Analógiák a megértéshez

1. **A Kutya (Példány) vs. Kutyatáp (Static) analógia:**
Képzeld el, hogy a `Dog` osztályból létrehozott konkrét kutya (pl. Bodri) a **példány (instance)**. Még mielőtt megszületne vagy vennél egy igazi kutyát (tehát **példányosítás előtt**), már tudsz `static` dolgokat csinálni: elmehetsz a boltba kutyatápot vagy kutyajátékot venni (`Dog.buyFood()`). Viszont **példányosítás nélkül** nem tudsz elmenni sétálni a kutyával (`bodri.walk()`), mert ahhoz már egy konkrét, létező kutyára (non-static példányra) van szükség!
2. **A Tervrajz (Static) és a Ház (Non-Static) analógia – Ki kit hívhat?**
* **Static-ból NEM érhetsz el non-static elemet:** Az osztály a tervrajz (`static`), a megépített ház pedig a példány (`non-static`). A tervrajz tervezője a rajzasztalnál ülve nem nyithatja ki a ház bejárati ajtaját, mert abban a pillanatban lehet, hogy még egyetlen ház sincs megépítve (vagy épp 50 darab van, és nem tudná, melyiknek az ajtaját kell kinyitni).
* **Non-static-ból ELÉRHETSZ static elemet:** Egy már felépített, konkrét házban (példánymetódusban) bármikor megnézheted az eredeti tervrajz adatait (`static` mezőket vagy metódusokat), hiszen a tervrajz már régóta létezik.



### Kódpélda a `static` működésére

```java
public class Dog {
    // STATIC mező: Minden kutya közösen osztozik rajta, az osztályhoz tartozik
    public static int dogFoodCount = 0;

    // NON-STATIC (példányszintű) mező: Minden kutyának saját neve van
    public String name;

    // STATIC metódus: Nem kell hozzá konkrét kutya (példány)
    public static void buyFood(int amount) {
        dogFoodCount += amount;
        System.out.println("Vettünk tápot. Összes táp: " + dogFoodCount);
        
        // HIBA LENNE: System.out.println(name); 
        // (Tervrajz szinten nem tudjuk, melyik kutya nevéről van szó!)
    }

    // NON-STATIC metódus: Csak konkrét kutyán (példányon) hívható meg
    public void walkAndEat() {
        System.out.println(name + " sétálni ment.");
        
        // Non-static metódusból simán elérjük a static mezőt!
        if (dogFoodCount > 0) {
            dogFoodCount--;
            System.out.println(name + " evett a közösből. Maradt: " + dogFoodCount);
        }
    }
}

class Main {
    public static void main(String[] args) {
        // 1. Példányosítás ELŐTT hívunk static metódust az osztály nevével:
        Dog.buyFood(5); 

        // 2. Példányosítunk két konkrét kutyát:
        Dog bodri = new Dog();
        bodri.name = "Bodri";

        Dog rex = new Dog();
        rex.name = "Rex";

        // 3. Példánymetódusok hívása (mindketten a közös static tápot eszik):
        bodri.walkAndEat(); // Maradt: 4
        rex.walkAndEat();   // Maradt: 3
    }
}

```

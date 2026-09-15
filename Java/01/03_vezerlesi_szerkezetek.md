Míg a Python az olvashatóságot kötelező behúzásokkal (szóközökkel) kényszeríti ki, a Javát ez egyáltalán nem érdekli: 
* a logikai blokkokat szigorúan **kapcsos zárójelek** `{ }`, a feltételeket pedig **kerek zárójelek** `( )` határozzák meg.

### 1. Feltételes utasítások (If-Else és Switch)

A Javában nincs `elif` kulcsszó, helyette kiírjuk, hogy `else if`. Ezen felül gyakran használjuk a `switch` szerkezetet, amely Pythonban (sokáig) nem létezett, csak a 3.10-es verzióban jelent meg `match-case` néven.

* **If-else:** A klasszikus elágazás. Figyeld meg a zárójeleket!
```java
int pont = 85;
if (pont >= 90) {
    System.out.println("Ötös");
} else if (pont >= 80) { // Nincs elif!
    System.out.println("Négyes");
} else {
    System.out.println("Közepes vagy rosszabb");
}

```


* **Switch (Többágú elágazás):** Egyetlen változó konkrét értékeinek vizsgálatára való (pl. menüpontok). A `break` kulcsszó hagyományos használatnál kötelező, különben a program "rácsorog" a következő esetre is!
```java
int erdemjegy = 5;
switch (erdemjegy) {
    case 5:
        System.out.println("Kiváló!");
        break;
    case 4:
        System.out.println("Jó!");
        break;
    default: // Olyan, mint egy végső 'else'
        System.out.println("Ez nem ötös vagy négyes.");
}

```
A `switch` utasítás modern, nyíl-alapú (arrow syntax) verzióban is létezik, ez kiküszöböli a kifelejtett `break` utasítások miatti hibákat. Később bemutatásra kerül.


### 2. Ciklusok (While és Do-While)

Ameddig egy feltétel igaz, addig futnak. A `while` teljesen analóg a Pythonnal, de a `do-while` újdonság.

* **While (Elöltesztelő):** Csak akkor lép be a ciklusmagba, ha a feltétel igaz.
```java
int i = 0;
while (i < 3) {
    System.out.println(i);
    i++; // Python: i += 1
}

```


* **Do-While (Hátultesztelő):** Ez a Pythonban **nem létezik**. Különlegessége, hogy a kódblokk *legalább egyszer mindenképpen lefut*, és csak utána ellenőrzi a feltételt. Tökéletes például felhasználói jelszó bekérésére.
```java
int j = 0;
do {
    System.out.println("Ez biztosan lefut egyszer! j = " + j);
    j++;
} while (j < 0); // Bár a feltétel eleve hamis, a blokk egyszer lefutott.

```

```java
import java.util.Scanner;

public class AdatBekeres {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        int jegy; // Csak deklaráljuk, de nem adunk neki kezdőértéket

        do {
            System.out.print("Kérek egy érdemjegyet (1-5): ");
            jegy = input.nextInt(); // Itt kapja meg az értékét
            
            if (jegy < 1 || jegy > 5) {
                System.out.println("Hibás adat! Próbáld újra.");
            }
            
        // A ciklus ADDIG ismétlődik, AMÍG a feltétel IGAZ (vagyis amíg az adat rossz)
        } while (jegy < 1 || jegy > 5); 

        System.out.println("Elfogadva, a megadott jegy: " + jegy);
        input.close();
    }
}
```


### 3. For ciklusok

Pythonban a `for` ciklust szinte mindig a `range()` függvénnyel vagy listák bejárásával használtuk. Javában ezt a két funkciót két külön szintaxis látja el.

* **Hagyományos For (Számláló):** Három részből áll, pontosvesszővel elválasztva: `kezdőérték; meddig menjen; lépésköz`.
```java
for (int k = 0; k < 5; k++) {
    System.out.println("Szám: " + k);
}

```


* **For-each:** Tömbök és gyűjtemények bejárására. Ez áll a legközelebb a Python `for x in lista:` logikájához, csupán az `in` kulcsszó helyett kettőspontot `:` használunk.
```java
String[] nevek = {"Anna", "Béla"};
for (String nev : nevek) {
    System.out.println(nev);
}

```
### 4. Break és continue
A `break` és a `continue` ugyanúgy működik Javában, mint Pythonban!
#### A. A break (Megszakítás)

A break parancs azonnal és végérvényesen kilépteti a programot az aktuális ciklusból. Akkor használjuk, ha elértük a célunkat, vagy egy olyan kritikus feltétel teljesült, ami miatt felesleges (vagy hibás) lenne tovább folytatni a keresést/számolást.

Példa: Egy konkrét szám keresése egy tömbben
Képzeljük el, hogy egy rendszámtáblát vagy egy nyertes sorsjegyet keresünk. Amint megvan, felesleges a többi elemet vizsgálni.

```java
int[] szamok = {12, 45, 87, 23, 9, 56};
int keresettSzam = 23;

for (int i = 0; i < szamok.length; i++) {
    System.out.println("Vizsgálom a(z) " + i + ". indexet...");
    
    if (szamok[i] == keresettSzam) {
        System.out.println("Megvan! A szám a(z) " + i + ". helyen található.");
        break; // Itt azonnal megszakítjuk a ciklust, a 9 és 56 már nem is kerül vizsgálatra
    }
}
```
#### B. A continue (Ugrás a következő lépésre)
A continue nem állítja le az egész ciklust, csupán átugorja a jelenlegi iteráció (lépés) hátralévő részét, és azonnal a következő lépéssel folytatja.
Példa: Csak a páratlan számok kiíratása
Vegyünk egy ciklust, ami 1-től 10-ig megy, de ha a szám páros (vagyis 2-vel osztva 0 a maradéka), akkor átugorjuk a kiíratást.
```java
for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) {
        continue; // Ha páros, a ciklusmag többi része (a kiíratás) elmarad, és jön a következő 'i'
    }
    
    System.out.println("Páratlan szám: " + i);
}
```

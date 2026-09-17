### Statisztikai műveletek
Írjunk egy (külön) Java osztályt, ami képes statisztikai műveleteket végezni egy tömbön, amelynek értékei számok.

* A műveletek:
  * Szélsőérték keresése, átlag.
* Az egyes számításokat külön függvények végezzék.
* A tömböt a konstruktorban adjuk át az osztálynak
* A tömb értékeit a felhasználótól olvassuk be.

Ehhez kis segítség:


```java
import java.util.Scanner;     
// A felhasználói interakcióhoz
// Valahol a mainben:
Scanner input = new Scanner( System.in );         // Bemeneti csatorna objektum
int szam = input.nextInt();                       // Felhasználói input beolvasása
```
* Extra feladat: további műveletek:
  - medián, módusz

Ehhez kis segítség:
* Klónozzuk a tömböt, hogy a rendezés ne módosítsa az eredeti sorrendet -> Arrays.copyOf()
* Rendezéshez: Arrays.sort()

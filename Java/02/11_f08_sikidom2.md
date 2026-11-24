Struktúra:
```
Sokszogek (absztrakt, üres)
├── Haromszog  implements KeruletTeruletSzamithato
└── Negyszog   implements KeruletSzamithato
    └── Teglalap  implements KeruletTeruletSzamithato
        └── Negyzet

Kor  implements KeruletTeruletSzamithato   (nem sokszög, ezért nem a Sokszogek leszármazottja)

KeruletTeruletSzamithato extends KeruletSzamithato

HibasSokszogException (extends Exception)
├── HibasHaromszogException
└── HibasNegyszogException
```
- Adott egy ősosztály Sokszogek:
  - Absztrakt osztály, nincs benne semmi. Csak leszármazottai lesznek.
  - *Nem tartalmaz sem adattagot, sem metódust, csak közös típusként szolgál, hogy a különféle sokszögeket egyetlen
 tömbben lehessen tárolni a Main-ben.*
- Adott két interfész: KeruletSzamithato:
  - Egy függvénye van: float computeKerulet();
- És egy másik: KeruletTeruletSzamithato:
  - float computeTerulet();
  - A KeruletSzamithato interfészből származik, így a kerület számítását is előírja.
  - *Tehát olyan alakzatokat ír le, amelyeknek a kerülete és a területe is kiszámítható.*
- Maga a feladat: Hozz létre egy háromszög, és egy négyszög osztályt. Mindkét osztály a sokszög oldalait tartalmazza, ezen adatok alapján valósítsa meg mindegyikhez a fentiek közül értelmezhető interfészt.
- Ha hibás adatot kap egy sokszög, vagyis, ha olyan oldalhosszok érkeznek, melyekből nem lehet az adott sokszöget megrajzolni (pl.: háromszög – háromszög-egyenlőtlenség), azt kezelje kivételekkel. Legyen egy olyan kivétel, ami minden sokszögre használható, és ebből származóan legyen egy-egy az egyes sokszögtípusokra is külön-külön.
- Ezek után tesztelj a megkapott Main osztállyal, amely létrehoz egy sokszögekből álló tömböt. Teszteli és megmutatja az osztályaid működését. Figyeld meg a kivételkezelést: az elkapás sorrendje a specifikusabbtól az általánosabb felé halad!
- Hozz létre egy kör osztályt is! Ez is valósítsa meg a megfelelő interfészt! Ezt is teszteld le!
- Main osztály:
```java
package OOP_11_f08_sikidom_interface;

/**
 * A sokszög-hierarchia bemutató- és tesztprogramja.
 */
public class Main {

    /**
     * A sokszögeket tároló tömb mérete.
     */
    private static final int TOMB_MERET = 10;

    /**
     * A sokszögek tömbje.
     */
    private static final Sokszogek[] sokszogek = new Sokszogek[TOMB_MERET];

    /**
     * A tömbben lévő érvényes elemek száma.
     */
    private static int darab = 0;

    /**
     * A program belépési pontja.
     *
     * @param args parancssori argumentumok (nem használja)
     */
    public static void main(String[] args) {
        System.out.println("=== 1. Sokszögek létrehozása ===");

        // Háromszögek: a specifikus kivételt kapjuk el 
        try {
            hozzaad(new Haromszog(3, 4, 5));                  // derékszögű
        } catch (HibasHaromszogException e) {
            System.out.println("  Hibás háromszög: " + e.getMessage());
        }
        try {
            hozzaad(new Haromszog(2, 2, 2));                  // szabályos
        } catch (HibasHaromszogException e) {
            System.out.println("  Hibás háromszög: " + e.getMessage());
        }
        try {
            hozzaad(new Haromszog(1, 2, 10));                 // HIBÁS: egyenlőtlenség
        } catch (HibasHaromszogException e) {
            System.out.println("  Hibás háromszög: " + e.getMessage());
        }
        try {
            hozzaad(new Haromszog(1, 2, 3));                  // HIBÁS: elfajuló (1+2=3)
        } catch (HibasHaromszogException e) {
            System.out.println("  Hibás háromszög: " + e.getMessage());
        }
        try {
            hozzaad(new Haromszog(-3, 4, 5));                 // HIBÁS: negatív oldal
        } catch (HibasHaromszogException e) {
            System.out.println("  Hibás háromszög: " + e.getMessage());
        }

        // Vegyes blokk: több catch ág, a specifikustól az általános felé
        try {
            hozzaad(new Negyszog(3, 4, 5, 6));
            hozzaad(new Teglalap(2, 5));
            hozzaad(new Negyzet(4));
            hozzaad(new Negyszog(1, 1, 1, 10));               // HIBÁS: 10 >= 1+1+1
            hozzaad(new Haromszog(1, 1, 5));                  // ide már nem jut el
        } catch (HibasNegyszogException e) {
            System.out.println("  Hibás négyszög: " + e.getMessage());
        } catch (HibasSokszogException e) {
            System.out.println("  Egyéb hibás sokszög: " + e.getMessage());
        }

        // Az általános kivétel bármelyik sokszög hibáját elkapja 
        try {
            hozzaad(new Teglalap(0, 3));                      // HIBÁS: 0 hosszú oldal
        } catch (HibasSokszogException e) {
            System.out.println("  Hibás sokszög (" + e.getClass().getSimpleName() + "): "
                    + e.getMessage());
        }
        try {
            hozzaad(new Haromszog(5, 5, 8));
        } catch (HibasSokszogException e) {
            System.out.println("  Hibás sokszög (" + e.getClass().getSimpleName() + "): "
                    + e.getMessage());
        }

        System.out.println();
        System.out.println("2. A tömb tartalma (" + darab + " / " + TOMB_MERET + " elem)");
        for (int i = 0; i < darab; i++) {
            kiir(sokszogek[i]);
        }

        System.out.println();
        System.out.println("3. Összesítés");
        float osszKerulet = 0;
        for (int i = 0; i < darab; i++) {
            // Az üres ősosztály miatt instanceof-fal ellenőrizzük a képességeket
            if (sokszogek[i] instanceof KeruletSzamithato) {
                osszKerulet += ((KeruletSzamithato) sokszogek[i]).computeKerulet();
            }
        }
        System.out.printf("  Kerületek összege: %.2f%n", osszKerulet);

        System.out.println();
        System.out.println("=== 4. Kör tesztelése ===");
        float[] sugarak = {1, 2.5f, 0, -4};
        for (float r : sugarak) {
            try {
                Kor kor = new Kor(r);
                kiir(kor);
            } catch (IllegalArgumentException e) {
                System.out.println("  Hibás kör: " + e.getMessage());
            }
        }

        System.out.println();
        System.out.println("5. Közös interfész: háromszög, téglalap és kör együtt");
        try {
            KeruletTeruletSzamithato[] alakzatok = {
                    new Haromszog(6, 8, 10),
                    new Teglalap(3, 7),
                    new Kor(3)
            };
            for (KeruletTeruletSzamithato alakzat : alakzatok) {
                kiir(alakzat);
            }
        } catch (HibasSokszogException e) {
            System.out.println("  Hibás sokszög: " + e.getMessage());
        }
    }

    /**
     * Beteszi a sokszöget a tömb következő szabad helyére, ha van még hely.
     *
     * @param s a hozzáadandó sokszög
     */
    private static void hozzaad(Sokszogek s) {
        if (darab >= sokszogek.length) {
            System.out.println("  A tömb megtelt, nem fér bele: " + s);
            return;
        }
        sokszogek[darab++] = s;
        System.out.println("  Hozzáadva: " + s);
    }

    /**
     * Kiírja az objektum leírását, valamint a kerületét és a területét,
     * attól függően, hogy melyik interfészt valósítja meg.
     *
     * @param o a kiírandó objektum
     */
    private static void kiir(Object o) {
        StringBuilder sb = new StringBuilder("  ").append(o);
        if (o instanceof KeruletSzamithato) {
            sb.append(String.format(" | K = %.2f", ((KeruletSzamithato) o).computeKerulet()));
        }
        if (o instanceof KeruletTeruletSzamithato) {
            sb.append(String.format(" | T = %.2f", ((KeruletTeruletSzamithato) o).computeTerulet()));
        } else {
            sb.append(" | T = nem számítható (az oldalak nem határozzák meg egyértelműen)");
        }
        System.out.println(sb);
    }
}

```
- Kimenet:
```
1. Sokszögek létrehozása
  Hozzáadva: Háromszög(a=3,00, b=4,00, c=5,00)
  Hozzáadva: Háromszög(a=2,00, b=2,00, c=2,00)
  Hibás háromszög: A(z) 1,00, 2,00, 10,00 oldalakra nem teljesül a háromszög-egyenlőtlenség.
  Hibás háromszög: A(z) 1,00, 2,00, 3,00 oldalakra nem teljesül a háromszög-egyenlőtlenség.
  Hibás háromszög: A háromszög oldalai csak pozitív számok lehetnek (-3,00, 4,00, 5,00).
  Hozzáadva: Négyszög(a=3,00, b=4,00, c=5,00, d=6,00)
  Hozzáadva: Téglalap(2,00 x 5,00)
  Hozzáadva: Négyzet(a=4,00)
  Hibás négyszög: A(z) 1,00, 1,00, 1,00, 10,00 oldalakból nem szerkeszthető négyszög: a(z) 10,00 oldal nem kisebb a többi három összegénél.
  Hibás sokszög (HibasNegyszogException): A négyszög oldalai csak pozitív számok lehetnek (0,00, 3,00, 0,00, 3,00).
  Hozzáadva: Háromszög(a=5,00, b=5,00, c=8,00)

2. A tömb tartalma (6 / 10 elem)
  Háromszög(a=3,00, b=4,00, c=5,00) | K = 12,00 | T = 6,00
  Háromszög(a=2,00, b=2,00, c=2,00) | K = 6,00 | T = 1,73
  Négyszög(a=3,00, b=4,00, c=5,00, d=6,00) | K = 18,00 | T = nem számítható (az oldalak nem határozzák meg egyértelműen)
  Téglalap(2,00 x 5,00) | K = 14,00 | T = 10,00
  Négyzet(a=4,00) | K = 16,00 | T = 16,00
  Háromszög(a=5,00, b=5,00, c=8,00) | K = 18,00 | T = 12,00

3. Összesítés
  Kerületek összege: 84,00

4. Kör tesztelése
  Kör(r=1,00) | K = 6,28 | T = 3,14
  Kör(r=2,50) | K = 15,71 | T = 19,63
  Hibás kör: A kör sugara csak pozitív szám lehet (kapott: 0,00).
  Hibás kör: A kör sugara csak pozitív szám lehet (kapott: -4,00).

5. Közös interfész: háromszög, téglalap és kör együtt
  Háromszög(a=6,00, b=8,00, c=10,00) | K = 24,00 | T = 24,00
  Téglalap(3,00 x 7,00) | K = 20,00 | T = 21,00
  Kör(r=3,00) | K = 18,85 | T = 28,27
```

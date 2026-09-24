### Iskolai napló (Tömbök, Típuskonverzió, Metódusok, Ciklusok)

Feladat: Készíts egy programot, amely egy diák jegyeit elemzi!


Lépések:
* Hozz létre egy grades nevű package-t, az osztályokat ebben hozd létre.
* Hozz létre egy GradeAnalyzer osztályt!


* Írj benne egy calculateAverage nevű statikus metódust, amely egy int[] tömböt vár, és double értékkel tér vissza (figyelni kell a helyes típuskonverzióra az osztásnál, hogy ne vesszenek el a tizedesek)!


* Írj egy countFails nevű statikus metódust is, amely szintén megkapja a tömböt, és visszaadja (return), hány darab 1-es található benne (for ciklus és if feltétel)!


* A Main osztályban hozz létre egy tömböt tesztadatokkal, hívd meg mindkét metódust, és írasd ki az eredményeket!
* A tesztadatok: 4, 5, 1, 3, 2, 1, 4, 5
* A várható output: 
  - Tanulmányi átlag: 3.125
  - Elégtelen osztályzatok száma: 2

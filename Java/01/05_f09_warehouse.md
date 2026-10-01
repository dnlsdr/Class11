### Raktárkészlet szűrése
Feladat: Keress meg kritikus termékeket egy raktárban!
Lépések:
* Hozz létre egy warehouse nevű package-t, a továbbiakban ebben dolgozz!
* Hozz létre egy Product osztályt price (double) és inStock (int, darabszám) mezőkkel!
* A Main osztályban hozz létre egy Product[] tömböt, amely 5 különböző terméket tartalmaz (példányosítsd és töltsd fel őket adatokkal)!
* Írj egy statikus metódust findCriticalProducts néven, amely paraméterként megkapja ezt a Product[] tömböt!
* A metóduson belül egy ciklussal nézd meg az összes terméket! Ha egy termék készlete 0, VAGY az ára magasabb mint 10 000, írasd ki a sorszámát (indexét) egy figyelmeztető üzenettel! 
* Várható kimenet:
```
--- Kritikus termékek listája ---
Figyelmeztetés a(z) 1. indexű terméknél! Készlet: 10 db, Ár: 12000.0 Ft
Figyelmeztetés a(z) 2. indexű terméknél! Készlet: 0 db, Ár: 800.0 Ft
Figyelmeztetés a(z) 3. indexű terméknél! Készlet: 0 db, Ár: 15000.0 Ft
```

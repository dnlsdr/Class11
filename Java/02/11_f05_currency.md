### 1. Feladat: Valutaváltó Alapok (Struktúra és Interfészek)

**Cél:** Az osztályhierarchia, az absztrakt osztályok és az interfészek (szerződések) megértése és alkalmazása.

**Feladat:**
Készíts egy alap valutaváltó alkalmazást!

* Hozd létre a `Currency` (Valuta) absztrakt ősosztályt!
* Legyen egy `final double` mezője, ami az adott valuta aktuális árfolyamát tárolja (1 egység ára HUF-ban).
* Egy másik `double` mezője a pénznemből nálunk lévő készletet tárolja (pl. 150 USD). Ennek alapértékét a konstruktorban lehessen megadni, az árfolyammal együtt.


* Hozz létre két interfészt a tranzakciókhoz:
* `BuyableCurrency`: egy függvénye legyen, a `buyCurrency(double hufAmount)`. Ezzel tudunk HUF-ot a külföldi pénznembe váltani.
* `SellableCurrency`: egy függvénye legyen, a `sellCurrency(double foreignAmount)`. Ezzel tudjuk a külföldi pénznemet HUF-ba váltani.
* Mindkét függvény `double` típusú visszatérési értékkel rendelkezzen (a kapott összeg).


* Minden külföldi pénznemet külön osztályként hozz létre (USD, GBP, EUR, CHF, JPY), amik a `Currency` osztályból örökölnek!
* **Árfolyamok:** 1 USD = 297,00 HUF; 1 EUR = 360,01 HUF; 1 CHF = 326,67 HUF; 1 GBP = 419,12 HUF; **1000 JPY = 2800,75 HUF** *(Figyelj a japán jen váltószámára!)*.
* **Interfészek implementálása:**
* Az USD, EUR és CHF osztályok mindkét interfészt implementálják.
* Angol fontot (GBP) a cég nem akar eladni (az ügyfél nem tud venni), így az **csak** a `SellableCurrency` interfészt implementálja.
* Japán jent (JPY) a cég nem akar venni (az ügyfél nem tudja HUF-ra váltani), így az **csak** a `BuyableCurrency` interfészt implementálja.




* Írj egy egyszerű `Main` osztályt, ahol példányosítod a valutákat, és kipróbálod a váltásokat (csak kiíratással, egyelőre hibakezelés nélkül)!

```
--- VALUTAVÁLTÓ MEGNYITOTT ---
Elérhető USD, EUR, GBP, JPY

--- USD Tranzakciók ---
Ügyfél 50000.0 Ft-ért kapott: 168,35 USD-t.

--- EUR Tranzakciók ---
Ügyfél 50000.0 Ft-ért kapott: 138,89 USD-t.
Ügyfél 100.0 USD eladásáért kapott: 29700 Ft-ot.

--- GBP Tranzakciók ---
Ügyfél 50.0 GBP eladásáért kapott: 20956 Ft-ot.

--- JPY Tranzakciók ---
Ügyfél 10000.0 Ft-ért kapott: 3570 JPY-t.
```

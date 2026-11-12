### Valutaváltó Haladó (Üzleti logika és Kivételkezelés)

**Cél:** Az előző feladat kódjának kiterjesztése egyedi üzleti szabályokkal, metódusok felüldefiniálásával és saját hibakezeléssel (Custom Exceptions).

**Feladat:**
Folytasd az előzőleg elkészített valutaváltó alkalmazást!

* **Saját kivételek létrehozása:**
* Hozz létre egyedi kivételosztályokat (pl. `LimitExceededException` és `CurrencyDisabledException`), amelyek az `Exception` (vagy `RuntimeException`) osztályból örökölnek!


* **Készletkezelés bevezetése:**
* A tranzakciók (`buyCurrency` és `sellCurrency`) metódusai ne csak számoljanak, hanem vonják is le / adják is hozzá a kiszámolt összeget a `Currency` ősosztályban tárolt készletből! Ha a készlet alá menne a tranzakció (nincs elég valuta a váltóban), dobjon egy `IllegalArgumentException`-t!


* **Egyedi szabályok az osztályokban:**
* **USD (Limitált eladás):** Semmiképpen sem akarunk 1000 USD-nél többet eladni egy alkalommal! Írd felül a HUF -> USD váltást végző metódust: ha az ügyfél több mint 1000 USD-nek megfelelő forintot adna meg, dobj egy `LimitExceededException` kivételt!
* **CHF (Kapcsolható kereskedés):** Néha szeretnénk letiltani a svájci frank mozgását.
* A `CHF` osztály kapjon egy `boolean enabled` adattagot (alapból `true`).
* Legyen egy `isEnabled()` getter függvénye.
* Legyen egy `trigger()` paraméter nélküli függvénye, ami kapcsolóként működik (ellenkezőjére állítja az `enabled` értékét).
* Írd felül a tranzakciós metódusait: ha az `enabled` hamis, dobjon `CurrencyDisabledException`-t bármilyen váltási kísérletnél!


* **Kibővített tesztelés (`Main` osztály):**
* Írj egy értelmes tesztelőt! Próbálj meg sikeres tranzakciókat végrehajtani, majd szándékosan idézd elő a hibákat (túl sok USD váltása, CHF tiltása utáni váltás, készlet kimerítése).
* A hibákat `try-catch` blokkokkal kapd el, és a konzolra írj ki felhasználóbarát hibaüzeneteket a program összeomlása helyett!

```
  --- 1. SIKERES TRANZAKCIÓ ---
Sikeres váltás! Kaptál: 200.0 USD-t.
Maradék USD készlet a bankban: 300.0

--- 2. KÉSZLETHIÁNY TESZTELÉSE ---
Kísérlet 400 USD vásárlására...
VÁRT HIBA ELKAPVA: Nincs elég készlet a valutából! Kért: 400.0, Elérhető: 300.0

--- 3. USD LIMIT TESZTELÉSE ---
Kísérlet 1500 USD vásárlására nagy bankból...
VÁRT HIBA ELKAPVA (Üzleti szabály): Tranzakció elutasítva! Egyszerre maximum 1000 USD adható el.

--- 4. CHF LETILTÁS TESZTELÉSE ---
CHF kereskedés állapota megváltozott: TILTOTT
Kísérlet CHF vásárlására...
VÁRT HIBA ELKAPVA: A svájci frank (CHF) eladása jelenleg szünetel!
```

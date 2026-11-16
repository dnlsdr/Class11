### 11_f05_currency feladat folytatása

### 2. Feladat: Valutaváltó Haladó

**Cél:** Az 1. feladatban felépített osztályhierarchia kiterjesztése állapotkezeléssel (készlet és forintkassza), polimorf módon kezelt valutákkal, valamint adatokkal ellátott saját kivételekkel, amelyekből a hívó fél érdemben helyre tudja állítani a tranzakciót.

#### 1. Saját kivételek tervezése (Állapottal és metódusokkal)

Hozz létre két ellenőrzött kivételosztályt (`extends Exception`), amelyek nemcsak üzenetet, hanem **konkrét üzleti adatokat** is tárolnak, hogy a `catch` blokk tudjon belőlük számolni:

* **`InsufficientInventoryException` (Készlethiány):**
  - Tárolja el a valuta kódját (`String currencyName`), a kért mennyiséget (`double requested`) és az aktuálisan elérhető készletet (`double available`).
  - Legyen egy `getAvailable()` és egy `getMissingAmount()` metódusa!


* **`LimitExceededException` (Limit túllépés):**
  - Tárolja el a megengedett maximális devizamennyiséget (`double maxLimit`) és a kért mennyiséget (`double requested`).
  - Legyen egy `getMaxLimit()` metódusa!


* **`CurrencyDisabledException` (Tiltott valuta):**
  - Tárolja el a letiltott valuta nevét (`String currencyName`), és legyen hozzá getter!



#### 2. A `Currency` ősosztály és az interfészek továbbfejlesztése

* **Interfészek frissítése:** Mind a `BuyableCurrency.buyCurrency(...)`, mind a `SellableCurrency.sellCurrency(...)` deklarációja jelezze a `throws Exception` lehetőséget!
* **Közös forintkassza (`static`):** A `Currency` osztály kapjon egy osztályszintű (`private static double hufBalance`) mezőt (pl. induljon `1_000_000.0` Ft-tal), valamint hozzá tartozó statikus gettert (`getHufBalance()`). Ha az ügyfél devizát vesz, a forintkassza nő; ha devizát ad el, a forintkassza csökken!
* **Készletmozgató metódusok (`protected`):**
  - `decreaseInventory(double amount)`: Ha `amount > inventory`, dobjon `InsufficientInventoryException`-t a pontos adatokkal! Különben vonja le az `inventory`-ból, és növelje a közös `hufBalance`-ot a váltott forintösszeggel!
  - `increaseInventory(double amount)`: Növelje az `inventory`-t, és vonja le a kifizetett forintot a `hufBalance`-ból (ha a forintkassza nem elég a kifizetéshez, itt is dobhat `InsufficientInventoryException`-t `"HUF"` névvel)!


* **Szöveges megjelenítés (`toString`):** A `Currency` osztály írja felül a `toString()` metódust, hogy kiírja a valuta nevét (osztálynevét), az árfolyamot és a fiókban maradt készletet! *Osztálynév kiíratása:* this.getClass().getSimpleName()

#### 3. Speciális üzleti szabályok a valutákban

* **USD (Egyszeri vásárlási limit):**
  - Egyszerre legfeljebb **1000 USD**-t adunk el egy ügyfélnek.
  - Ha a beadott HUF több mint 1000 USD-nek felel meg, a `buyCurrency` dobjon `LimitExceededException`-t (`maxLimit = 1000.0`) még a készletlevonás előtt!


* **CHF:**
  - Kapjon egy `private boolean enabled = true;` adattagot, egy `isEnabled()` gettert, és egy `trigger()` kapcsoló metódust (ami megfordítja az `enabled` értékét).
  - Ha `enabled == false`, mindkét tranzakciós irány (`buyCurrency` és `sellCurrency`) dobjon `CurrencyDisabledException`-t!


* **JPY és EUR:** Használják az ősosztály biztonságos készletkezelő metódusait (a `GBP` maradjon csak eladható, a `JPY` pedig csak vehető).

#### 4. Tesztelő `Main` osztály

Írj egy `Main` osztályt, amely bemutatja a rendszer működését, és **nem csak kinyomtatja a hibákat**, hanem reagál is rájuk:

1. **Kezdőállapot:** Példányosítsd az összes valutát (`USD`, `EUR`, `CHF`, `GBP`, `JPY`), írasd ki a fiók nyitókészleteit és a forintkasszát! A kiíratáshoz használj Currency típusú tömböt, ebben tárold a különböző valutapéldányokat, majd for ciklussal írasd ki.
2. Példanyosításhoz: 
```
        USD usd = new USD(1500.0);
        EUR eur = new EUR(500.0);
        CHF chf = new CHF(800.0);
        GBP gbp = new GBP(400.0);
        JPY jpy = new JPY(200_000.0);
```
3. **Normál tranzakciók:** Végezz el egy sikeres EUR vételt és eladást, egy GBP eladást, valamint egy JPY vételt!
4. **USD limit kezelése (`LimitExceededException`):**
* Egy ügyfél `400 000 Ft`-ért szeretne USD-t venni (ami ~1346 USD, túllépi az 1000 USD limitet).
* A `catch` blokkban kérd le a kivételből a maximumot (`e.getMaxLimit()`), vásárold meg helyette pontosan a maximumot (`maxLimit * usd.getRate()`), és írasd ki, mennyi forint visszajárót kap az ügyfél!


4. **Készlethiány kezelése (`InsufficientInventoryException`):**
* Próbálj meg annyi EUR-t venni, amennyi már nincs a kasszában!
* A `catch` blokkban olvasd ki az `e.getAvailable()` és `e.getMissingAmount()` értékeket: add el az ügyfélnek a teljes maradék készletet, és jelezd, mennyit kell rendelni a központból!


5. **CHF tiltás tesztelése (`CurrencyDisabledException`):**
* Hívd meg a `chf.trigger()` metódust, próbálj meg CHF-et váltani, kapd el a kivételt, majd kapcsold vissza (`chf.trigger()`) és hajtsd végre újra sikeresen a tranzakciót!


6. **Záróegyenleg:** Írasd ki a nap végi készleteket és a forintkassza egyenlegét!

---

### Várható minta kimenet (részlet)

```text
=== VALUTAVÁLTÓ MEGNYITOTT (Forintkassza: 1000000,0 Ft) ===
USD | Árfolyam: 297,00 Ft | Készlet: 1500,00
EUR | Árfolyam: 360,01 Ft | Készlet: 500,00
CHF | Árfolyam: 326,67 Ft | Készlet: 800,00
GBP | Árfolyam: 419,12 Ft | Készlet: 400,00
JPY | Árfolyam: 2,80 Ft | Készlet: 200000,00

--- 1. Normál tranzakciók ---
Ügyfél 50000.0 Ft-ért kapott: 138,89 EUR-t.
Ügyfél 100.0 EUR eladásáért kapott: 36001 Ft-ot.
Ügyfél 50.0 GBP eladásáért kapott: 20956 Ft-ot.
Ügyfél 10000.0 Ft-ért kapott: 3570 JPY-t.

--- 2. USD Limit teszt (400 000 Ft váltása) ---
[FIGYELEM] Egyszerre max 1000,00 USD vehető!
-> Részleges teljesítés: 1000,00 USD kiadva, visszajáró: 103000,0 Ft.

--- 3. EUR Készlethiány teszt (200 000 Ft váltása) ---
[KÉSZLETHIÁNY] Nincs elég EUR (hiányzik: 94,43 EUR).
-> Maradék teljes készlet (461,11 EUR) kiadva az ügyfélnek!

--- 4. CHF Tiltás teszt ---
[TILTOTT] A(z) CHF kereskedése jelenleg fel van függesztve!
-> Adminisztrátor visszakapcsolja a CHF kereskedést...
-> Sikeres pótlólagos váltás: 100,00 CHF kiadva.

=== NAP VÉGI ZÁRÁS (Forintkassza: 1498716,0 Ft) ===
USD | Árfolyam: 297,00 Ft | Készlet: 500,00
EUR | Árfolyam: 360,01 Ft | Készlet: 0,00
CHF | Árfolyam: 326,67 Ft | Készlet: 700,00
GBP | Árfolyam: 419,12 Ft | Készlet: 450,00
JPY | Árfolyam: 2,80 Ft | Készlet: 196429,53

```

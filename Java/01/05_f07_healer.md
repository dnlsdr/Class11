### Játékos gyógyítása (Objektumok, Pass by Value mechanika)
Feladat: Szimulálj egy egyszerű játékmechanikát, ahol egy karakter életerőt kap!
Lépések:
* Hozz létre egy healer nevű package-t, a továbbiakban ebben dolgozz
* Hozz létre egy Player osztályt két int mezővel: health (aktuális életerő) és maxHealth (maximális életerő).
* Hozz létre egy Healer osztályt, benne egy statikus healPlayer metódussal! A metódus várjon egy Player objektumot és egy int healAmount (gyógyítás mértéke) paramétert.
* A metóduson belül növeld meg a kapott játékos health értékét. Használj if-else szerkezetet annak biztosítására, hogy az új életerő ne lépje túl a maxHealth határt!
* A Main osztályban példányosíts egy Player-t, állítsd be az értékeit, majd hívd meg rá a healPlayer metódust, és írasd ki az életerőt előtte és utána!
* A várható output:
   - Gyógyítás ELŐTT az életerő: 50
   - Első gyógyítás UTÁN az életerő: 80
   - Második (túlgyógyító) hívás UTÁN az életerő: 100

Ez a feladat a String alapvető ellenőrző és tisztító metódusait (trim, toLowerCase, length, indexOf, equals) kombinálja a hozzáférési módosítókkal (public és private metódusok) és a vezérlési szerkezetekkel.

Lépések:
* Hozz létre egy user_account nevű csomagot, a továbbiakban ebben dolgozz!
* Hozz létre egy UserAccount nevű osztályt két private String mezővel: username és password!
* Írj egy private String cleanUsername(String rawName) segédmetódust, amely eltávolítja a kapott név elejéről és végéről a felesleges szóközöket (trim()), majd az egészet kisbetűssé alakítja (toLowerCase()), és visszaadja az új String-et!
* Írj egy private boolean isPasswordStrong(String pass) segédmetódust, amely ellenőrzi a jelszót! Akkor térjen vissza true értékkel, ha:
  - A jelszó hossza (length()) legalább 8 karakter, ÉS
  - Nem tartalmaz szóközt (tipp: az indexOf(" ") metódus -1-et ad vissza, ha a keresett karakter nem szerepel a szövegben).
* Írj egy public boolean register(String rawUser, String rawPass) metódust!
  - Először tisztítsa meg a felhasználónevet a cleanUsername metódussal!
  - Ha a megtisztított név túl rövid (kevesebb mint 3 karakter), vagy a jelszó nem elég erős (!isPasswordStrong(rawPass)), írjon ki hibaüzenetet, és térjen vissza false értékkel!
  - Ha minden rendben, mentse el az értékeket a private mezőkbe, írja ki a regisztrált (megtisztított) felhasználónevet, és térjen vissza true-val!
* Írj egy public void login(String enteredUser, String enteredPass) metódust! Tisztítsa meg a belépéskor megadott nevet is, majd az .equals() metódussal hasonlítsa össze a tárolt username és password mezőkkel! Írja ki, hogy sikeres volt-e a belépés!
* A Main osztályban példányosíts egy UserAccount-ot, próbálj meg regisztrálni túl rövid/szóközös jelszóval, majd egy szabályossal (pl. " KovacsJanos " és "Titkos123"), végül teszteld a login metódust!
  - például:
```java
    UserAccount user = new UserAccount();
    user.register("   ab   ", "Titkos1234");
```
  - lehetséges output:
```
--- 1. Regisztrációs tesztek ---
Regisztrációs hiba: A felhasználónév túl rövid (min. 3 karakter)!
Regisztrációs hiba: A jelszó túl gyenge (min. 8 karakter, szóköz nélkül)!
Regisztrációs hiba: A jelszó túl gyenge (min. 8 karakter, szóköz nélkül)!
Sikeres regisztráció! Felhasználónév: 'kovacsjanos'

--- 2. Belépési tesztek ---
Belépés megtagadva: Hibás felhasználónév vagy jelszó!
Sikeres belépés! Üdvözöljük, kovacsjanos!
```
    


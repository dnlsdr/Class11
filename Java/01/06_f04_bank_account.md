### **Védett Bankszámla (A `private` és `public` módosítók, vezérlési szerkezetek)**
Ez a feladat megmutatja, miért veszélyes minden mezőt `public`-ra állítani, és hogyan védhetjük meg az adatokat `public` metódusok (getter/setter jellegű logika) valamint egy `private` segédmetódus segítségével.

**Lépések:**


0. Hozz létre egy bank nevű csomagot, a továbbiakban ebben dolgozz!
1. Hozz létre egy `BankAccount` nevű osztályt!
2. Az osztályon belül hozz létre három mezőt:
* `public int accountNumber` (számlaszám – bárki láthatja)
* `private double balance` (egyenleg – kívülről közvetlenül nem módosítható)
* `private int pinCode` (PIN-kód – szigorúan titkos)


4. Írj egy `public void setupAccount(int accNum, int pin, double initialBalance)` metódust, amellyel beállíthatod a számla kezdőértékeit!
4. Írj egy `private boolean isPinValid(int enteredPin)` segédmetódust, amely visszaadja (`true` vagy `false`), hogy a paraméterként kapott PIN megegyezik-e a tárolt `pinCode` mezővel!
5. Írj egy `public void withdraw(int enteredPin, double amount)` metódust! Ez a metódus először hívja meg a `private` láthatóságú `isPinValid` metódust:
* Ha a PIN hibás, írja ki: „Hibás PIN-kód!”
* Ha a PIN helyes, ellenőrizze (`if-else`), hogy van-e elég fedezet (`balance >= amount`). Ha igen, vonja le az összeget és írja ki az új egyenleget, különben írjon ki hibaüzenetet!


6. Hozz létre egy `Main` osztályt a `main` metódussal, példányosíts egy `BankAccount` objektumot!
7. Próbáld meg a `Main`-ből közvetlenül módosítani a `balance` mezőt (`account.balance = 1000000;`) vagy meghívni az `isPinValid` metódust! Figyeld meg a fordítási hibát, majd kommentezd ki a hibás sorokat, és használd a `setupAccount` és `withdraw` metódusokat a szabályos működéshez!
8. Egy lehetséges konzolos kimenet:
```
Számlaszám: 112233
-----------------------------------
Tranzakció elutasítva: Hibás PIN-kód!
Tranzakció elutasítva: Nincs elegendő fedezet! (Egyenleg: 50000.0 Ft)
Sikeres kifizetés (15000.0 Ft). Új egyenleg: 35000.0 Ft
```

---

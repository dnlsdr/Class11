### Raktári Cikkszám (SKU - stock keeping unit) Generátor
* Hozz létre egy package-t, továbbiakban abban dolgozz!
* Hozz létre egy ProductItem osztályt következő mezőkkel: public String category, public String name, public int id, private String sku.
* Írj a ProductItem osztályba egy public void generateSku() példánymetódust, 
amely összeállítja a termék cikkszámát a következő szabály szerint:
  - A category első 3 betűje csupa nagybetűvel
  - egy kötőjel ,
  - a name első 2 betűje csupa nagybetűvel,
  - egy kötőjel,
  - végül az id értéke,
  - pl. "ELE-LA-101" egy "Electronics" kategóriájú "Laptop" esetén, aminek az ID-ja 101.
* A kész szöveget mentsd el a private String sku mezőbe!
* A Main osztályban hozz létre egy 3 elemű ProductItem[] tömböt, töltsd fel termékekkel, és egy for ciklussal hívd meg mindegyiken a generateSku() metódust!
* Írj a Main osztályba egy public static void printItemsByFirstLetter(ProductItem[] items, char letter) metódust, 
amely végigmegy a tömbön, lekéri az egyes termékek cikkszámát és ellenőrzi, hogy a cikkszám a megadott karakterrel kezdődik-e! 
Ha igen, írja ki a teljes cikkszámot!
* Érdemes használni: StringBuilder, substring, charAt
* Várható kimenet:
```
--- Cikkszámok Generálása ---
Laptop generált SKU: ELE-LA-101
Smartphone generált SKU: ELE-SM-102
Table generált SKU: FUR-TA-201

--- Szűrés 'E' betűre ---
Találat: ELE-LA-101 (Termék: Laptop)
Találat: ELE-SM-102 (Termék: Smartphone)
```

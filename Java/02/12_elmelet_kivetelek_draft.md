**1. Elméleti bevezető**


A kivétel egy olyan objektum, amely a program futása során fellépő, az elvárttól eltérő (abnormális) állapotot ír le. A kivételkezelés célja, hogy a normál működés kódját elválassza a hibakezeléstől, ezáltal növelve a program olvashatóságát és megbízhatóságát. Ha hiba történik, a program "dob" egy kivételt, amelyet a megfelelő helyen "elkapunk", és megpróbáljuk a rendszert újra konzisztens állapotba hozni.

**2.1. Ellenőrzött és nem ellenőrzött kivételek**

* **Throwable**: A kivételosztályok hierarchiájának csúcsa, minden kivétel őse.
* **Error**: Komoly, általában futást gátló és előre nem látható problémát jelez (pl. `OutOfMemoryError`), nem kötelező kezelni.
* **RuntimeException** (Nem ellenőrzött / Unchecked): Futási időben bekövetkező, programozói hibából eredő kivétel (pl. `NullPointerException`), amit a fordító nem kényszerít ki, hogy lekezeljünk.
* **Exception** (Ellenőrzött / Checked): A `RuntimeException`-ön kívül minden ide tartozó kivételt (pl. `IOException`) kötelező a kódban lekezelni vagy a továbbdobását specifikálni.



**2.2. Java kivétel objektumok**

Minden kiváltott kivétel objektum információt hordoz magáról a hibáról és a kiváltáskori állapotról. Ezek az információk a beépített metódusokkal kérdezhetők le, mint például a `getMessage()`, `getLocalizedMessage()`, vagy a hívási láncot visszaadó `getStackTrace()`.

**2.3. Kivételek keletkezése**

A kivételek háromféle módon jöhetnek létre:

1. Futás közben bekövetkező rendellenes helyzet során (pl. nullával osztás, memória elfogyása).
2. Szándékosan, a kódba írt `throw` utasítás hatására.
3. Aszinkron módon (például egy másik végrehajtási szál megszakadásakor).



**2.4. Kivételek elkapása**

A kivételeket a `try-catch` blokk segítségével kaphatjuk el. A `try` blokkban lévő kódrészletet a rendszer figyeli, és ha kivétel dobódik, a futás ott megszakad. A program a `try` utáni `catch` ágakon keresi a kivétel típusának megfelelő blokkot, és ha megtalálja, végrehajtja az ahhoz tartozó hibakezelő utasításokat.

Az alábbi gyakorlati példa egy átmeneti fájl létrehozását és írását mutatja be, ahol a fájlrendszer-műveleteket egy `try` blokk védi, az esetleges problémát pedig egy `catch` blokk kapja el `IOException` formájában:

```java
try {
    // Temp fájl létrehozása és beállítása törlésre kilépéskor
    File temp = File.createTempFile("filename", ".txt");
    temp.deleteOnExit();
    
    // Írás a temp fájlba
    BufferedWriter out = new BufferedWriter(new FileWriter(temp));
    out.write("...");
    out.close();
} catch (IOException e) {
    /* Hiba lekezelése */
}

```

**2.5. Mindenképpen lefutó programkód**

A `finally` blokk olyan kódrészleteket tartalmaz, amelyek a `try` blokk lefutása után mindenképpen végrehajtódnak – függetlenül attól, hogy történt-e kivétel vagy sem. Jellemzően erőforrások lezárására használjuk. Az alábbi példában a `finally` blokk garantálja, hogy a `BufferedReader` (`br.close()`) mindenképpen lezárásra kerüljön:

```java
public String readFirstLineFromFile(String path) throws IOException {
    BufferedReader br = new BufferedReader(new FileReader(path));
    try {
        return br.readLine();
    } finally {
        if (br != null) br.close();
    }
}

```

**2.6. Ellenőrzött kivételek specifikálása**

Ha egy metóduson belül ellenőrzött kivétel keletkezhet, de azt helyben nem kezeljük le (nincs `catch` ág), akkor a `throws` utasítással a metódus fejében jelezni kell, hogy a metódus ezt a kivételt továbbadja a hívójának:

```java
public static String readFromKeyboard() throws IOException {
    /*...*/
}

```

**2.7. Multicatch ágak**

Ha több különböző típusú hibát pontosan ugyanazzal a logikával (például hibaüzenet kiírásával) szeretnénk lekezelni, egyetlen `catch` blokkban is elkaphatjuk őket. A kódismétlés elkerülése érdekében a típusokat a `|` (vagy) operátorral választjuk el:

```java
try {
    /*...*/
} catch (IOException | SQLException e) {
    System.err.println(e.getMessage());
}

```

**2.8. Try with resources**

Ez a szerkezet automatizálja az erőforrások lezárását, így kiváltja a `finally` blokk használatát a `close()` hívásokhoz. A hagyományos szintaxisban a `try` kulcsszó utáni zárójelben kell példányosítani az erőforrást, a Java 9 óta pedig egy már korábban létrehozott változó (ha később nem kap új értéket) is megadható:

```java
// Hagyományos try-with-resources:
static String readFirstLineFromFile(String path) throws IOException {
    try (BufferedReader br = new BufferedReader(new FileReader(path))) {
        return br.readLine();
    }
}

// Java 9 try-with-resources (több erőforrással):
static String readFirstLineFromFile(String path) throws IOException {
    BufferedReader br0 = new BufferedReader(new FileReader(path0));
    try (br0; 
         BufferedReader br1 = new BufferedReader(new FileReader(path1));
         BufferedReader br2 = new BufferedReader(new FileReader(path2))) {
        
        return br2.readLine().append(br1.readLine());
    }
}

```

**2.9. Saját kivételek**

Gyakran előfordul, hogy az üzleti logika miatt saját, egyedi kivételosztályra van szükségünk, amit az `Exception` osztályból származtatva hozhatunk létre. A kivétel a `throw` kulcsszóval váltható ki:

```java
class MyException extends Exception {
    public MyException(String message) {
        super(message);
    }
}

class ExceptionTest1 {
    public static void main(String[] args) {
        try {
            throw new Exception("Exception");
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }

        try {
            throw new MyException("MyException");
        } catch (MyException e) {
            System.out.println(e.getMessage());
        }
    }
}

```

**3. Kivételek elkapásának sorrendje**

A `catch` ágak kiértékelése szigorúan fentről lefelé halad. Emiatt kulcsfontosságú a sorrend: először mindig a specifikusabb (leszármazott) kivételeket kell elkapni, és csak utána az általánosabbakat (ősosztályokat). Ha az ősosztály megelőzné a leszármazottat, az alsó catch ág sosem kerülhetne sorra. Ha az öröklődési lánc `Exception` -> `MyException` -> `MySpecialException1` / `MySpecialException2`, akkor a helyes sorrend a következő:

```java
try {
    /*...*/
} catch (MySpecialException2 mse2) {
    /*...*/
} catch (MySpecialException1 mse1) {
    /*...*/
} catch (MyException me) {
    /*...*/
} catch (Exception e) {
    /*...*/
}

```

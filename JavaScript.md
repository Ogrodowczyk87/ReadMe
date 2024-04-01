- [Java Script](#java-script)
- [Deklaracja zmiennej](#deklaracja-zmiennej)
- [Kiedy wykorzystywać const i let](#kiedy-wykorzystywać-const-i-let)
- [constants i CONSTANTS (stałe)](#constants-i-constants-stałe)
- [Operator typeof](#operator-typeof)
- [Operatory porównania](#operatory-porównania)
- [Operatory równości](#operatory-równości)
- [Klasa Math](#klasa-math)
- [Metoda indexOf()](#metoda-indexof)
- [Metoda includes()](#metoda-includes)
- [Metoda endsWith()](#metoda-endswith)
- [Metoda replace() i replaceAll()](#metoda-replace-i-replaceall)
- [Metoda slice()](#metoda-slice)


# Java Script


# Deklaracja zmiennej


:bulb: **const**  
Deklaracja zmiennej najczęściej zaczyna się od słowa kluczowego **const**. Taka zmienna musi mieć nadaną wartość, po przypisaniu której nie może już być nadpisana.

:bulb:  **let**  
W celu zadeklarowania zmiennej, której w przyszłości będzie można przypisać nową wartość, używane jest słowo kluczowe **let**.



# Kiedy wykorzystywać const i let

Jedyną różnicą między const i let jest to, że const nie pozwala na ponowne przypisywanie wartości do zmiennej. Deklaracja const sprawia, że kod jest bardziej czytelny, ponieważ zmienna zawsze odnosi się do tej samej wartości. W przypadku let takiej pewności już nie ma.

let i const należy wykorzystywać w ten sposób:

    Używaj const domyślnie, większość zmiennych będzie deklarowana właśnie w taki sposób.
    Używaj let, jeśli będziesz przypisywał nowe wartości do zmiennej podczas wykonywania skryptu.

# constants i CONSTANTS (stałe)

Nazwy CONSTANTS - są zmienne, których znaczenie nie zmienia się nigdy podczas wykonywania całego skryptu i mają znaczenie konfiguracyjne. Nazwy takich zmiennych przyjęło się zapisywać następująco: UPPER_SNAKE_CASE.


```JS
// Zmienna stała, która zawiera wartość koloru
const COLOR_TEAL = "#009688";

// Zmienna stała, która zawiera wiadomość powitalną po zalogowaniu się
const LOGIN_SUCCESS_MESSAGE = "Witamy!";
```

# Operator typeof
Służy do sprawdzenia typu wartości zmiennej. Pokazuje typ wartości zmiennej którą podamy

```JS
let username;
console.log(typeof username); // "undefined"

let inputValue = null;
console.log(typeof inputValue); // "object"

const quantity = 17;
console.log(typeof quantity); // "number"

const message = "JavaScript is awesome!";
console.log(typeof message); // "string"

const isSidebarOpen = false;
console.log(typeof isSidebarOpen); // "boolean"

```

# Operatory porównania

Wykorzystuje się do porównania dwóch wartości. Rezultatem wykonania będzie zwrócony boolean - true lub false, czyli «tak» lub «nie».

- a > b i a < b - większe / mniejsze
- a >= b i a <= b - większe lub równe / mniejsze lub równe
- a == b - równa się
- a != b - nie równa się
- a === b - ściśle równa się
- a !== b - ściśle nie równa się

```JS
const x = 5;
const y = 10;
const z = 5;

console.log("x > y:", x > y);// false
console.log("x < y:", x < y);// true
console.log("x < z:", x < z);// false
console.log("x <= z:", x <= z);// true
console.log("x === y:", x === y);// false
console.log("x === z:", x === z);// true
console.log("x !== y:", x !== y);// true
console.log("x !== z:", x !== z);// false

```

# Operatory równości

Dlatego, aby sprawdzić równość lub nierówność dwóch wartości, wykorzystują się tylko operatory === (ścisła równość) i ! == (ścisła nierówność), które nie wykonują transformacji typów operandów.

```JS
// ✅ Dobrze
console.log(5 === "5");// false
console.log(5 === 5);// true
console.log(5 !== "5");// true
console.log(5 !== 5);// false
console.log(1 === true);// false
console.log(1 !== true);// true
```

# Klasa Math

Jedna z wbudowanych klas udostępniająca zestaw metod do pracy z liczbami. Znajomość wszystkich metod na pamięć nie jest wymagana, ale należy znać te najbardziej przydatne.

```JS
// Math.floor(num) - zwraca największą całkowitą liczbę,
// mniejszą, lub równą podanej liczbie, czyli zaokrągla w dół.
console.log(Math.floor(1.7)); // 1

// Math.ceil(num) - zwraca najmniejszą całkowitą liczbę,
// większą, lub równą podanej liczbie, czyli zaokrągla w górę.
console.log(Math.ceil(1.2)); // 2

// Math.round(num) - zwraca wartość liczby,
// zaokrągloną do najbliższej liczby całkowitej
// wedle zasad znanych ze szkoły
console.log(Math.round(1.2)); // 1
console.log(Math.round(1.5)); // 2

// Math.max(num1, num2, ...) - zwraca największą liczbę z podanych
console.log(Math.max(20, 10, 50, 40)); // 50

// Math.min(num1, num2, ...) - zwraca najmniejszą liczbę z podanych
console.log(Math.min(20, 10, 50, 40)); // 10

// Math.pow(base, exponent) - operacja potęgowania
console.log(Math.pow(2, 4)); // 2^4 === 16

// Math.random() - zwraca zmiennoprzecinkową liczbę pseudolosową 
// z zakresu [0, 1)
console.log(Math.random()); // pseudolosowa losowa liczba pomiędzy 0 i 1
console.log(Math.random() * (10 - 1) + 1); // pseudolosowa liczba od 1 do 10
```

# Metoda indexOf()

Zwraca pozycję (indeks), na której zaczyna się pierwsze dopasowanie podanego stringa lub -1 jeśli nigdzie takiego nie mamy. Wielkość liter jest tutaj istotna

```JS
const message = "Welcome to Bahamas!";
console.log(message.indexOf("to"));// 8
console.log(message.indexOf("To"));// -1
console.log(message.indexOf("hello"));// -1
```


# Metoda includes()

Sprawdza, czy dany string jest zawarty w wierszu, (jest jego tak zwanym substringiem) zwraca boolean - true jeśli tak i false w przeciwnym wypadku. Dla tej metody również ważna jest wielkość liter, ponieważ litera "a" nie równa się literze "А".


```JS
const productName = "Repair droid";

console.log(productName.includes("p"));// true
console.log(productName.includes("P"));// false
console.log(productName.includes("droid"));// true
console.log(productName.includes("Droid"));// false
console.log(productName.includes("Repair"));// true
console.log(productName.includes("repair"));// false
```


 🙃 CIEKAWOSTKA 

    Wszystkie metody wierszy uwzględniają wielkość litery. Mówimy o nich, że są “case-sensitive”

# Metoda endsWith()

Pozwala określić, czy wiersz kończy się określonym stringiem podanym w nawiasach, zwracając true lub false.

```JS
const jsFileName = "script.js";
console.log(jsFileName.endsWith(".js"));// true

const cssFileName = "styles.css";
console.log(cssFileName.endsWith(".js"));// false
```

# Metoda replace() i replaceAll()

Zwraca nowy wiersz, w którym pierwsze (replace) lub wszystkie dopasowania (replaceAll) podanego stringa są zastępowane określoną wartością (w nawiasach po przecinku).

```JS
const jsFileName = "script.js";
const minifiedJsFileName = jsFileName.replace(".js", ".min.js");
console.log(minifiedJsFileName);// "script.min.js"

const cssFileNames = "styles.css, about.css, portfolio.css";
const minifiedCssFileNames = cssFileNames.replaceAll(".css", ".min.css");
console.log(minifiedCssFileNames);// "styles.min.css, about.min.css, portfolio.min.css"
```


# Metoda slice()

Metodę string slice(startIndex, endIndex) wykorzystuje się do utworzenia kopii części lub całego łańcucha. Tworzy kopię elementów wiersza od znaku o indeksie startIndex i do, ale nie włączając w to znaku o indeksie podanym jako endIndex i zwraca nowy string.

```JS
const productName = "Repair droid";
console.log(productName.slice(0, 4)); // "Repa"
console.log(productName.slice(3, 9)); // "air dr"
console.log(productName.slice(0, productName.length)); // "Repair droid"
console.log(productName.slice(7, productName.length)); // "droid"
```
- [System modułowy Node.js](#system-modułowy-nodejs)
- [Globalne zmienne](#globalne-zmienne)
- [Moduły](#moduły)
- [Moduły ComonJS](#moduły-comonjs)
- [Moduły ECMAScript](#moduły-ecmascript)
- [Praca z plikami](#praca-z-plikami)
- [Tworzenie aplikacji konsolowych](#tworzenie-aplikacji-konsolowych)
- [Wprowadzenie do Express](#wprowadzenie-do-express)
- [Oprogramowanie pośredniczące](#oprogramowanie-pośredniczące)
- [Przekazanie danych na serwer](#przekazanie-danych-na-serwer)
  - [Przekazanie parametru do URL](#przekazanie-parametru-do-url)
- [Wykorzystanie parametrów zapytania `GET`](#wykorzystanie-parametrów-zapytania-get)
- [Wysyłanie danych przy pomocy formularza](#wysyłanie-danych-przy-pomocy-formularza)
- [Przekazanie JSON](#przekazanie-json)
- [Routing w aplikacji](#routing-w-aplikacji)
  - [Metody Route](#metody-route)
- [Metody odpowiedzi](#metody-odpowiedzi)
- [Łańcuchy metod](#łańcuchy-metod)
- [REST API](#rest-api)
- [Przyklad aplikacji](#przyklad-aplikacji)
- [Zmienne środowiskowe](#zmienne-środowiskowe)
- [Logowanie](#logowanie)
- [Czym jest REST](#czym-jest-rest)
  - [Wprowadzenie](#wprowadzenie)
- [Czym jest URI i URL?](#czym-jest-uri-i-url)
- [Architektoniczne ograniczenia rozwiązań RESTful](#architektoniczne-ograniczenia-rozwiązań-restful)
  - [Klient-serwer](#klient-serwer)
  - [Stateless](#stateless)
  - [Jeden interfejs](#jeden-interfejs)
  - [Pierwsze ograniczenie interfejsu: identyfikacja źródeł](#pierwsze-ograniczenie-interfejsu-identyfikacja-źródeł)
  - [Drugie ograniczenie interfejsu: manipulowanie zasobami przed widoki](#drugie-ograniczenie-interfejsu-manipulowanie-zasobami-przed-widoki)
  - [Trzecie ograniczenie interfejsu: samoopisujące się (self-descriptive) wiadomości](#trzecie-ograniczenie-interfejsu-samoopisujące-się-self-descriptive-wiadomości)
  - [Czwarte ograniczenie interfejsu: hipermedia](#czwarte-ograniczenie-interfejsu-hipermedia)
- [Buforowanie](#buforowanie)
- [Wielowarstwowy (wielopoziomowy) system](#wielowarstwowy-wielopoziomowy-system)
  - [Kod zapytania](#kod-zapytania)
- [Podsumowanie](#podsumowanie)


# System modułowy Node.js
# Globalne zmienne
 Aby zmienna w Node.js była dostępna globalnie, trzeba zadeklarować ją jako właściwość obiektu Global.

 ```js
 global.foo = 3;
```
Obiekt Global to odpowiednik obiektu window z przeglądarki. Metoda require, służąca do połączenia modułów nie jest globalna i będzie lokalna dla każdego modułu.

Również lokalnymi dla każdego modułu są:

- module.export – obiekt odpowiadający za to, co dokładnie będzie eksportować moduł gdy zażądamy go przy wykorzystaniu require;
- __filename – nazwa pliku wykonywanego skryptu;
- __dirname – absolutna ścieżka do wykonywanego skryptu.

Wrócimy do nich trochę później i przeanalizujemy je bardziej szczegółowo, gdy przyjrzymy się podłączaniu modułów w Node.js.

W sekcji Global znajdują się między innymi takie klasy, jak:

- Buffer – klasa wykorzystywana do operacji z danymi binarnymi.
- process – obiekt w którym znajdują się informacje o danym procesie Node.js

Na przykład właściwość process.argv będzie zawierać tablicę argumentów polecenia podanych przy uruchamianiu skryptu w Node.js. Zerowym elementem będzie ścieżka absolutna dla Node.js, drugim ścieżka pliku który został uruchomiony a następnie dostępne będą podane parametry.

Do pracy z katalogami wykorzystuje się następujące funkcje: 

- process.cwd() zwraca ścieżkę obecnego katalogu roboczego,
- process.chdir() wykonuje przejście do innego katalogu. Polecenie
- process.exit() kończy proces ze wskazanym jako argument kodem: 0 - poprawne zakończenie działania, inne statusy np. 1 – oznaczają błąd.

Ważna metoda process.nextTick(fn) zaplanuje wykonanie podanej jako callback funkcji w taki sposób, że zostanie wykonana przed zakończeniem bieżącego “obrotu” Event Loop, (nazwa funkcji może być tutaj nieco myląca, aby przesunąć wykonanie funkcji na następny tick możemy użyć setImmediate)


```js
// jest uzywana na koniec event loopa
setImmediate(function () {
  console.log("setImmediate callback");
});

// uzywa sie zeby jeszcze przed wszystkim na poczatku
// wcisnac jaks funkcje
process.nextTick(function () {
  console.log("NextTick callback");
});

// NextTick callback
// setImmediate callback
```

Obiekt process zawiera jeszcze wiele właściwości i metod, z którymi można zapoznać się samodzielnie w dokumentacji do Node.js.

# Moduły

Do podłączenia dodatkowych modułów do twojego projektu w Node.js można zastosować wygodny system zarządzania modułami NPM. Najprościej rzecz ujmując: z jednej strony jest to publiczne repozytorium dla Node.js zawierające stworzone przez społeczność moduły czy biblioteki. Z drugiej strony jest to też narzędzie który instalujemy razem z Node.js i dostępne przez:

Polecenie npm które pozwala tworzyć, usuwać lub aktualizować potrzebne ci moduły, automatycznie uwzględniając przy tym wszystkie zależności wybranego przez ciebie modułu od innych modułów uzupełniających. Instalacja modułu nastąpi po wykonaniu polecenia:

```js
npm install nazwa-paczki@wersja-paczki flagi
```

Biorąc pod uwagę to, że wszystkie publiczne moduły NPM można łatwo zainstalować przy pomocy npm, dla twojego projektu należy stworzyć plik package.json z listą wszystkich niezbędnych do pracy zależności i później zainstalować na serwerze wszystkie potrzebne moduły dzięki poleceniu:

```
npm install
```

Podstawowe flagi przy instalacji to:

- -S lub --save - moduł instaluje się jako podstawowa zależność. Oznacza to, że moduł jest niezbędny do normalnego funkcjonowania programu niezależnie do tego gdzie jest uruchomiony. W pliku package.json znajdziemy go potem pod kluczem dependancies
- -D lub --save-dev - oznacza, że moduł zainstaluje się w devDependencies - czyli jest potrzebny tylko podczas developmentu, a nie w środowisku produkcyjnym.
- -g czyli global - pozwala zainstalować moduł do wykorzystania w dowolnym projekcie na danej maszynie.

Moduły należą do jednej z trzech kategorii:

- bazowe (core modules)
- plikowe (file modules)
- moduły npm (npm modules)

Nazwy bazowych modułów są zarezerwowane i nie powinny być nadpisywane; moduły takie jak fs i os, dostarcza nam środowisko Node.

File Module – gdy tworzysz plik w którym przypisane zostało coś (funkcja, obiekt, itd.) do właściwości module.exports, a później plik ten jest wykorzystywany w innych plikach twojego programu to mówimy o module plikowym.

Moduły npm – to w zasadzie moduły plikowe (często rozbite na wiele plików), które znajdują się w specjalnym folderze o nazwie node_modules.

Gdy wykorzystujesz funkcję require, Node określa typ modułu na podstawie podanej ścieżki/nazwy.

Jeżeli wskażesz moduł który nie jest jednym z core modules, to Node.js będzie szukał w bieżącym katalogu podkatalogu node_modules.

Jeżeli to nie zakończy się sukcesem, to Node.js przejdzie do katalogu-rodzica i znów zacznie szukać katalogu node_modules, i następnie szukał tam modułu.

Proces będzie się powtarzał, dopóki moduł nie zostanie znaleziony lub nie zostanie osiągnięty katalog root.

 # Moduły ComonJS

 Node.js pracuje z systemem podłączania modułów CommonJS. Każdy moduł CommonJS reprezentuje gotowy do wykorzystania fragment kodu JavaScript, który eksportuje obiekty specjalne który możemy używać w dowolnej ilości miejsc. Dwa główne narzędzia modułów CommonJS to:

- obiekt module.exports zawierający to, co moduł chce uczynić dostępnym dla innych części systemu
- funkcja require, która jest wykorzystywana przez jedne moduły do importu obiektu exports z innych.

Stwórzmy plik/moduł module.js

```js
const info = msg => {
  console.log(`Info: ${msg}`);
};

const log = msg => {
  console.log(`Log: ${msg}`);
};

module.exports = {
  info,
  log,
};
```

Konstrukcja module – specjalny obiekt, który jest dostępny w Node.js w celu realizacji modułów Common.js. Wszystko to, co będzie przypisane jego właściwości exports, będzie eksportować się z tego modułu.

W innym pliku, main.js, podłączymy ten moduł i wywołajmy eksportowane funkcje:

```js
const logger = require('./module');

logger.info('info function');
logger.log('log function');
```

# Moduły ECMAScript

Zaczynając od wersji: 6.х Node.js wspiera również podłączenie modułów zgodnie ze standardem ECMAScript. Jednak pełne importowanie działa dopiero z wersją 14.x.

Aby wykorzystać moduły ECMAScript musimy skorzystać z jednego z dwóch sposobów. Pierwszy – dodać do plików rozszerzenie .mjs, abyśmy mogli wykorzystać moduły ECMAScript lub drugi – w pliku package.json utworzyć pole "type" z wartością "module". Wykorzystamy drugi sposób i najpierw utworzymy w pustym folderze plik package.json przy pomocy polecenia:

```
npm init -y
```
i dodamy w nim pole "type" z wartością "module".

Później utworzymy moduł import.js.

```js
export const info = msg => {
  console.log(`Info: ${msg}`);
};

export const log = msg => {
  console.log(`Log: ${msg}`);
};
```

W pliku app.js importujemy wskazany moduł i wywołujemy funkcję:
```js
import { info, log } from './import.js';

info('info function');
log('log function');
```
W ten sposób zrealizowaliśmy drugie podejście podłączenia modułów czyli ECMAScript.

# Praca z plikami
Moduł FileSystem (fs) pozwala na pracę z plikami w Node.js. Obecnie najczęściej korzystamy z jego wersji promise, dzięki czemu nie musimy korzystać ze składni callback-ów.
```js
const fs = require('fs').promises;
```

Najczęściej wykorzystywane funkcje do podstawowych operacji na plikach to:

- fs.readFile(filename, [options]) - czytanie pliku;
- fs.writeFile(filename, data, [options]) - zapis pliku;
- fs.appendFile(filename, data, [options])dodanie do pliku;
- fs.rename(oldPath, newPath) - zmiana nazwy pliku;
- fs.unlink(path, callback) - usunięcie pliku.

Przy operacjach z plikami nigdy nie powinniśmy zapominać o obsłudze błędów.

Należy również pamiętać, że parametr data w funkcji readFile, zawiera obiekt będący instancją klasy Buffer, zawierający ciąg przeczytanych bajtów, to znaczy surowe dane.

W przypadku prostych plików tekstowych możemy przekonwertować data metodą toString():

```js
fs.readFile('readme.txt')
  .then(data => console.log(data.toString()))
  .catch(err => console.log(err.message));
```

Istnieją również metody modułu fs z synchronicznymi wersjami, kończące się na Sync, jak również i wersje przyjmujące callback.

Możemy je importować poprzez:

```js
const fs = require('fs');
```

Wtedy wszystkie funkcje nie zwracają promise więc nie możemy wykorzystać składni .then. Funkcjom synchronicznym nie jest również potrzebny callback, ponieważ są one blokujące i dlatego też nie są rekomendowane, chyba że wymaga tego obecne zadanie i dobrze rozumiesz, w jakim celu je stosujesz. Zobaczmy przykłady:

**readFileSync**

```js
try {
  const data = fs.readFileSync("readme.txt");
  console.log(data.toString());
} catch (err) {
  console.log(err.message);
}
```
**readFile** (wersja bez promise)

```js
const fs = require("fs");

fs.readFile("readme.txt", function (err, data) {
  if (err) {
    console.log("error", err.message);
  } else {
    console.log(data.toString());
  }
});
```

Napiszmy skrypt files.js, który będzie odczytał obecny katalog i wyprowadzał do konsoli jego zawartość: nazwę pliku, jego rozmiar i datę wprowadzenia ostatniej zmiany w pliku.

```js
const fs = require('fs').promises;

fs.readdir(__dirname)
  .then(files => {
    return Promise.all(
      files.map(async filename => {
        const stats = await fs.stat(filename);
        return {
          Name: filename,
          Size: stats.size,
          Date: stats.mtime,
        };
      }),
    );
  })
  .then(result => console.table(result));
```
Przeanalizujmy ten kod bardziej szczegółowo. Na początku podłączamy standardowy moduł fs w wersji z promisami:

```js
const fs = require('fs').promises;
```

Przy pomocy ścieżki ze zmiennej __dirname odczytujemy wszystkie pliki z obecnego katalogu.

W rezultacie działania funkcji readdir, w zmiennej files otrzymujemy promise który zwróci tablicę nazw plików i katalogów z obecnego katalogu.

Zwracamy więc tablicę promisów gdzie każdy analizuje kolejny element files

Zmienna stats zawierać będzie szczegółowe informacje o każdym kolejnym pliku i katalogu.

Zwracamy obiekt z nazwą pliku oraz 

- stats.mtime - czas ostatniej zmiany pliku
- stats.size - który określa rozmiar pliku w bajtach.

Rezultat wykonania tego promise, czyli zmienną result, przekazujemy funkcji console.table i dzięki niej wyświetlimy tabelkę z informacjami przy wykonaniu naszego skryptu w konsoli.

**Wynik wykonania skryptu:**

![Node](./Images//01-file-1941ca9bbb2c43099a45689058a497bc.png)
Rys. 1 - wynik wykonania skryptu

# Tworzenie aplikacji konsolowych

Aplikacje konsolowe (CLI applications) - to takie który uruchamiamy poprzez terminal, tradycyjnie precyzujemy działanie takich poleceń poprzez przekazywanie parametrów przy ich wywoływaniu. Skrót CLI (command-line interface) tłumaczy się jako 'wiersz poleceń', ale dla niektórych systemów operacyjnych lub programów mówimy też właśnie o terminalu (np. dla VSC lub MacOS).

W poprzednim module zauważyliśmy, że przekazane parametry przy uruchomieniu skryptu są dostępne w tablicy process.args w postaci: ```["/path/to/node", "/path/to/yourScript.js", "param 1", "param 2", …].```

Aby otrzymać same parametry należy więc wykonać polecenie ```process.argv.slice(2)```, który zwróci tablicę ze wszystkimi rozdzielonymi spacjami wartościami: ```["param 1", "param 2"```, …].

Opracowywanie wszelkiego rodzaju kombinacji parametrów i ich formatów jest bardzo niewygodne. Z tego powodu zazwyczaj wykorzystuje się dodatkowe moduły npm. Jeden z najpopularniejszych, którego będziemy używać, to moduł ```commander```.

Dodatkowo możemy wykorzystać wprowadzanie danych w konsoli przez użytkownika zgodnie poprzez schemat pytanie-odpowiedź, będziemy do tego wykorzystywać moduł Node.js ```readline```. Inicjalizacja jest dość prosta:

```js
const readline = require('readline');
const rl = readline.createInterface({
  input: process.stdin,// wprowadzenie ze standardowego strumienia
  output: process.stdout,// wyprowadzenie do standardowego strumienia
});
```

Podpinamy moduł ```readline``` i tworzymy instancję ```rl```, dla której w opcjach przekazujemy strumienie wejścia i wyjścia, do wyboru may konsolę, plik i tak dalej. W naszym przypadku bierzemy standardowe strumienie i będziemy pracować używając konsoli, gdzie uruchamiamy skrypt. Jeśli chcemy zareagować na każdą wprowadzoną przez użytkownika wartość (potwierdzoną klawiszem ``enter``) użyjemy zdarzenia `line`:

```js
rl.on('line', cmd => {
  console.log(`You just typed: ${cmd}`);
});
```
Nas jednak bardziej interesuje możliwość zadania użytkownikowi pytania i otrzymania na nie odpowiedzi, analogicznie do funkcji `prompt` z przeglądarki:

```js
rl.question('Jak się nazywasz? ', answer => {
  console.log(`Miło cię poznać. ${answer}`);
});
```
W trakcie trwania dłuższej operacji możemy ustawić w rozmowie pauzę czyli innymi słowami zablokować wprowadzenie kolejnych danych:

```js
rl.pause();
```

Aby zamknąć wprowadzanie danych, należy wywołać metodę:

```js
rl.close();
```

Napiszmy teraz prostą aplikację – "Odgadnij liczbę", gdzie należy zgadywać, jaką liczbę od 1 do 10 wylosował program, a ten na końcu pokaże, za którym razem nam się to udało i zapisze udaną próbę do pliku podanego jako parametr.

Przydadzą się nam standardowe moduły `fs`, `readline` oraz niestandardowe, które należy zainstalować przy pomocy `npm`, moduł `[commander](https://www.npmjs.com/package/commander)` i `[colors](https://www.npmjs.com/package/colors)`. Poniżej znajdziecie kod programu i jego analizę.

```js
const readline = require("readline");
const fs = require("fs").promises;
const { program } = require("commander");
require("colors");
program.option(
  "-f, --file [type]",
  "file for saving game results",
  "results.txt"
);
program.parse(process.argv);

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

let count = 0;
const logFile = program.opts().file;
const mind = Math.floor(Math.random() * 10) + 1;

const isValid = (value) => {
  if (isNaN(value)) {
    console.log("Wprowadź liczbę!".red);
    return false;
  }
  if (value < 1 || value > 10) {
    console.log("Liczba powinna znajdować się w przedziale od 1 do 10".red);
    return false;
  }
  return true;
};

const log = async (data) => {
  try {
    await fs.appendFile(logFile, `${data}\n`);
    console.log(`Udało się zapisać rezultat w pliku ${logFile}`.green);
  } catch (err) {
    console.log(`Nie udało się zapisać pliku ${logFile}`.red);
  }
};

const game = () => {
  rl.question(
    "Wprowadź liczbę od 1 do 10, aby zgadywać: ".yellow,
    async (value) => {
      value = Number.parseInt(value, 10);
      if (!isValid(value)) {
        game();
        return;
      }
      count += 1;
      if (value === mind) {
        console.log("Gratulacje. Odgadłeś liczbę za %d razem".green, count);
        await log(
          `${new Date().toLocaleDateString()}: Gratulacje. Odgadłeś liczbę za ${count} razem`
        );
        rl.close();
        return;
      }
      console.log("Nie zgadłeś. Kolejna próba.".red);
      game();
    }
  );
};

game();
```
Cały program składa się z trzech funkcji. Podstawowa z nich to funkcja gry game(), która wywołuje się rekurencyjnie do momentu, aż nie zgadniemy podanej liczby. Na początku podłączamy moduł colors, który pozwala nam zmieniać kolor tekstu w konsoli. Dalej podłączamy moduł commander, który pozwoli nam obsłużyć parametry podane przy uruchomieniu programu

```js
const { program } = require('commander');
program.option(
  '-f, --file [type]',
  'file for saving game results',
  'results.txt',
);
program.parse(process.argv);
```
Wskazujemy, że opcjonalnie czekamy na wprowadzenie parametrów z flagą `-f` lub jej dłuższym zapisem `--file`. Innymi słowami określamy, że uruchomienie programu powinno nastąpić w następującej postaci:

```
node game.js -f my_log.txt
````

Kiedy w ten sposób uruchomimy nasz program, przekażemy, że należy w zmiennej `program.file` umieścić wartość `my_log.txt`.

W kodzie wskazujemy przez trzeci parametr `program.option`, że jeżeli parametr `-f` nie będzie przekazany przy uruchomieniu, to domyślnie program.file będzie miał wartość `results.txt`.

Drugi parametr to podpowiedź która pokaże się jeśli spróbujemy uruchomić nasz program z flagą `-h` czyli `help`

```
node index.js -h
````

![console](./Images/console-app-1pl-49cade24fc81fb01528d2f05836bc643.png)

Dalej w naszym programie wykonujemy inicjalizację modułu `readline`.

Tworzymy również trzy zmienne, które będziemy dalej wykorzystywać:

- `count` – to licznik ilości prób, które podjął użytkownik, aby odgadnąć liczbę,
- `logFile` - nazwa pliku, w którym będą zapisane rezultaty gry,
- `mind` - to wylosowana liczba od 1 do 10, którą należy odgadnąć.

Funkcja `isValid` odpowiada za walidację wprowadzonych wartości w konsoli. Sprawdza, czy wprowadzona wartość jest liczbą i znajduje się w przedziale od 1 do 10. Jeżeli dane są poprawne, to funkcja zwraca prawdę, jeżeli nie – fałsz.

Funkcja `log` odpowiada za zapisanie wyników gry. Wykorzystuje ona funkcję `appendFile` modułu `fs` do zapisu danych. Jeżeli plik istnieje, to rezultaty będą dopisane w istniejącym pliku, jeżeli pliku nie ma – zostanie on utworzony. Zwróć uwagę, że funkcja jest asynchroniczna i czekamy w niej na wykonanie operacji zapisania wyników.

Następnie docieramy do podstawowej funkcji `game`. Wewnątrz zachodzi wywołanie metody:

```js
rl.question(
    'Wprowadź liczbę od 1 do 10, aby zgadnąć wybraną: '.yellow,
    (value) => {...});
```

która podsłuchuje konsolę i przy wprowadzeniu wartości wywołuje funkcję `callback`, która opracowuje wprowadzoną wartość.

Jeżeli wartość po sparsowaniu nie przechodzi walidacji, to włączamy funkcję gry ponownie:

```js
value = Number.parseInt(value, 10);
if (!isValid(value)) {
  game();
  return;
}
```

- Jeśli walidacja przebiegła pomyślnie, to zwiększamy licznik prób do 1.
- Następnie porównujemy wprowadzoną wartość z "założoną" i jeśli tak to:
    - Wyprowadzamy gratulacje i ilość prób w grze,
    - Następnie przy pomocy funkcji log zapisujemy wynik w pliku,
    - Po próbie zapisania pliku zamykamy interfejs do wprowadzania przez rl.close(),
    - Nasz program kończy się
- Jeżeli jednak odpowiedź nie pokrywa się, wykonujemy ponownie funkcję game() do momentu w którym liczba zostanie odgadnięta.
![console2](./Images/console-app-2pl-c3cd1b4143c8989bcbd8269ea288a445.png)

>:fire:usuwasz z palca node_modules paczki i tak są zapisane w dependencies, więc npm install zainstaluje node_modules gdy ich nie ma

# Wprowadzenie do Express

Express - to minimalistyczny i elastyczny web framework dla aplikacji Node.js, dostarczający obszerny zestaw narzędzi potrzebnych dla typowej aplikacji backend. Pozwala wygodnie tworzyć między innymi API oparte o protokół HTTP i metodologię REST.

Przystąpmy od razy do praktyki. Utwórz katalog dla swojej aplikacji i przejdź do niego

```
$ mkdir myapp
$ cd myapp
```
Przy pomocy polecenia:
```
$ npm init -y
```
utwórz plik `package.json` dla swojej aplikacji. Teraz zainstaluj paczkę Express w katalogu myapp i zapisz go na liście zależności poprzez polecenie:
```
$ npm install express
```
UWAGA: w poniższych przykładach korzystamy z wersji express `4.18.2` - możesz sprawdzić ją w pliku `package.json`

W katalogu myapp utwórz plik o nazwie `app.js` i dodaj w niej następujący kod:
```js
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(3000, () => {
  console.log('Example app listening on port 3000!');
});
```
Aplikacja uruchamia serwer i nasłuchuje połączeń na porcie 3000. Uruchom ją korzystając z polecenia:
```
$ node app.js
```
Aplikacja daje w przeglądarce odpowiedź` “Hello World!”` dla zapytania GET adresowanego do root URL (/) (w przypadku uruchomienia aplikacji lokalnie, pełną ścieżką będzie więc `[localhost:3000/](http://localhost:3000`) lub `127.0.0.1:3000/`.

O ścieżce i funkcji która zostanie wywołana dla niej będziemy mówić w skrócie jako o `route`

URL-e które udostępnia dany serwer w swoim API, nazywamy też często `endpoint`-ami, są to właśnie swojego rodzaju “punkty styku” pomiędzy frontendem a backendem.

Dla wszystkich pozostałych ścieżek URL odpowiedzią będzie standardowy status `404 - Not Found`.

Dla opracowywania zapytań, w `routes` w Express dodano szereg wbudowanych funkcji. `Routing` bądź trasowanie określa, jak twoja aplikacja odpowiada na zapytanie klienta dla konkretnego adresu – URL. Każdy route może mieć jedną lub więcej funkcji, które wykonują się przy odpytaniu danej ścieżki. Określenie route w Express ma następującą strukturę:

`app.METHOD(PATH, HANDLER)`

Gdzie:

**app** to instancja aplikacji Express.

**METHOD** -— metoda zapytania HTTP (GET, POST, PUT, PATCH, DELETE).

**PATH** —- ścieżka na serwerze, w naszym przypadku mamy tylko jedną i jest nią root strony '/'.

**HANDLER** —- funkcja, wykonywana po odpytaniu naszego endpointa.

Przyjrzyjmy się krótko temu, do czego wykorzystuje się każdą z metod HTTP:

- GET żąda danych o zasobie. Zapytania z wykorzystaniem tej metody mają tylko zwracać dane.
- POST wykorzystuje się aby utworzyć nowy zasób przy użyciu danych wysłanych w zapytaniu
- PUT służy do utworzenia lub **(częściej) modyfikacji całościowej danego zasobu
- DELETE usuwa wskazany zasób.
    PATCH wykorzystuje się do częściowej aktualizacji zasobu. Wrócimy jeszcze do czasowników, gdy będziemy analizować tworzenie pełnego REST API. 

W naszym przypadku funkcja HANDLER przyjmuje również dwa parametry, obiekt zapytania req i obiekt odpowiedzi `res` (istnieje trzeci parametr `next` do którego wrócimy w dalszej części kursu).

```js
(req, res) => {
  res.send('Hello World!');
};
```
Do uruchomienia serwera wywołuje się metodę `app.listen()`, w której przekazuje się numer portu. Aplikacja zwraca odpowiedź "Hello World!" na zapytania adresowane do root URL (/) lub trasy. Dla wszystkich pozostałych ścieżek (endpointów) które nie są zdefiniowane w naszej aplikacji, na przykład `http://localhost:3000/contact`, odpowiedzią będzie:

```js
Cannot GET /contact
--- 404 Not Found ---
```
Dodamy program obsłużenia route `/contact`:

```js
app.get('/contact', (req, res) => {
  res.send('<h1>Contact page</h1>');
});
```
I teraz URL http://localhost:3000/contact będzie zwracał nam dokument z nagłówkiem Contact page.

::: tip ❗ NOTE

Nie zapomnij zapisać pliku serwera i ponownie go uruchomić, aby zobaczyć zmiany w kodzie.

:::

Symbol ? w ścieżce definiuje, że poprzedni znak może wystąpić 1 raz lub być całkiem nieobecny. Zdefiniowana poniżej ścieżka obsługuje zarówno `/cotact` jak i `/contact`

```js
app.get('/con?tact', (req, res) => {
  res.send('<h1>Contact page</h1>');
});
```
Symbol + definiuje, że poprzedzający go znak może wystąpić jeden raz lub więcej. Ta ścieżka obsługuje: `/contact`, `/conntact`, `/connntact` i tak dalej.
```js
app.get('/con+tact', (req, res) => {
  res.send('<h1>Contact page</h1>');
});
```
Symbol * definiuje, że na miejscu tego symbolu może znajdować się dowolna ilość innych znaków. Ta ścieżka obsługuje `/contact`, `/conxtact`, /`con123tact` i tak dalej.
```js
app.get('/con*tact', (req, res) => {
  res.send('<h1>Contact page</h1>');
});
```

Warto zauważyć, że choć jest taka możliwość, lepiej wybierać określone ścieżki bez korzystania z symboli jeśli nie ma takiej potrzeby.

# Oprogramowanie pośredniczące

Przejdźmy teraz do tematu middleware lub oprogramowania pośredniczącego. W praktyce oprogramowanie pośredniczące to po prostu funkcja przyjmująca trzy argumenty: obiekt zapytania (`req`), obiekt odpowiedzi (`res`) i funkcję `next`. Oprogramowanie pośredniczące wykonuje się kaskadowo lub łańcuchowo. Wyobraź sobie rurę wodociągową, którą płynie woda. Woda pompowana jest przez jeden koniec rury, następnie przepływa przez ciśnieniomierz i zawory, nasze oprogramowanie pośredniczące, zanim wpada do miejsca przeznaczenia – naszej szklanki. Podobnie jak w tej analogii, kolejność `middlewares` ma znaczenie. Na końcu łańcucha middleware znajdzie się funkcja obsługująca ścieżkę (kończąca się na przykład metodą `res.send()`)

Stwórzmy własne oprogramowanie pośredniczące w naszym pliku `app.js` przed wywołaniem dowolnej ścieżki.

```js
app.use((req, res, next) => {
  console.log('Nasze oprogramowanie pośredniczące');
  next();
});
```
Ta funkcja na ten moment nie robi nic istotnego, ale każde zapytanie przechodzi przez nią, a na konsoli zawsze będzie wyskakiwać nasza wiadomość.

![middelwere](./Images/05-middleware-pl.png)

>:bulb:Definicja
>
>Oprogramowanie pośredniczące (middleware) to funkcje mające dostęp do obiektu zapytania (req), obiektu odpowiedzi (res) i funkcji przetwarzania pośredniego w cyklu "zapytanie-odpowiedź" aplikacji. Funkcja kontynuowania wykonywania z reguły definiowana jest jako next.


Co robi więc `middleware`?

- wykonuje kod wspólny dla wielu ścieżek;
- wnosi zmiany do obiektów zapytań i odpowiedzi;
- może zakończyć cykl "zapytanie-odpowiedź" i przerwać opracowywanie zapytania;
- może wywołać następną funkcję przetwarzania z kolejki, poprzez wykonanie funkcji next().
Ważne!

>:fire:Jeżeli bieżąca funkcja przetwarzania pośredniego nie kończy cyklu "zapytanie-odpowiedź", powinna ona wywołać next() aby przejść do następnej funkcji. W przeciwny razie zapytanie zawiesi się. 

# Przekazanie danych na serwer

## Przekazanie parametru do URL
Pierwszy sposób – przekazanie przez parametr. Ścieżki mogą zawierać parametry – nazwane segmenty adresu URL. Nazwa parametru powinna zawierać symbole z przedziału [A-Za-z0-9_]. W określeniu ścieżki, przed nazwą parametru stawia się znak dwukropka. Dodamy następujący handler dla ścieżki:

Pierwszy sposób – przekazanie przez parametr. Ścieżki mogą zawierać parametry – nazwane segmenty adresu `URL`. Nazwa parametru powinna zawierać symbole z przedziału `[A-Za-z0-9_]`. W określeniu ścieżki, przed nazwą parametru stawia się znak dwukropka. Dodamy następujący handler dla ścieżki:

```js
app.get('/contact/:id', (req, res) => {
  res.send(`<h1>Contact</h1> Prametr: ${req.params.id}`);
});
```
Jeżeli teraz zwrócimy się po ścieżce `/contact/123` to `req.params.id` będzie zawierał wartość `123`. Ten sposób przekazywania parametrów na serwer wykorzystywany jest bardzo często. Na przykład aktualizacja danych użytkownika może wyglądać następująco:
```js
app.patch('/user/:userId', (req, res) => {
  const { userId } = req.params;
    // wykonujemy wymagane działania
});
```
Zobaczymy więcej przykładów gdy będziemy analizować REST API.

# Wykorzystanie parametrów zapytania `GET`
Drugi sposób – parametry zapytania GET lub też `query params`. W adresie URL, po którym następuje zwrócenie się do serwera, stawia się znak zapytania `?`, za którym następuje lista par `klucz=wartość` rozdzielonych symbolami `&`. Na przykład:
```
http://localhost:3000/contacts?skip=0&limit=10
```
To najprostszy sposób między innymi na obsłużenie paginacji wyników naszego zapytania. Rezultat takiego zapytania znajduje się w obiekcie `req.query`. W naszym konkretnym przykładzie:

```
{
  skip: "0",
  limit: "10"
}`
```
Jeżeli w zapytaniu `GET` parametry `query` nie są podane, na przykład mamy ścieżkę /`search` bez znaku zapytania i dalszych danych, to req.query domyślnie otrzyma pusty obiekt: `{}`.

Framework Express zawiera wbudowane narzędzie służące do odczytywania parametrów `query`, ponieważ jest to bardzo często spotykana praktyka.

# Wysyłanie danych przy pomocy formularza

Przy wysyłaniu danych na serwer zazwyczaj wykorzystuje się metody `HTTP`: `POST`, `PUT` i `PATCH`. Formularze HTML mogą korzystać z metody `POST`, zobaczmy więc jak odczytać z nich dane.

Żądanie `HTTP` składa się między innymi z listy nagłówków `(headers)` i ciała wiadomości`(request body)`. Zapytanie `POST` od formularza standardowo zawiera nagłówek `Content-Type: application/x-www-form-urlencoded`. Zazwyczaj w celu otrzymania wysłanych danych należy podłączyć odpowiedni parser jako middleware, jest on już zawarty w frameworku. Do utworzenia parsera danych od formularzy stosuje się funkcję `urlencoded()`.:

```js
app.use(express.urlencoded({ extended: false }));
```

Do tej funkcji przekazujemy obiekt definiujący parametry parsowania. Wartość `extended: false` wskazuje, że rezultat parsingu będzie reprezentować listę par klucz-wartość, a każda wartość może być przedstawiona jako string lub tablica. Gdy parametr ten jest równy `true`, parser wykorzystuje inną bibliotekę do analizy formatu parametrów.

Przyjmijmy informację od formularza uwierzytelnienia:

```js
<form action="/login" method="POST">
  <label for="email">Email</label>
  <input type="text" name="email" id="email" />
  <label for="password">Hasło</label>
  <input type="password" name="password" id="password" />
  <button type="submit">Zaloguj</button>
</form>
```
Po wciśnięciu przycisku w formularzu, przeglądarka wyśle na `URL` `<url naszej aplikacji>/login` dane formularza. Będą to dwie zmienne: `email` i `password`. Odpowiedzialne są za to wartości atrybutów `name` w znacznikach typu `input`. Te dane możemy odczytać po stronie serwera w następujący sposób:

```js
app.post('/login', (req, res, next) => {
  const { email, password } = req.body;
    // Wykonujemy niezbędne operacje
});
```
W wyniku tego serwer powinien otrzymać dane w obiekcie req.body, w następującej postaci:
```js
{
  email: 'Wartość wprowadzona w polu input o name=email',
  password: 'Wartość wprowadzona w polu input o name=password'
}
```
# Przekazanie JSON
Przy tworzeniu aplikacji webowych na `Node.js`, często trzeba pracować z danymi w formacie JSON. To podstawowy sposób przekazywania danych dla Web-API. Istnieje również format `XML`, jednak coraz bardziej się on starzeje z powodu swojej rozwlekłości i wychodzi z użytku. Parser `JSON` w naszej aplikacji podłączamy w następujący sposób:
```js
app.use(express.json());
```
Dane w postaci JSON mogą pochodzić między innymi

- z kodu `JavaScript`  po stronie przeglądarki,
- z zapytania z innego serwera
- zapytania `curl` dla systemu linux
- Przy użyciu kl`ienta służącego do testowania zapytań HTTP, takiego jak Postman lub Insomnia (więc o nich dowiemy się na module dotyczącym REST API)

Po tym, jak parser `JSON` zostanie podłączony, nasze `handlers` mogą interpretować wartość`req.body` jako obiekt `JavaScript` zamiast wartości string.

```js
app.post('/login', (req, res, next) => {
  const { email, password } = req.body;
    // Wykonujemy niezbędne operacje
});
```
Dany przykład wskazuje, że wysłany został obiekt `JSON` z właściwościami `email` i `password`. Co najważniejsze, w zapytaniu nagłówek `Content-Type` powinien zawierać `application/json`, a ty powinieneś wysłać właśnie wartość typu JSON.

Przeanalizowaliśmy wszystkie podstawowe sposoby przesyłania danych na serwer, które przydadzą się nam później.

# Routing w aplikacji
## Metody Route

Przy pomocy `klasy express.Router` można utworzyć modułowe, programy obsługi ścieżek (handlers). Instancja `Router` reprezentuje kompleksowy system pośredniczących programów obsługi i trasowania; z tego powodu często nazywany jest "mini-aplikacją".
```js
const express = require("express");
const router = express.Router();

// określamy bazową ścieżkę
router.get("/", (req, res) => {
  res.send("To główny router");
});

// określamy ścieżkę about
router.get("/about", (req, res) => {
  res.send("About");
});

module.exports = router;
```
Później podłączamy moduł `my-router.js` w aplikacji:
```js
const myRouter = require('./my-router');
...
app.use('/my-router', myRouter);
```
Dana aplikacja może teraz opracowywać zapytania adresowane do zasobów `/my-router` i `/my-router/about`.

Express wspiera dużą ilość metod trasowania, odpowiadających metodom `HTTP`, ale z większością nie będziemy nawet mieć do czynienia. Podstawowymi metodami dla nas będą:

- `get`
- `post`
- `put`
- `delete`
- `patch`

Istnieje także szczególna metoda `app.all()`, nie będąca odpowiednikiem konkretnej metody `HTTP`. Ta metoda wykorzystywana jest do ładowania funkcji pośredniczącego opracowywania w ścieżce dla wszystkich metod zapytań. Bywa przydatna, gdy musimy reagować na dowolne zwrócenie się do serwera.

W podanym niżej przykładzie program opracowywania będzie uruchomiony dla zapytań do dla ścieżki /anything, niezależnie od tego, czy wykorzystywany jest `GET`, `POST`, `PUT`, `DELETE` lub jakakolwiek inna metoda zapytania `HTTP`, wspierana w module `http`.
```js
app.all('/anything', (req, res, next) => {
  console.log('Anything method.');
  next();// przechodzimy do dalszej obsługi zapytania
});
```
# Metody odpowiedzi
Część metod znajdujących się w obiekcie odpowiedzi (`res`), wymienione zostały w tablicy poniżej, mogą przekazywać odpowiedź do klienta i zakończyć cykl “zapytanie-odpowiedź”. Jeżeli żadna z tych metod nie zostanie wywołana w którejkolwiek funkcji obsługi trasy, zapytanie klienta zawiesi się.

Metoda	            Opis

res.download()	Zaproszenie do ładowania pliku

res.end()	Zakończenie procesu odpowiedzi

res.json()	Wysłanie odpowiedzi JSON

res.jsonp()	Wysłanie odpowiedzi JSON ze wsparciem JSONP

res.redirect()	Przekierowanie odpowiedzi

res.render()	Wyprowadzenie szablonu widoku

res.send()	Wysłanie odpowiedzi różnych typów

res.sendFile()	Wysłanie pliku w postaci strumienia obiektów

# Łańcuchy metod
Metoda app.route() pozwala tworzyć programy opracowywania tras, łańcuchy dla konkretnej ścieżki trasy. O ile ścieżka jest taka sama, to dla różnych metod HTTP, wygodne jest tworzenie tras modułowych, aby minimalizować redundancję i ilość błędów. Niżej pokazano przykład połączonych w łańcuch programów opracowywania tras, określonych przy pomocy funkcji app.route().
```js
app
  .route("/blog")
  .get((req, res) => {
    res.send("Get a list of blog");
  })
  .post((req, res) => {
    res.send("Add a record to blog");
  })
  .put((req, res) => {
    res.send("Update blog");
  });
  ```

  # REST API
  
  # Przyklad aplikacji

  Po części teoretycznej przejdźmy do praktyki i stworzymy prostą aplikację opartą o express.

Framework Express zapewnia swój generator aplikacji https://expressjs.com/en/starter/generator.html. Generator zorientowany jest na architekturę aplikacji MVC i tworzy następującą strukturę katalogów:

```
├── app.js
├── bin
│ └── www
├── package.json
├── public
│ ├── images
│ ├── javascripts
│ └── stylesheets
│   └── style.css
├── routes
│ ├── index.js
│ └── users.js
└── views
  ├── error.pug
  ├── index.pug
  └── layout.pug
  ```
Aby zainstalować template należy użyć polecenia:
```
npx express-generator --view=ejs simple-express
```
`npx` - narzędzie, które jest już w systemie, jeśli zainstalowany został `Node.js` w wersji wyższej niż 8.x. Pozwala ono wykonywać polecenia innych narzędzi, nie instalując ich globalnie w systemie.

Dalej wskazujemy, że chcemy wykorzystać szablony `ejs` parametrem `--view=ejs` 

Jako ostatni parametr wskazujemy nazwę aplikacji (i folderu) simple-express.

Aplikacja znajduje się w pliku `app.js`. Pierwsze, co powinniśmy zrobić, to zmienić `var` na `const` w całej aplikacji. Po tej operacji plik app.js powinien wyglądać następująco:

```js
const createError = require('http-errors');
const express = require('express');
const path = require('path');
const cookieParser = require('cookie-parser');
const logger = require('morgan');

const indexRouter = require('./routes/index');
const usersRouter = require('./routes/users');

const app = express();

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');

app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', indexRouter);
app.use('/users', usersRouter);

// catch 404 and forward to error handler
app.use(function (req, res, next) {
  next(createError(404));
});

// error handler
app.use(function (err, req, res, next) {
// set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

// render the error page
  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;
```
Na początku podłączone są wszystkie niezbędne pakiety, potrzebne do działania aplikacji.

Następnie zażądane są moduły zawierające routing.
```js
const indexRouter = require('./routes/index');
const usersRouter = require('./routes/users');
```
Później tworzymy egzemplarz aplikacji i ustawiamy wykorzystanie szablonów `ejs`.

```js
const app = express();

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');
```
Następnie pojawią się podłączenia middleware
```js
app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
```
Podłącza się logger, opracowywanie `JSON` i danych z formularzy, a na koniec moduł do pracy z `cookie`.

Dalej dodawane jest opracowywanie zasobów statycznych z folderu `public`:
```js
app.use(express.static(path.join(__dirname, 'public')));
```
Następnie mamy podłączenie routerów w aplikacji:
```js
app.use('/', indexRouter);
app.use('/users', usersRouter);
```

Pamiętaj, że porządek podłączanego programu pośredniczącego ma znaczenie. Na końcu aplikacji pojawia się opracowywanie błędów. Najpierw zachodzi opracowywanie nieistniejącej ścieżki czyli klasyczny błąd 404.
```js
app.use(function (req, res, next) {
  next(createError(404));
});
```
Następnie z kolei mamy `handler` błędów które zostaną ”wyrzucone” podczas obsługi ścieżek.

```js
app.use(function (err, req, res, next) {
// set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

// render the error page
  res.status(err.status || 500);
  res.render('error');
});
````
Tutaj zachodzi opracowywanie błędu. Podajemy zmienne `message` i `error` do szablonu `error.ejs` i renderujemy go.

Wewnątrz folderu z naszą aplikacją trzeba zainstalować wszystkie pakiety z pliku package.json poprzez polecenie:
```
npm i
```
Teraz dla ułatwienia sobie pracy zainstalujemy pakiet `nodemon`. Pozwala on wykonywać live reload serwera w trakcie pracy nad kodem. Dodamy wymaganą zależność do `devDependencies`:

```
npm i nodemon -D
```
Następnie w pliku p`ackage.json` dla uruchomienia aplikacji w trybie deweloperskim dodajemy skrypt `start:dev`:
```
  "scripts": {
    "start": "node ./bin/www",
    "start:dev": "nodemon ./bin/www"
  },
```
Uruchomienie aplikacji w trybie deweloperskim będzie następowało przez polecenie:
```
npm run start:dev
```
Po uruchomieniu, aplikacja powinna wyglądać następująco po przejściu na adres [localhost:3000](http://localhost:3000) w przeglądarce:

![expres](./Images/Untitled.png)

Aplikacja wykonuje renderowanie swojego jedynego szablonu. Samo renderowanie wykonuje się w pliku routingu `routes/index.js`.
```js
router.get('/', (req, res, next) => {
  res.render('index', { title: 'Express' });
});
```
Przyszedł czas na zmianę naszej aplikacji – dodamy formularz, abyśmy mogli przyjąć dane od użytkownika. Plik `index.ejs` powinien teraz wyglądać tak:
```js
<!DOCTYPEhtml>
<html>
  <head>
    <title><%= title %></title>
    <link rel="stylesheet" href="/stylesheets/style.css" />
  </head>
  <body>
    <form action="/login" method="POST">
      <label for="email">Email</label>
      <input type="text" name="email" id="email" />
      <label for="password">Hasło</label>
      <input type="password" name="password" id="password" />
      <button type="submit">Zaloguj się</button>
    </form>
  </body>
</html>
```
Dla lepszego wyglądu dodamy następujące style do pliku `public/stylesheets/style.css`.
```scss
form {
  display: flex;
  flex-direction: column;
  width: 400px;
}

input,
button {
  margin-bottom: 15px;
}
```
Potrzebny jest nam program opracowywania dla ścieżki /login, do którego będą przychodzić dane z formularza. Dodajmy go, ale najpierw potrzebujemy nowy szablon response.ejs, gdzie będziemy pokazywać dane z formularza.
```js
<!DOCTYPEhtml>
<html>
<head>
  <title><%= title %></title>
  <link rel='stylesheet' href='/stylesheets/style.css' />
</head>
<body>
<p>Email: <%= email %></p>
<p>Password: <%= password %></p>
<a href='/'>Wróć do strony główej</a>
</body>
</html>
```
Teraz w pliku routingu dodamy program opracowywania ścieżki /login.
```js
router.post('/login', (req, res, next) => {
  const { email, password } = req.body;
  res.render('response', { title: 'Simple express app', email, password });
});
```
Jest dość uproszczony. Przyjmujemy w nim tylko dwie zmienne i przekazujemy je do renderowania szablonu response.ejs, aby pokazać, że dane zostały otrzymane. Jeżeli wszystko zostało wykonane prawidłowo, wtedy przy wysyłaniu formularza będziemy po prostu widzieli, co wysłaliśmy na serwer
![express2](./Images/02-simple-express-pl-cb2d29386e4e9198504152121878b3b5.png)
Ten przykład pokazuje przekazanie danych z frontendu do backendu, wykorzystując formularz.

Teraz w pliku routingu user.js dodamy następujący obiekt z kontaktami:
```js
const express = require('express');
const router = express.Router();
const contacts = [
  { id: '1', username: 'Felix', surname: 'Brown', email: 'felix@test.com' },
  { id: '2', username: 'Sonya', surname: 'Redhead', email: 'sonya@test.com' },
  { id: '3', username: 'Conan', surname: 'Barbarian', email: 'conan@test.com' },
];
/* GET users listing. */
router.get('/', function (req, res, next) {
  res.json(contacts);
});

module.exports = router;
```
Jeżeli zwrócimy się po ścieżce /users, serwer powinien odesłać nam JSON z tablicą kontaktów.

Dla lepszego odczytywania plików JSON w przeglądarce można wykorzystać następującą aplikację: https://github.com/callumlocke/json-formatter. Dostępna jest ona także jako rozszerzenie dla Chrome. Gdy je zainstalujesz, zawsze będziesz widział dane JSON w czytelnej wersji.

Dodajmy program opracowywania dla otrzymania konkretnego użytkownika zgodnie z jego identyfikatorem:
```js
router.get('/:id', function (req, res, next) {
  const { id } = req.params;
  const contact = contacts.filter(el => el.id === id);
  res.json(contact);
});
```
Teraz po zwróceniu się do url /users/2 otrzymujemy dane Rudej Soni:
```js
[
  {
    "id": "2",
    "username": "Sonya",
    "surname": "Redhead",
    "email": "sonya@test.com"
  }
]
```
Taki sposób przekazywania danych będziemy najczęściej wykorzystywać do redagowania i usuwania konkretnego podmiotu zgodnie z jego unikalnym identyfikatorem.

Z pełnym kodem wskazanego przykładu można zapoznać się tutaj:

https://glitch.com/~simple-express-nodebook

# Zmienne środowiskowe

Gdy zaczynamy pisać aplikację webową, prędzej czy później spotkamy się z potrzebą wykorzystania zewnętrznych źródeł danych, serwisów, danych kont API i tak dalej. Dostęp do tych źródeł prawie zawsze realizowany jest przy pomocy tajnych kluczy. Jest to spory problem, gdy kod aplikacji jest przekazywany do ogólnodostępnego repozytorium, takiego jak GitHub. Kod dostępny jest dla wszystkich, którzy mają do niego dostęp, co oznacza, że nasze tajne klucze również są widoczne. Nawet prywatne repozytoria mogą nie być do końca bezpieczne.

Jak rozwiązywany jest ten problem? Dobrą praktyką jest wykorzystanie zmiennych środowiskowych. To innymi słowy lokalne zmienne, które są dostępne dla naszej aplikacji, ustalane podczas jej uruchamiania.

Dostęp do tych zmiennych często realizowany jest przy pomocy modułu `dotenv`. Paczka ta ładuje zmienne środowiskowe z pliku `.env`, który należy utworzyć np. w głównym katalogu naszej aplikacji. Następnie podłączamy moduł do naszej aplikacji, po czym dodaje on zmienne środowiskowe do obiektu process.env, przy użyciu którego możemy wykorzystać je w aplikacji. Sam plik `.env` powinniśmy dodać do pliku `.gitignore` aby przez przypadek nie znalazł się w repozytorium.

Na początku zainstalujmy pakiet.
```
npm install dotenv
```
Teraz dodajmy do pliku `app.js` następującą instrukcję.
```js
require('dotenv').config();
```
Następnie tworzymy plik `.env` w katalogu root naszej aplikacji i dodajemy do niego zmienne.
```js
SECRET_KEY=123456
NODE_ENV=development
```
Teraz w pliku aplikacji `app.js` dostępne będą wszystkie zmienne, które dodajemy do pliku `.env`. Przykłady powyżej, dostępne są teraz w aplikacji w następujący sposób:
```js
process.env.SECRET_KEY;
process.env.NODE_ENV;
```
Dalej będziemy wykorzystywać zmienne środowiskowe do dostępu do tajnych danych, takich jak klucze szyfrujące dla `cookie` lub `jwt`, `url` podłączenia do bazy danych i inne.
# Logowanie

W prawie każdej aplikacji niezbędne jest odnotowywanie zapytań do serwera. Generator aplikacji wykorzystuje w tym celu moduł morgan — elastyczny komponent pośredniczący do logowania informacji o zapytaniach z możliwością ustawienia formatu.

Aby wykorzystać ten moduł należy skorzystać z następujących instrukcji:
```js
const logger = require('morgan');
...
app.use(logger('dev'))
```
Przy tworzeniu middleware, wykorzystujemy jeden z dostępnych formatów naszych logów - dev. Istnieje pięć zdefiniowanych formatów, które możemy wykorzystać

- `combined` - wykorzystuje tryb `combined` serwera Apache:
  
```js
:remote-addr - :remote-user [:date[clf]] ":method :url HTTP/:http-version" :status :res[content-length] ":referrer" ":user-agent"
```
- `common` - wykorzystuje tryb `common` serwera Apache:

```js
:remote-addr - :remote-user [:date[clf]] ":method :url HTTP/:http-version" :status :res[content-length]
```
- dev - format dziennika z kolorowym kodowaniem (po statusie odpowiedzi).
-   zielony dla kodów zakończonych sukcesem,
-   czerwony dla kodów z błędami serwera,
-   żółty dla kodów z błędami klienta,
-   turkusowy dla kodów przekierowania
-   biały dla kodów informacyjnych
  
```
:method :url :status :response-time ms - :res[content-length]
```
![logger](./Images/logger-6e0b43f2038253c78acc5c892f38380f.png)
- `short` krótszy odpowiednik formatu domyślnego

```js
:remote-addr :remote-user :method :url HTTP/:http-version :status :res[content-length] - :response-time ms
```
- `tiny` - najkrótszy ze zdefiniowanych formatów, zawiera tylko czas odpowiedzi i kilka elementów uzupełniających.

```js
:method :url :status :res[content-length] - :response-time ms
```
Często nasze logi muszą zostać opracowane przez zewnętrzne oprogramowanie, dzięki użyciu Morgan, można tworzyć również własne formaty logów. W tym celu należy przekazać specjalny string zawierający odpowiednie markery. Na przykład przekazując następujący format :method :url :response-time ms będziemy widzieć zapisy w rodzaju:

```js
GET / 15 ms:
```
Domyślnie dostępne są następujące markery. Możesz również określać niestandardowe markery i dodać je do informacji wyprowadzanych przez logger.

# Czym jest REST
## Wprowadzenie

REST (REpresentational State Transfer) - to inaczej przekazywanie reprezentatywnego stanu. Jest to lista zasad projektowania architektury, dla zwiększenia skalowalności i elastyczności komunikacji sieciowych. Zasady te odpowiadają na szereg pytań:

- Jakie istnieją komponenty systemu?
- Jak komunikują się one między sobą?
- Jak można zagwarantować możliwość zmiany i rozwinięcia różnych części systemu w dowolnym czasie?
- Jak można skalować systemy obsługi coraz większej ilości użytkowników?

Roy Fielding po raz pierwszy wprowadził termin REST w 2000 roku w swojej pracy doktorskiej "Architectural Styles and the Design of Network-based Software Architectures". Można ją znaleźć tutaj.

W momencie publikacji dysertacji Internet był już bardzo popularny. Fielding zrobił krok do tyłu i przeanalizował cechy, które sprawiły, że Internet odniósł większy sukces niż konkurencyjne rozwiązania. Przedstawił on schematyczną strukturę, która uczyni komunikację internetową "podobną do sieci". Dlatego też, REST to ogólny zbiór zasad, niespecyficznych dla Internetu. Można je stosować do innych typów sieci, na przykład do wewnętrznych systemów. REST nie jest również protokołem, ponieważ nie definiuje on szczegółów implementacji. W pracy doktorskiej Fieldinga przedstawiony został szereg architektonicznych ograniczeń, które powinien spełniać system, aby być RESTful.

# Czym jest URI i URL?

Zanim przejdziemy do tego, jakie architektoniczne ograniczenia nakłada REST, przeanalizujmy terminologię URL i URI. Terminy URI i URL często wykorzystywane są zamiennie, ale to nie do końca to samo.

::: tip URI

Reprezentuje sobą identyfikator konkretnego zasobu, jak np. strona, książka lub dokument.

:::

::: tip URL

Reprezentuje szczególny typ identyfikatora zasobu, który zawiera również informację o tym, jak otrzymać do niego dostęp.

:::

W zasadzie URI to szersze pojęcie i zawiera w sobie URL. Jeżeli chcemy przeprowadzić jakieś upraszczające analogie, to można założyć, że URI jest rodzajem nazwy, a URL to konkretna nazwa i sposób, jak do niej dotrzeć. Na przykład ISBN książki to URI, a https://goit.ua to URL – mamy tu nazwę oraz sposób dotarcia do niej, czyli protokół.

To oznacza, że jeżeli protokół (https, ftp itd.) jest obecny w ścieżce, to powinniśmy nazywać ją adresem URL, nawet jeśli jest ona także URI.

Tradycyjnie zapisujemy URL w formacie:
```
<scheme>://[<login>[:<password>]@]<host>[:<port>]][/<path>][?<query>][#<fragment>]
```
Przyklad:
```
http://user:password@host.com:80/resourse/path/?query=name&ttt=123#hash
```
W tym zapisie:

- scheme - to protokół sieciowy, poprzez który następuje zwrócenie się do zasobu;
- login - nieobowiązkowy parametr, nazwa użytkownika wykorzystywana aby uzyskać dostęp do zasobu;
- password - hasło dla wskazanego użytkownika, jeśli jest wymagane;
- host - zapisana w całości domenowa nazwa hosta w systemie DNS (goit.ua) lub adres IP hosta w postaci czterech grup liczb dziesiętnych (jeśli korzystamy z formatu IPV4), rozdzielonych kropkami;
- port - port hosta dla danego połączenia. Pamiętamy klasyczna ścieżkę do aplikacji express: http://localhost:3000 gdzie 3000 to właśnie port. Pojawia się pytanie, dlaczego port nie pokazuje się dla URL w przeglądarce. Wynika to z tego, że domyślnie, dla protokołu http port to 80, a dla https jest równy 443 i przeglądarka podstawia go za nas.
- path - ścieżka prowadząca do miejsca znajdowania się zasobu;
- query - łańcuch parametrów zapytania z przekazywanymi na serwer (metodą GET) parametrami. Zaczyna się od symbolu ?, następnie znajdziemy delimiter poszczególnych parametrów – znak &. Przykład: ?foo=123&baz=234&bar=value;
- fragment - identyfikator z poprzedzającym symbolem #. Często nazywamy go kotwicą. Z takim odnośnikiem przeglądarka otworzy stronę i przewinie okno przeglądania do wskazanego elementu z odpowiednim atrybutem id, na przykład: https://example.com/#contact

# Architektoniczne ograniczenia rozwiązań RESTful
## Klient-serwer

Pierwsze ograniczenie wynika z tego, że sieć powinna składać się z klientów i serwerów. Serwer to komputer, który przechowuje zasoby, a klientami mogą być na przykład przeglądarki, które chcą wykorzystać zasoby przechowywane na serwerze. Gdy przeglądasz Internet, twój komputer zachowuje się jak klient i wysyła zapytania HTTP na serwer w celu uzyskania dostępu do informacji i zarządzania nimi. Każdy system RESTful powinien działać zgodnie z modelem klient-serwer, nawet jeśli dany element systemu czasem zachowuje się jak klient, a czasem jak serwer (ponieważ serwery mogą komunikować się między sobą i wtedy jeden z nich przyjmuje rolę klienta w danych połączeniu).

Alternatywą dla architektury klient-serwer, odmienną od RESTful, jest architektura na podstawie zdarzeń. W tym modelu każdy komponent nieprzerwanie transmituje zdarzenia, oczekując przy tym odpowiednich zdarzeń od innych komponentów. Nie ma bezpośredniej komunikacji, tylko transmisja i nasłuchiwanie. REST wymaga indywidualnej współpracy elementów systemu, dlatego architektura integracji na podstawie zdarzeń nie będzie RESTful.

## Stateless
Brak stanu nie oznacza, że serwery i klienci nie mają swoich stanów. Oznacza to jednak, że nie mogą śledzić swoich stanów nawzajem. Gdy klient nie jest aktualnie połączony z serwerem, serwer nie wie o jego istnieniu i na odwrót. Każde zapytanie analizowane jest oddzielnie, co oznacza brak ciągłej sesji.

## Jeden interfejs

Ograniczenie to gwarantuje, że istnieje wspólny język między serwerami i klientami, który pozwala wymieniać lub zmieniać część bez naruszenia pracy całego systemu. Jest to osiągalne kosztem 4 dodatkowych ograniczeń:

- identyfikacja zasobów;
- manipulowanie zasobami przy pomocy widoków;
- informacyjne (samoopisujące się) wiadomości;
- hipermedia.

## Pierwsze ograniczenie interfejsu: identyfikacja źródeł

Pierwsze z tych ograniczeń wpływa na sposób identyfikacji zasobów. W terminologii REST zasobem może być cokolwiek – dokument HTML, plik, informacja o zamówieniu i tak dalej. Każdy zasób powinien być jednoznacznie identyfikowalny stabilnym identyfikatorem. "Stabilny" identyfikator oznacza, że nie zmienia się on przy interakcji z zasobem, ani nawet przy zmianie stanu zasobu. Jeżeli zasób naprawdę przemieścił się do innego identyfikatora, serwer powinien dać klientowi odpowiedź wskazującą, że zapytanie było błędne i dać mu odnośnik do nowego położenia zasobu.

Internet wykorzystuje URL do identyfikacji zasobów oraz HTTP jako standard łączności. Aby otrzymać zasób, który przechowywany jest na serwerze, klient wysyła zapytanie HTTP – GET na URL, który identyfikuje ten zasób. Za każdym razem, gdy wprowadzasz do swojej przeglądarki adres, tworzy ona zapytanie GET na podany URL. Jeżeli otrzymuje status 200 i dokument w formacie który rozumie (np.HTML), wyświetla stronę w oknie, aby można było ją przejrzeć.

## Drugie ograniczenie interfejsu: manipulowanie zasobami przed widoki

Drugie ograniczenie mówi o tym, że klient zarządza zasobami, wysyłając “widoki” na serwer – powinien być to obiekt JSON lub XML, zawierający dane, który użytkownik chciałby dodać, usunąć lub zmienić. W REST serwer w pełni kontroluje zasoby i odpowiada za wszelkie zmiany. Jako zasoby mogą służyć zapisy w bazie danych, pliki i tak dalej. Gdy klient chce wprowadzić zmiany do zasobów, wysyła do serwera widok tego, jak powinien wyglądać otrzymany zasób. Serwer przyjmuje zapytanie jako propozycję, ale ma pełną kontrolę i sam podejmuje działanie w zależności od zdefiniowanych zasad.

Najprostszy przykład to blog. Gdy użytkownik tworzy nowy wpis na blogu, komputer-klient zawiadamia serwer, że należy go dodać. W tym celu wysyła on zapytanie HTTP przy użyciu metody POST, lub ewentualnie PUT z zawartością dla nowego posta na blogu. Serwer odpowiada do klienta i przekazuje informację, czy wpis był utworzony lub czy powstał problem z jego utworzeniem.

## Trzecie ograniczenie interfejsu: samoopisujące się (self-descriptive) wiadomości

Wiadomości informacyjne – kolejne ograniczenie, pomaga ujednolicić interfejs między klientami i serwerem. Wiadomość samoopisująca się to taka, która zawiera wszystkie niezbędne informacje, które niezbędne są odbiorcy dla jej zrozumienia.

W celu zrozumienia, jak odnosi się to do Internetu, przeanalizujemy później zbiór metod zapytań HTTP i kody odpowiedzi. Poniżej zobaczmy krótki przykład:

Gdy użytkownik wprowadza http://www.example.com w polu adresu swojej przeglądarki internetowej, przeglądarka wysyła następujące zapytanie HTTP:

```js
GET / HTTP/1.1
Host: www.example.com
```
Ta wiadomość jest informacyjna, ponieważ zawiadamia serwer, jaka metoda HTTP została wykorzystana oraz jaki jest protokół użytkownika (HTTP 1.1).

Serwer może wysłać odpowiedź podobną do tej:
```js
HTTP/1.1 200 OK
Content-Type: text/html

<!DOCTYPE html>
<html lang="en-US">
  <head>
    <meta charset="UTF-8" />
    <title>Your Site Title Here</title>
  </head>
  <body>
    Hello world!
    <a href="https://goit.ua">GoIT world!</a>
    <img src="https://goit.ua/wp-content/themes/1/images/Layer.png">
  </body>
</html>
```
Ta wiadomość jest informacyjna (samoopisowa), ponieważ informuje klienta, jak powinien interpretować ciało wiadomości wskazując na przykład, że Content-type to text/html. Klient otrzymuje w jednej wiadomości wszystko co jest mu potrzebne, aby opracować ją w odpowiedni sposób.

Dodatkowo widzimy kod odpowiedzi 200 OK, który oznacza, że nie było problemów z realizacją naszego żądania.

## Czwarte ograniczenie interfejsu: hipermedia

Ostatnie ograniczenie interfejsu, to ograniczenie hipermediów. Hipermedia to nazwa dla danych wysyłanych z serwera do klienta, które będą zawierać informację o tym, co klient może zrobić dalej, innymi słowy, jakie dalsze zapytania możemy wykonać. W REST serwery powinny wysyłać do klientów tylko hipermedia. HTML to jedna z odmian hipermediów.

Zobaczmy przykład:
```js
<a href="https://goit.ua">GoIT world!</a>
```
Informuje przeglądarkę, że powinna wykonać zapytanie GET na https://goit.ua, jeśli użytkownik kliknie na odnośnik.
```js
<img src="https://goit.ua/wp-content/themes/1/images/Layer.png" />
```
Informuje przeglądarkę, że trzeba niezwłocznie wysłać zapytanie GET na https://goit.ua/wp-content/themes/1/images/Layer.png, aby mogła pokazać zdjęcie (również przykład hipermediów) użytkownikowi.

# Buforowanie
Buforowanie odnosi się do ograniczenia, przy którym odpowiedzi serwera powinny być oznaczone jako buforowalne lub niebuforowalne. Mówimy o nim wtedy, gdy klient zapisuje poprzednie odpowiedzi otrzymane od serwera, ponieważ, gdy te dane znów będą przydatne, nie musi robić zapytań do sieci, a wykorzysta już posiadaną część danych. Buforowanie częściowo lub w pełni eliminuje niektóre interakcje klient-serwer, rozwijając skalowalność i wydajność aplikacji. Wykorzystywane jest np. przy serwisach streamingowych takich jak Netflix.

# Wielowarstwowy (wielopoziomowy) system

Wielowarstwowy (wielopoziomowy) system polega na tym, że komponentów może być więcej niż tylko serwery i klienci. Oznacza to, że w systemie może być więcej niż jeden poziom. Jednakże każdy komponent powinien być tak ograniczony, aby widzieć i współdziałać tylko z następną warstwą. Dla przykładu, proxy to dodatkowy komponent, który przekazuje zapytania HTTP na odpowiednie serwery lub inne proxy. Serwery proxy są przydatne dla implementacji dodatkowych warstw bezpieczeństwa. Serwer proxy pełni rolę serwera dla klienta, który wysyła zapytanie, a następnie działa już jako klient, gdy przekazuje to zapytanie dalej do właściwych serwerów.

API Gateway to kolejny przykład dodatkowej warstwy, która przekazuje zapytanie HTTP do odpowiedniego serwera, a następnie odpowiada otrzymanymi informacjami. Wykorzystywana jest często przy architekturze mikroserwisów.

## Kod zapytania

Nieobowiązkowe ograniczenie, które odnosi się do możliwości serwera dotyczących wysyłania wykonalnego kodu do klienta. Na przykład gdy dokument HTML jest załadowany, przeglądarka automatycznie ładuje kod plików JavaScript z serwera i wykonuje go lokalnie.
# Podsumowanie
W ten sposób system RESTful to dowolna sieć podlegająca analizowanym ograniczeniom. System RESTful powinien być elastyczny dla różnych rodzajów wykorzystania, skalowalny dla podsumowania dużej ilości użytkowników i komponentów i możliwy do adaptowania z biegiem czasu. Pamiętaj jednak, że REST to projekt teoretyczny a nie konkretne implementacje.
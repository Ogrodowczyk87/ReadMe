- [Single-Page Application](#single-page-application)
- [Renderowanie warunkowe przy pomocy operatora logicznego \&\&](#renderowanie-warunkowe-przy-pomocy-operatora-logicznego-)
- [:bulb: wazne](#bulb-wazne)
- [Renderowanie warunkowe przy pomocy conditional ternary operator ( ? : )](#renderowanie-warunkowe-przy-pomocy-conditional-ternary-operator----)
- [Style wbudowane](#style-wbudowane)
- [:bulb: WAZNE](#bulb-wazne-1)
- [Vanilla CSS](#vanilla-css)
- [Tworzenie klas CSS](#tworzenie-klas-css)
- [Normalizacja stylu](#normalizacja-stylu)
- [Komponenty klasy](#komponenty-klasy)
- [Zdarzenia](#zdarzenia)
- [Licznik](#licznik)
- [Funkcje obsługi zdarzeń](#funkcje-obsługi-zdarzeń)
- [Powiązanie this](#powiązanie-this)
- [Powiązanie w trakcie przekazywania callbacku](#powiązanie-w-trakcie-przekazywania-callbacku)
- [Powiązanie w konstruktorze](#powiązanie-w-konstruktorze)
- [Publiczne właściwości klasy](#publiczne-właściwości-klasy)
- [Wewnętrzny stan komponentu](#wewnętrzny-stan-komponentu)
- [Stan początkowy w zależności od props](#stan-początkowy-w-zależności-od-props)
- [Zmiana stanu komponentu](#zmiana-stanu-komponentu)
- [Jak aktualizuje się stan](#jak-aktualizuje-się-stan)
- [Asynchroniczność aktualizacji stanu](#asynchroniczność-aktualizacji-stanu)
- [setState z funkcją](#setstate-z-funkcją)


# Single-Page Application

Współczesne podejście zakłada tworzenie stron dynamicznych, na których użytkownik nigdy nie przechodzi na inne strony HTML. Interfejs, zamiast otrzymywać kolejne dokumenty HTML z serwera, dynamicznie zmienia się w przeglądarce. Odbywa się to na tej samej stronie, bez ponownego ładowania dokumentu.


![single-Page Aplication ](./Images/SinglePageAplication.png)


:bulb: Single-page application (SPA) wyróżnia:

     
- Architektura klient-serwer  
- Podczas pierwszego ładowania strony serwer zawsze zwraca stronę startową - dokument index.html
- Każde kolejne zapytanie do serwera wykonuje się jedynie celem wymiany danych w formacie JSON
- Aktualizacja interfejsu odbywa się dynamicznie w kliencie (w przeglądarce)
- Pierwsze załadowanie strony może być dość wolne (dlatego, że zawiera ona cały interfejs)
- Logika niezwiązana z bezpieczeństwem znajduje się w kliencie
- Gorsze SEO, w porównaniu do MPA (można to jednak naprawić)
- Lepsza skalowalność i utrzymywalność kodu

---

# Renderowanie warunkowe przy pomocy operatora logicznego &&


# :bulb: wazne

>children to jest to co definujemy miedzy nawiasami!!

---

Czyta się jako: jeśli warunek sprowadza się do true, wyrenderuj wszystko na prawo od operatora.

```JS
const Mailbox = ({ unreadMessages }) => {
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 && (
        <p>You have {unreadMessages.length} unread messages.</p>
      )}
    </div>
  );
};
```

# Renderowanie warunkowe przy pomocy conditional ternary operator ( ? : )

Czyta się jako: jeśli warunek sprowadza sie do true, wyrenderuj wszystko po ?, w przeciwnym razie wyrenderuj przekazaną wartość po : .

```JS
const Mailbox = ({ username, unreadMessages }) => {
  return (
    <div>
      <h1>Hello {username}</h1>
      {unreadMessages.length > 0 ? (
        <p>You have {unreadMessages.length} unread messages.</p>
      ) : (
        <p>No unread messages.</p>
      )}
    </div>
  );
};
```

Ostatni przykład można zapisać inaczej, rezultat będzie taki sam.

```JS
const Mailbox = ({ name, unreadMessages }) => {
  return (
    <div>
      <h1>Hello {name}</h1>
      <p>
        {unreadMessages.length > 0
          ? `You have ${unreadMessages.length} unread messages.`
          : "No unread messages."}
      </p>
    </div>
  );
};
```


---

# Style wbudowane

Istnieje kilka sposobów stylowania komponentów, z których najprostszym (ale jednocześnie najbardziej ograniczonym) są style wbudowane. W tym celu używany jest atrybut HTML style, który w składni JSX React przyjmuje obiekt styli (w przeciwieństwie do oryginalnej składni HTML, która oczekuje łańcucha znaków).

# :bulb: WAZNE
>zamaist myslinkow jak w css uwywa sie cammelCase  
background-color: red;        <---- css  
 backgroundColor: "gray",     <---- wbudowany styl
```JS
const App = () => {
  return (
    <p
      style={{
        margin: 8,
        padding: "12px 16px",
        borderRadius: 4,  // wartosc liczbowa domyslnie jest w px
        backgroundColor: "gray",
        color: "white",
      }}
    >
      Please update your email!
    </p>
  );
};
```

Na podstawie powyższego przykładu możemy wyróżnić kilka reguł dla obiektu style:

- Nazwy właściwości składające się z dwóch lub więcej słów, takie jak background-color, muszą być zapisane w notacji camelCase (backgroundColor). Analogicznie jak w przypadku odwoływania się do właściwości obiektu style elementu DOM.
- Przyrostek px zostanie automatycznie dodany do wartości liczbowych większości właściwości. Jeśli konieczne jest użycie jednostek innych niż px lub jeśli wartość składa się z wielu części, może być ona określona jako łańcuch znaków.

Przenieśmy obiekt style do zmiennej, aby poprawić czytelność znaczników JSX.

```JS
const alertStyles = {
  margin: 8,
  padding: "12px 16px",
  borderRadius: 4,
  backgroundColor: "gray",
  color: "white",
};

const App = () => {
  return (
    <>
      <p style={alertStyles}>Please update your email!</p>
      <p style={alertStyles}>There was an error during transaction!</p>
      <p style={alertStyles}>Payment received, thank you for your purchase!</p>
    </>
  );
};
```

Na podstawie powyższego przykładu możemy wyróżnić kilka reguł dla obiektu style:

- Nazwy właściwości składające się z dwóch lub więcej słów, takie jak background-color, muszą być zapisane w notacji camelCase (backgroundColor). Analogicznie jak w przypadku odwoływania się do właściwości obiektu style elementu DOM.
- Przyrostek px zostanie automatycznie dodany do wartości liczbowych większości właściwości. Jeśli konieczne jest użycie jednostek innych niż px lub jeśli wartość składa się z wielu części, może być ona określona jako łańcuch znaków.

 >Przenieśmy obiekt style do zmiennej, aby poprawić czytelność znaczników JSX.

 ```JS
 const alertStyles = {
  margin: 8,
  padding: "12px 16px",
  borderRadius: 4,
  backgroundColor: "gray",
  color: "white",
};

const App = () => {
  return (
    <>
      <p style={alertStyles}>Please update your email!</p>
      <p style={alertStyles}>There was an error during transaction!</p>
      <p style={alertStyles}>Payment received, thank you for your purchase!</p>
    </>
  );
};
```

Style inline mogą wydawać się wygodne ze względu na łatwość ich użycia, ale mają wiele istotnych wad.

- Bardzo słaba skalowalność i ponowne wykorzystanie styli w innych miejscach aplikacji
- Ograniczone funkcje (pseudoklasy, pseudoelementy, właściwości adaptacyjne)
- Kiepska wydajność podczas renderowania dużej liczby elementów
- Brak wygodnych narzędzi programistycznych ułatwiających pracę ze stylami
- Brak wsparcia popularnych narzędzi, takich jak autoprefixer
- 

>Wnioski   
>
>W praktyce style wbudowane są używane tylko dla wartości właściwości CSS obliczanych dynamicznie, w połączeniu z zewnętrznymi arkuszami stylów. Nie są one jednak zalecane i dlatego nie powinny być używane w projektach.


# Vanilla CSS

Style komponentu można również umieścić w arkuszu stylów. W tym przypadku style każdego komponentu są deklarowane w osobnym pliku CSS z rozszerzeniem .css. Nazwa pliku powinna się składać z nazwy komponentu i rozszerzenia. Na przykład dla komponentu Alert arkusz stylów miałby nazwę Alert.css.

```css
.alert {
  margin: 8px;
  padding: 12px 16px;
  border-radius: 4px;
  background-color: gray;
  color: white;
}
```

# Tworzenie klas CSS

Dodajmy klasy CSS dla każdego typu alertu, aby kontrolować kolor tła na podstawie właściwości variant. Dla wygody nazwijmy poszczególne klasy analogicznie jak nasze zdefiniowane stany właściwości variant.

```css
.alert {
  margin: 8px;
  padding: 12px 16px;
  border-radius: 4px;
  color: white;
}

.alert.info {
  background-color: blue;
}

.alert.success {
  background-color: green;
}

.alert.error {
  background-color: red;
}

.alert.warning {
  background-color: orange;
}
```

Dodajmy jeszcze dwie opcjonalne właściwości (props) outlined i elevated do komponentu Alert. Wartości jakie będą one przyjmować to true, false lub undefined. Jeśli wartość właściwości będzie równa true dodamy odpowiednie klasy is-outlined i is-elevated do elementu <p>.

```css
/* Cały poprzedni kod CSS */

.alert.is-outlined {
  outline: 1px solid black;
}

.alert.is-elevated {
  box-shadow: rgb(0 0 0 / 20%) 0px 3px 3px -2px, rgb(0 0 0 / 14%) 0px 3px 4px 0px,
    rgb(0 0 0 / 12%) 0px 1px 8px 0px;
}
```

Proces obliczania końcowej wartości atrybutu className zależy od dewelopera i bieżącego zadania. W poniższym przykładzie używamy tablicy łańcuchów i bloku if. Klasa alert w połaczeniu z wariantem jest zawsze dodana do tablicy. Natomiast klasy dla właściwości elevated i outlined zostaną dodane tylko wtedy, gdy spełniony zostanie odpowiedni warunek.

```js
import "./Alert.css";

const Alert = ({ variant, outlined, elevated, children }) => {
  const classNames = ["alert", variant];

  if (outlined) classNames.push("is-outlined");
  if (elevated) classNames.push("is-elevated");

  return <p className={classNames.join(" ")}>{children}</p>;
};
```
---

>Brak standardu :bulb:
>
>Aby obliczyć ostateczną wartość atrybutu className, moglibyśmy użyć bloku if...else, instrukcji switch, operatora warunkowego lub innej składni JavaScript dającej analogiczny wynik. Najważniejsze jest to, aby ostateczna wartość atrybutu była poprawnie skomponowana i nie zawierała dodatkowych, lub nieprawidłowych wartości.

# Normalizacja stylu

Style elementów mogą się różnić w zależności od przeglądarki. Aby nadać im jednakowy wygląd, konieczne może być dodanie zestawu reguł, które w jak największym stopniu korygują te różnice.

W aplikację Create React App wbudowane jest narzędzie PostCSS Normalize, będące mieszanką najlepszych praktyk normalizacji (normalize.css oraz sanitize.css). Wszystko co musisz zrobić to dodać dyrektywę @import-normalize; w dowolnym miejscu arkusza styli lub module CSS. Zduplikowane importy zostaną automatycznie usunięte, więc dyrektywę wystarczy dodać raz, np. do index.css.

```css 
@import-normalize;

/* Pozostały kod CSS */
```

>:bulb:Ustawienia VSCode
>
>Jeśli zobaczysz ostrzeżenie «Unknown at rule @import-normalize css(unknownAtRules)» w VSCode, to zmień wartość parametru 'css.lint.unknownAtRules' w ustawieniach na 'ignore'.

Pozostaje teraz tylko zaimportować plik styli index.css (z włączoną normalizacją) w dowolnym miejscu naszej aplikacji.

```css
import "./index.css";

/* Reszta kodu z pliku */
```

Oprócz standaryzacji wyglądu elementów, przydatne może być również zresetowanie lub dodanie globalnych styli elementów. Na przykład wcięcia list i nagłówków, style obrazów, style elementów <body> i tym podobne. Logiczne będzie zrobienie tego w tym samym pliku, w którym dodano normalizację.

```css
@import-normalize;

body {
  font-family: sans-serif;
  line-height: 1.5;
}

h1,
h2,
h3,
h4,
h5,
h6,
p {
  margin: 0;
}

ul,
ol {
  margin: 0;
  padding: 0;
}

img {
  display: block;
  max-width: 100%;
  height: auto;
}
```

# Komponenty klasy

Komponenty tworzymy jako klasy, kiedy niezbędne jest dodanie do nich dynamiki. Dotychczas komponenty funkcyjne były ograniczone możliwościami tylko do otrzymywania propsów. Nie jest to już prawda, odkąd w React udostępnione zostały hooki (od wersji React 16.8), natomiast zostanie to omówione w późniejszych rozdziałach.

![single-Page Aplication ](./Images/class-component.jpg)

- Zwykła klasa ES6, dlatego stosujemy wymaganą składnię JavaScript: konstruktor, metody, kontekst (this).
- Obowiązkowo rozszerza klasę podstawową React.Component.
- Działa jak funkcja, która otrzymuje props, ale dostęp do właściwości odbywa się z użyciem kontekstu (this.props).
- Należy zadeklarować obowiązkową metodę render(), która zwraca elementy JSX. Zostanie ona wywołana automatycznie przez Reacta.
- Użycie komponentu klasy spowoduje, że React za każdym będzie tworzył nowy egzemplarz komponentu (klasy). Dlatego dostęp do propsów przebiega przez this.props.
- Można określić niestandardowe metody klasy i wykorzystać je w dowolnym miejscu, w tym również wewnątrz JSX.
- Zmiana stanu komponentu lub jego propsów spowoduje ponowne renderowanie ("re-render").

```JS
// Używaj importów nazwanych zamiast składni `React.Component`, zwiększa to czytelność kodu
import React, { Component } from "react";

class MyClassComponent extends Component {
  static defaultProps = {};

  static propTypes = {};

  render() {
    return <div>Class Component</div>;
  }
}
```


# Zdarzenia

Dla natywnego zdarzenia przeglądarki React posiada obiekt-opakowanie SyntheticEvent Object z identycznym interfejsem. Jest to niezbędne, aby zapewnić kompatybilność z różnymi przeglądarkami i zoptymalizować wydajność.

```JS
<button onClick={event => console.log(event)}>Click me!</button>
```

- Obsługa zdarzeń z wykorzystaniem EventTarget.addEventListener() praktycznie nie jest niewykorzystywana (poza kilkoma wyjątkami).
- Propsy zdarzeń nie są wyjątkiem i nazywane są w notacji camelCase, np. onClick, onChange, onSubmit, onMouseEnter.
- Do propsu zdarzenia przekazujemy referencję do funkcji (callback), która zostanie wywołana w przypadku wystąpienia danego zdarzenia.
- Funkcje obsługi zdarzeń otrzymują egzemplarz SyntheticEvent Object.
- 
W React "pod maską" realizowane jest delegowanie zdarzeń. "Listenery" nie są dodawane bezpośrednio do elementów DOM. Przekazanie callback'a to po prostu rejestracja funkcji, która będzie wywołana przez wewnętrzne mechanizmy React'a w przypadku wystąpienia zdarzenia.

# Licznik

Stwórzmy komponent-licznik, który docelowo będzie miał możliwość zwiększania i zmniejszania wartości.


```JS
import React, { Component } from "react";
import ReactDOM from "react-dom";

class Counter extends Component {
  static defaultProps = {
    step: 1,
  };

  render() {
    const { step } = this.props;

    return (
      <div>
        <span>0</span>
        <button type="button">Increment by {step}</button>
        <button type="button">Decrement by {step}</button>
      </div>
    );
  }
}

ReactDOM.render(<Counter step={5} />, document.getElementById("root"));
```


# Funkcje obsługi zdarzeń

Najczęściej funkcje obsługi zdarzeń deklaruje się jak metody klasy, a następnie do atrybutu JSX przekazywana jest referencja do danej metody.

```JS
class Counter extends Component {
/* ... */

  handleIncrement(evt) {
    console.log("Increment button was clicked!", evt);// działa
    console.log("this.props: ", this.props);// Error: cannot read props of undefined
  }

  handleDecrement(evt) {
    console.log("Decrement button was clicked!", evt);// działa
    console.log("this.props: ", this.props);// Error: cannot read props of undefined
  }

  render() {
    const { step } = this.props;

    return (
      <div>
        <span>0</span>
        <button type="button" onClick={this.handleIncrement}>
          Increment by {step}
        </button>
        <button type="button" onClick={this.handleDecrement}>
          Decrement by {step}
        </button>
      </div>
    );
  }
}
```

# Powiązanie this

Należy zawsze pamiętać o wartości this w metodach wykorzystywanych jako funkcje callback. W JavaScripcie kontekst w metodach klasy nie przywiązuje się domyślnie. Jeśli zapomni się o powiązaniu kontekstu to w czasie wywołania funkcji (w ramach obsługi zdarzenia) this pozostanie nieokreślony.

# Powiązanie w trakcie przekazywania callbacku

Unikaj powiązywania kontekstu w metodzie render(). Za każdym razem, gdy komponent renderuje się ponownie, Function.prototype.bind() zwraca nową funkcję i przekazuje ją w dół drzewa komponentów. Pprowadzi do powtórnego renderowania komponentów dzieci. Może to mieć istotny wpływ na wydajność.

```JS 
// ❌ Źle
class Counter extends Component {
/* ... */

  handleIncrement(evt) {
// ...
  }

  handleDecrement(evt) {
// ...
  }

  render() {
    const { step } = this.props;

    return (
      <div>
        <span>0</span>
        <button type="button" onClick={this.handleIncrement.bind(this)}>
          Increment by {step}
        </button>
        <button type="button" onClick={this.handleDecrement.bind(this)}>
          Decrement by {step}
        </button>
      </div>
    );
  }
}
```

# Powiązanie w konstruktorze

Kontekst można również powiązać w konstruktorze klasy. Jednak można sobie wyobrazić, o ile zwiększy się otrzymamy konstruktor, jeśli funkcji będzie wiele.

-  Konstruktor wykonuje się jeden raz, dlatego bind również zostanie wywołane tylko jeden raz.
-  Metody klasy zapisywane są we właściwości prototype funkcji-konstruktora.



```JS
// ✅ Nieźle
class Counter extends Component {
/* ... */

  constructor() {
    super();
    this.handleIncrement = this.handleIncrement.bind(this);
    this.handleDecrement = this.handleDecrement.bind(this);
  }

  handleIncrement(evt) {
// ...
  }

  handleDecrement(evt) {
// ...
  }

  render() {
    const { step } = this.props;

    return (
      <div>
        <span>0</span>
        <button type="button" onClick={this.handleIncrement}>
          Increment by {step}
        </button>
        <button type="button" onClick={this.handleDecrement}>
          Decrement by {step}
        </button>
      </div>
    );
  }
}
```

# Publiczne właściwości klasy
Rekomendowany sposób przywiązania kontekstu to składnia publicznych pól klasy. Po wywołaniu publicznych pól klasy, zapisują się one nie we właściwości prototype funkcji-konstruktora, a w obiekcie egzemplarza klasy.

```JS
// ✅ Super
class Counter extends Component {
/* ... */

  handleIncrement = evt => {
    console.log("Increment button was clicked!", evt);// działa
    console.log("this.props: ", this.props);// działa
  };

  handleDecrement = evt => {
    console.log("Decrement button was clicked!", evt);// działa
    console.log("this.props: ", this.props);// działa
  };

  render() {
    const { step } = this.props;

    return (
      <div>
        <span>0</span>
        <button type="button" onClick={this.handleIncrement}>
          Increment by {step}
        </button>
        <button type="button" onClick={this.handleDecrement}>
          Decrement by {step}
        </button>
      </div>
    );
  }
}
```

# Wewnętrzny stan komponentu

Stan komponentu (state) pozwala nam dynamicznie aktualizować interfejs użytkownika w odpowiedzi na jego działania. Za każdym razem, gdy zmienia się stan komponentu (lub propsy), wywoływana jest metoda render(). W stanie powinniśmy przechowywać jedynie minimalny, niezbędny zestaw danych, potrzebny do prawidłowego zaktualizowania interfejsu użytkownika.

![stan komponentu](./Images//reactivity.jpg)

Stan należy do komponentu klasowego i można go zmienić tylko za pomocą metod zdefiniowanych w obrębie klasy. Zmiana stanu komponentu nigdy nie aktualizuje jego rodzica i sąsiadów. Aktualizacji podlegają natomiast wszystkie elementy dzieci danego komponentu. W takim modelu dane w aplikacji przekazują się tylko w jeden konkretny sposób nazywany jednokierunkowym przepływem danych (one way data flow).v

![stan komponentu](./Images/data-flow.jpg)

Stan deklaruje się w konstruktorze, ze względu na to, że jest on wywoływany jako pierwszy podczas tworzenia egzemplarza klasy.

```JS
class Counter extends Component {
  constructor() {
    super();

    this.state = {
      value: 0,
    };
  }

/* ... */

  render() {
    return (
      <div>
        <span>{this.state.value}</span>
        {/* ... */}
      </div>
    );
  }
}
```

# Stan początkowy w zależności od props
Czasami konieczne może być aby początkowy stan zależał od przekazanych propsów, np. początkowa wartość naszego licznika. W takim przypadku należy jawnie zadeklarować parametr props w konstruktorze i przekazać go do wywołania super(props). Dopiero wtedy w konstruktorze będzie dostępne this.props.

```JS
class Counter extends Component {
  static defaultProps = {
    step: 1,
    initialValue: 0,
  };

  constructor(props) {
    super(props);

    this.state = {
      value: this.props.initialValue,
    };
  }

/* ... */
}

ReactDOM.render(<Counter initialValue={10} />, document.getElementById("root"));
```

Możemy jednak pominąć męczące deklarowanie konstruktora i zdefiniować stan jako publiczną właściwość klasy. Traspilator (Babel) zajmie się wszystkim za nas.

```JS
class Counter extends Component {
  static defaultProps = {
    step: 1,
    initialValue: 0,
  };

  state = {
    value: this.props.initialValue,
  };

/* ... */
}
```

# Zmiana stanu komponentu
Aktualizacja stanu komponentu odbywa się z wykorzystaniem odziedziczonej metody setState().

```JS
setState(updater, callback);
```

- Jako pierwszy, obowiązkowy argument przekazujemy obiekt z polami wskazującymi, jaką część stanu chcemy zmienić.
- Jako drugi, nieobowiązkowy argument można przekazać funkcję callback, która wykona się po zmianie stanu.

>⚠️ danger
>
>Obiekt stanu state to właściwość klasy, jednak nigdy nie wolno jej zmieniać bezpośrednio.

```JS
state = { fullName: "Poly" };

// ❌ Źle - mutacja stanu
this.state.fullName = "Mango";

// ✅ Dobrze
this.setState({
  fullName: "Mango",
});
```

Stwórzmy komponent z przełącznikiem, którego metody będą nadpisywać wartość isOpen w stanie.

```JS
class Toggle extends Component {
  state = { isOpen: false };

  show = () => this.setState({ isOpen: true });

  hide = () => this.setState({ isOpen: false });

  render() {
    const { isOpen } = this.state;
    const { children } = this.props;

    return (
      <>
        <button onClick={this.show}>Show</button>
        <button onClick={this.hide}>Hide</button>
        {isOpen && children}
      </>
    );
  }
}
```

# Jak aktualizuje się stan
Aktualizując stan przy pomocy setState() nie musimy przekazywać wszystkich właściwości przechowywanych w stanie. Wystarczy przekazać jedynie tą część stanu, którą chcemy zmienić w danym momencie. React sam zadba o poprawną aktualizację stanu biorąc aktualny stan i obiekt, który został przekazany w setState(). Odbywa się to zgodnie ze schematem:


```JS
// stan przed połączeniem
const currentState = { a: 2, b: 3, c: 7, d: 9 };

// obiekt przekazany w setState
const updateSlice = { b: 5, d: 4 };

// nowa wartość this.state po połączeniu
const nextState = { ...currentState, ...updateSlice };// {a: 2, b: 5, c: 7, d: 4}
```

# Asynchroniczność aktualizacji stanu

Tak naprawdę metoda setState() nie zmienia stanu natychmiast. Rejestruje ona asynchronicznną operację aktualizacji stanu, która staje w kolejce aktualizacji. Może się również zdarzyć tak, że kilka aktualizacji zostanie połączonych w jedną, w celu polepszenia wydajności. Ze względu na asynchroniczność aktualizacji, dostęp do this.state w synchronicznym kodzie może zwrócić wartość stanu sprzed aktualizacji.

Wyobraź sobie, że w trakcie zmiany stanu polegasz na jego obecnej wartości. Wykorzystamy pętlę for do stworzenia (rejestracji) kilku operacji zmiany stanu.

```JS
// Zaczynamy z następującym stanem:
state = { value: 0 };

// Rozpocznynamy pętlę i wywołujemy 3 operacje zmiany stanu
for (let i = 0; i < 3; i += 1) {
// Ponieważ to synchroniczny kod i aktualizacja stanu jeszcze nie zaszła
  console.log(this.state.value);

  this.setState({ value: this.state.value + 1 });
}
```

Powyższy fragment kodu zwróci w konsoli wartość 0 dla każdej iteracji pętli.

Wyjaśnienie:

Wartość właściwości this.state.value jest zapamiętywana w czasie tworzenia obiektu przekazywanego do setState(), a nie w czasie aktualizacji stanu. Oznacza to, że jeśli w czasie utworzenia obiektu, this.state.value miało wartość 0, to do funkcji setState() przekazany zostanie obiekt {value: 0 + 1}.

W wyniku wykonania pętli otrzymamy kolejkę aktualizacji z 3 obiektów { value: 0 + 1 }, { value: 0 + 1 }, { value: 0 + 1 } i oryginalny stan w momencie aktualizacji { value: 0 }. Po wszystkich aktualizacjach otrzymamy stan { value: 1 }.

Z tego względu nie można polegać na obecnym stanie podczas obliczania następnego (zależnego od poprzedniego w momencie aktualizacji). Rozwiązaniem tego problemu jest drugi sposób na aktualizację stanu.

# setState z funkcją

Metoda setState() jako pierwszy argument może przyjmować nie tylko obiekt, ale również funkcję. Niemniej, funkcja taka powinna w dalszym ciągu zwrócić obiekt, którym chcemy zaktualizować stan.

```JS
setState((state, props) => {
  return {};
}, callback);
```
Aktualny stan i propsy zostaną przekazane do funkcji na czas jej wykonywania. W ten sposób można być pewnym poprawnej wartości poprzedniego stanu podczas tworzenia następnego.

```JS
state = { value: 0 };

for (let i = 0; i < 3; i += 1) {
  console.log(this.state.value);// 0

  this.setState(prevState => {
    console.log(prevState.value);// zwróci poprawne wartości stanu podczas każdej iteracji

    return { value: prevState.value + 1 };
  });
}
```

Poprawmy zatem komponent przełącznika Toggle.

```JS
class Toggle extends Component {
  state = { isOpen: false };

  toggle = () => {
    this.setState(state => ({ isOpen: !state.isOpen }));
  };

  render() {
    const { isOpen } = this.state;
    const { children } = this.props;

    return (
      <div>
        <button onClick={this.toggle}>{isOpen ? "Hide" : "Show"}</button>
        {isOpen && children}
      </div>
    );
  }
}
```

Natomiast licznik będzie wyglądał następująco:

```JS
class Counter extends Component {
/* ... */

  handleIncrement = () => {
    this.setState((state, props) => ({
      value: state.value + props.step,
    }));
  };

  handleDecrement = () => {
    this.setState((state, props) => ({
      value: state.value - props.step,
    }));
  };

/* ... */
}

```
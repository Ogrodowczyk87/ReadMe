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
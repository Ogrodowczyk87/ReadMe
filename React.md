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
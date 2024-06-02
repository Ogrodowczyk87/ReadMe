- [Start nowego projektu](#start-nowego-projektu)
- [Normalizacja stylu](#normalizacja-stylu)
- [Biblioteka JS do haszowania API](#biblioteka-js-do-haszowania-api)
- [Redux od czego zaczac](#redux-od-czego-zaczac)


# Start nowego projektu 

```JS
// Vite
npm create vite@latest
// React 
npx create-react-app my-app
```

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
npm 
```

# Biblioteka JS do haszowania API

https://www.npmjs.com/package/md5

# Redux od czego zaczac

- provider na app po pierwsze
- potem przechodzimy do stora i w zaleznosci co chcemy tast jakis np taskow
- wtedy tworzymy slica od taska createSlice i tworzymy reducery i wyciagamy akcje i reducer i wrzucamy do stora

#rozwoj

Konfy:
AppJS conf
React Berlin Conference


Ogarniać Typescript
I Reacta
https://react.dev/learn

https://nextjs.org/ - do frontu


BE: NodeJS
(ExpressJS)

https://nestjs.com/ - do backendu


https://reactnative.dev/

GitHub Co pilot

Pad Nine
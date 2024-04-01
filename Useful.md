- [Normalizacja stylu](#normalizacja-stylu)


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
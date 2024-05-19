- [**Browser DOM**](#browser-dom)
- [**Virtual DOM**](#virtual-dom)
- [pytania na temat formularzy](#pytania-na-temat-formularzy)
- [useEffect !!!](#useeffect-)
- [useCotext](#usecotext)
- [useMemo](#usememo)
- [useRef](#useref)
- [Gdzie stosowac Redux-a](#gdzie-stosowac-redux-a)


>  :bulb: **DOM** - The Document Object Model
>

# **Browser DOM**   
>Pozwala na przedstawienie dokumentu HTML w postaci struktury drzewa, w którym węzły odpowiadają poszczególnym elementom. Przechowywany jest on w przeglądarce i ma bezpośredni związek z tym, co widzimy na stronie.
Po każdej zmianie w DOM przeglądarka wykonuje kilka skomplikowanych operacji. Dlatego częste aktualizacjie drzewa wpływają negatywnie na wydajność i responsywność (czas odpowiedzi) interfejsu.


# **Virtual DOM**  

>Abstrakcja, będąca niedokładnym odwzorowaniem rzeczywistego drzewa DOM w postaci dokumentu JSON.
Istnieje tylko w pamięci i nie renderuje się w przeglądarce
Nie zależy od wewnętrznej implementacji przeglądarki
Wykorzystuje dobre praktyki aktualizacji rzeczywistego DOM (optymalizacja poprzez grupowanie aktualizacji tzw. batching)


> VDOM przekopiuje naprzyklad wszytskie elementy a tek ktory sie zmienil wyrendreuje na nowo.. taki szybki przyklad

---

# pytania na temat formularzy

Trzeba pamietac zeby przygotowac sie z tworzenia formularzy!

:bulb: bez funkcji 

```js
const onSubmit = (event) => {
    event.preventDefault()
  }
  ```
  przeladuje storne poniewaz zachodzi bombelkowanie, bombelek idzie w gore az do obiektu **window** i przeladowuje strone.- [**Browser DOM**](#browser-dom)

  # useEffect !!!

```js
useEffect(() => {
  // kod ktory chcemy uruchomic

  // opcjonalnie return function (jest to obcja ukryta)
}, [])

//dependency array mowi czego powinien sluchac zeby useEffect aby uruchomic kod, jezeli sotawimy pusty nie utzymamy efektu ktory chcemy uzyskac
```

# useCotext

Store to miejsce dzieki ktoremu provider pozwala sie dostac
do zmiennych dostepnych globalnie.


# useMemo
czy zawsze stosujemy memo i czy to jesgt takie zajebiste :)

Jest bezsensu wtedy kiedy opercja jest zbyt prosta, i wykonanie jej jest szybsze niz sprawdzenie jej w cache-u w
tedy memonizacja nie ma sensu.

# useRef 

Odchodzi sie juz od niego. Problem polega na tym ze Ui nie zostanie przeladowany.

# Gdzie stosowac Redux-a

Na pewno warto to przenalizowac z architektem oprogramowania, na pewno w duzych aplikacjach ktore zarzadzaja duzym stanem, magazynowym. W maplych aplikacjach wystrczy context
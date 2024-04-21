- [**Browser DOM**](#browser-dom)
- [**Virtual DOM**](#virtual-dom)
- [pytania na temat formularzy](#pytania-na-temat-formularzy)
- [useEffect !!!](#useeffect-)


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
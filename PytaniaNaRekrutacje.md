

**Browser DOM**   
>Pozwala na przedstawienie dokumentu HTML w postaci struktury drzewa, w którym węzły odpowiadają poszczególnym elementom. Przechowywany jest on w przeglądarce i ma bezpośredni związek z tym, co widzimy na stronie.
Po każdej zmianie w DOM przeglądarka wykonuje kilka skomplikowanych operacji. Dlatego częste aktualizacjie drzewa wpływają negatywnie na wydajność i responsywność (czas odpowiedzi) interfejsu.


**Virtual DOM**  

>Abstrakcja, będąca niedokładnym odwzorowaniem rzeczywistego drzewa DOM w postaci dokumentu JSON.
Istnieje tylko w pamięci i nie renderuje się w przeglądarce
Nie zależy od wewnętrznej implementacji przeglądarki
Wykorzystuje dobre praktyki aktualizacji rzeczywistego DOM (optymalizacja poprzez grupowanie aktualizacji tzw. batching)



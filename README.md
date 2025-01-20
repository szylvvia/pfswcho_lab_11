# Laboratorium 11: Programowanie FullStack w Chmurze Obliczeniowej

## Jaka jest różnica pomiędzy skalowaniem StatefulSet a Deployment?

<p>Główna różnica polega na zarządzaniu stanem aplikacji. Ponieważ obiektów Deployment używa się dla skalowania aplikacji stateless, usuwanie czy dodawanie podów/replik odbywa się bez względu na pozostałych instancji. Dla obiektu Deployment skalowanie zarówno w góre, jak i w dół jest prostsze ponieważ każdy z podów jest traktowany identycznie nie ma uporządkowanej kolejności uruchamiania i synchronizacji kolejnych pod-ów i ich zależności. Tworzone i usuwane są wszytskie na raz. </p>
<p>Obiekty typu StatefullSet pozwalają za zarządzanie stanem poszczególnych podów, ze względu na wykorzystanie unikalnych identyfikatorów oraz trwałego stanu. Gdy pody są wdrażane (skalowanie w góre), są one tworzone sekwencyjnie od 0 do n replik. Natomiast w przypadku skalowania w dół, usuwane są w dowrotnej kolejności od n do 0. Zanim operacja skalowania zostanie zastosowana, wszystkie jego poprzedniki muszą być uruchomione i gotowe. Zanim Pod zostanie zakończony, wszyscy jego następcy muszą być całkowicie zamknięci.</p>

> Dla StatefulSet z 3 replikami (web-0, web-1, web-2) -> skalowanie w dół:  web-2 zostałby zakończony jako pierwszy, web-1 nie zostałby zakończony, dopóki web-2 nie zostanie w pełni zamknięty i usunięty i analogicznie dalej w zależności do ile replik skalujemy.

> Dla StatefulSet z 1 replika (web-0) -> skalowanie w górę: web-1 nie zostanie wdrożony, zanim web-0 nie będzie uruchomiony i gotowy, a web-2 nie zostanie wdrożony, dopóki web-1 nie będzie uruchomiony i gotowy.

## Utworzenie bazy danych jako obiektu Deployment i przetestowanie skalowania

![image](https://github.com/user-attachments/assets/84c88bca-01fe-4ea7-923b-ef5e691c51f3)

Skalowanie w górę obiektu Deployment

![image](https://github.com/user-attachments/assets/03940140-3874-421d-b79d-cb4543b66ce3)

Skalowanie w dół obiektu Deployment

![image](https://github.com/user-attachments/assets/e0b9826c-23b8-4d14-9870-23572518839d)

![image](https://github.com/user-attachments/assets/4b8e4104-04d4-4ec9-82bf-2878014e43bf)

<p> Zgodnie z założeniami dla obiektu Deployment wszytskie podu uruchamiane są na raz w przypadku skalowania w górę oraz usuwane w przypadku skalowania w dół. </p>

## Utworzenie bazy danych jako obiektu StatefulSet i przetestowanie skalowania

Utworzono pliki manifestów dla PV oraz PVC aby podczas skalowania mogły zostac tworzone automatycznie kolejne trwałe magazyny danych. Zapewniają, że każda replika w StatefulSet ma dostęp do trwałego i unikalnego dysku. PVC tworzone przez StatefulSet automatycznie otrzymuje powiązany PV, co umożliwia skalowanie aplikacji z zachowaniem trwałości danych.

![image](https://github.com/user-attachments/assets/574d684e-4714-461f-afaa-b8b48e893b26)

Skalowanie w górę obiektu StatefulSet

![image](https://github.com/user-attachments/assets/85b044d2-fc3b-47fc-a5e5-da66d452e808)

![image](https://github.com/user-attachments/assets/d00003c0-07f4-4537-bb8c-fe7595df3ed7)

Skalowanie w dół obiektu StatefulSet

![image](https://github.com/user-attachments/assets/b3777ffd-ff3f-4325-bc61-a32cae8c6b7a)

<p>Zgodnie z założeniami dla obiektu StatefulSet pody są uruchamiane sekwencyjnie, w kolejności od 0 do n replik, przy jednoczesnej kontroli stanu. Kolejne obiekty nie są uruchamiane dopoki poprzedni nie jest poprawnie uruchomiony i gotowy do pracy.</p><br>
<p>Przy skalowaniu w dół pody są usuwane w kolejności odwrotnej do tworzenia (od n do 0), analogicznie przy jednoczesnej kontroli stanu. Zanim Pod zostanie zakończony, wszyscy jego następcy muszą być całkowicie zamknięci.</p>

## Podsumowanie

Przedstawienie procesu skalowania bazy danych zarówno dla obiektu typu Deployment oraz StatefulSet zakończyło się pomyślnie. Na zamieszczonych zrzutach widoczna jest różnica w skalowaniu zarówno w górę i w dół dla poszczególnych typów obiektów. Główną różnicą jest uruchamianie i zamykanie podów w ściśle określonej kolejnosci z jednoczesną kontrolą stanu dla obiektów typu StatefulSet. 
<br>
<hr>
<p><i>Opracowanie zadania powstało w ramach laboratorium</i></p>





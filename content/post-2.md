+++
title = "Wykrywanie anomalii w ruchu morskim z wykorzystaniem algorytmów RL"
date = 2025-06-08

[taxonomies]
tags = ["Analiza danych", "Nauczanie przez wzmacnianie", "Sieci neuronowe", "RL"]
categories = ["AI", "RL"]

+++

W dzisiejszym świecie, gdzie transport morski odpowiada za ponad 80% globalnego handlu, monitorowanie ruchu statków jest kluczowe dla zapewnienia bezpieczeństwa, ochrony środowiska i przeciwdziałania nielegalnym działaniom. System AIS (Automatic Identification System) dostarcza szczegółowych danych o pozycji, prędkości i kursie jednostek pływających, umożliwiając analizę ich trajektorii w czasie rzeczywistym. Jednak w ogromie tych danych kryją się potencjalne anomalie – nietypowe manewry, wyłączanie nadajników AIS czy dryfowanie – które mogą wskazywać na problemy, takie jak przemyt, nielegalne połowy czy ryzyko kolizji. W tym artykule przyjrzymy się, jak nauczanie przez wzmacnianie (Reinforcement Learning, RL) może zrewolucjonizować wykrywanie anomalii w ruchu morskim, a także pokaże prosty przykład implementacji w Pythonie.



## Czym jest nauczanie przez wzmacnianie?


Nauczanie przez wzmacnianie to dziedzina uczenia maszynowego, w której agent uczy się podejmować decyzje poprzez interakcję ze środowiskiem. Agent wykonuje akcje, obserwuje ich skutki (stan środowiska i nagrodę), a następnie optymalizuje swoją strategię, by maksymalizować przyszłe nagrody. W kontekście danych AIS, RL może analizować strumień informacji o ruchu statków, ucząc się odróżniać normalne trajektorie od potencjalnych anomalii, takich jak nagłe zmiany kursu czy prędkości.

Kluczowe elementy RL to:

- Stan: Aktualne dane AIS, np. pozycja, prędkość, kurs.

- Akcja: Klasyfikacja ruchu jako normalnego lub nie normalnego (anomalia).

- Nagroda: Sygnał zwrotny, np. +1 za poprawną identyfikację anomalii, -1 za błąd.

- Polityka: Strategia agenta określająca, jakie akcje podejmować w danym stanie.



## Dlaczego RL w wykrywaniu anomalii morskich?


Tradycyjne metody wykrywania anomalii, takie jak algorytmy statystyczne czy uczenie nienadzorowane, mają swoje ograniczenia. Wymagają one często dużych ilości oznakowanych danych lub zakładają statyczność wzorców ruchu. RL oferuje unikalne zalety:

- Adaptacyjność: Agent uczy się w czasie rzeczywistym, dostosowując się do zmieniających się warunków, np. nowych szlaków żeglugowych.

- Minimalna potrzeba etykiet: RL może działać z ograniczonym feedbackiem, ucząc się na podstawie nagród.

- Sekwencyjność: Idealnie nadaje się do analizy trajektorii, gdzie decyzje zależą od wcześniejszych stanów.


Przykłady zastosowań RL w danych AIS obejmują:

- Wykrywanie nielegalnych działań: Identyfikacja statków wyłączających AIS w podejrzanych strefach.

- Zapobieganie kolizjom: Rozpoznawanie nietypowych manewrów w zatłoczonych szlakach.


## Jak to działa? Prosty przykład w Pythonie

Na potrzeby blog posta stworzono prototypowy system w Pythonie, który demonstruje zastosowanie RL do wykrywania anomalii w danych AIS. Poniżej opisane jest, jak działa kod i jakie możliwości oferuje.


1. Generator danych AIS

System generuje syntetyczne trajektorie statku, zawierające:

- Pozycję (szerokość i długość geograficzna),

- Prędkość (w węzłach),

- Kurs (w stopniach),

- Czas (w sekundach).


Normalne ruchy symulują płynną żeglugę z niewielkimi zmianami prędkości i kursu, podczas gdy anomalie to nagłe zmiany, np. zwrot o 90° lub skok prędkości o ±5 węzłów. Anomalie występują z prawdopodobieństwem 10%, co odzwierciedla rzadkość takich zdarzeń w rzeczywistości.


2. Środowisko RL

Użyto biblioteki gym, aby stworzyć środowisko, w którym:

- Stan: Wektor danych AIS [lat, lon, speed, course, time].

- Akcje: Klasyfikacja punktu jako normalnego (0) lub nie normalnego (1).

- Nagroda: +1 za poprawną klasyfikację, -1 za błędną.

3. Agent Q-learning

Zaimplementowano prosty algorytm Q-learning, który uczy się poprzez aktualizację Q-tabeli. Agent balansuje między eksploracją (losowe akcje) a eksploatacją (wybór najlepszej akcji na podstawie dotychczasowej wiedzy). Choć Q-learning jest podstawowym algorytmem, dobrze ilustruje koncepcję RL w tym zastosowaniu.


4. Wizualizacja i wyniki

Po przeszkoleniu agenta generowany jest wykres trajektorii, gdzie:

- Niebieskie punkty oznaczają normalny ruch.

- Czerwone krzyżyki wskazują prawdziwe anomalie.

- Zielone kółka pokazują anomalie przewidziane przez agenta.

Przykładowy wynik pokazuje dokładność wykrywania anomalii na poziomie 90-97% po 100 epizodach treningu. Trajektoria i predykcje są zapisywane do pliku CSV, co ułatwia dalszą analizę.


## Wyzwania i przyszłość RL w danych AIS

Mimo obiecujących wyników, prototyp posiada wiele ograniczeń:

- Uproszczony model: Syntetyczne dane nie uwzględniają złożoności rzeczywistych warunków morskich, takich jak prądy czy przepisy żeglugowe.

- Skalowalność: Q-learning nie radzi sobie dobrze z ciągłymi danymi AIS. Zaawansowane algorytmy, jak Deep Q-Networks (DQN) czy Proximal Policy Optimization (PPO), mogą poprawić wyniki.

- Rzadkość anomalii: Anomalie są rzadkie, co utrudnia projektowanie funkcji nagrody i wymaga starannego balansowania eksploracji.


Przyszłe prace mogą obejmować:

- Integrację z rzeczywistymi danymi AIS: Wykorzystanie bibliotek takich jak pyais lub API MarineTraffic do analizy prawdziwych trajektorii.

- Zaawansowane algorytmy RL: Zastosowanie DQN lub PPO dla lepszej obsługi dużych i ciągłych danych.

- Dynamiczne nagrody: Włączenie feedbacku od ekspertów morskich, np. straży przybrzeżnej, do projektowania nagród.


## Zastosowania w praktyce

RL w wykrywaniu anomalii AIS ma ogromny potencjał:

- Bezpieczeństwo morskie: Wykrywanie statków dryfujących lub w tarapatach.

- Ochrona środowiska: Monitorowanie zrzutów odpadów w obszarach chronionych.

- Walka z przemytem: Identyfikacja podejrzanych trajektorii w podejrzanych regionach.

Przykładem sukcesu jest projekt Global Fishing Watch, który już wykorzystuje uczenie maszynowe do monitorowania nielegalnych połowów na podstawie danych AIS. RL może dodatkowo zwiększyć skuteczność takich systemów, adaptując się do nowych wzorców w czasie rzeczywistym.

## Podsumowanie

Nauczanie przez wzmacnianie otwiera nowe możliwości w analizie danych AIS, umożliwiając dynaminaszczne i adaptacyjne wykrywanie anomalii w ruchu morskim. Przygotowany prosty prototyp w Pythonie pokazuje, jak RL może klasyfikować nietypowe zachowania statków, osiągając wysoką dokładność przy minimalnych wymaganiach dotyczących danych. Choć przed projektem jest jeszcze wiele wyzwań, takich jak skalowalność i integracja z rzeczywistymi danymi, RL ma szansę stać się kluczowym narzędziem w nowoczesnym monitorowaniu mórz.


## Link do projektu

Poniżej znajduje się repozytorium związane z zagadnieniem o którym była mowa.
-  [Link do repozytorium](https://github.com/BobakMariusz/reinforcement-learning)

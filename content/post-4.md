+++
title = "Nienadzorowane wykrywanie anomalii w ruchu morskim z wykorzystaniem Transformera"
date = 2025-12-30

[taxonomies]
tags = ["Analiza danych", "Sieci neuronowe", "Transofrmers", "AIS", "Autoencoder"]
categories = ["AI", "DL"]

+++

# Jak wykrywać dziwne zachowania statków na morzu dzięki sztucznej inteligencji?

Wyobraź sobie, że tysiące statków na całym świecie co chwilę nadają swoją pozycję, prędkość i kurs – to system **AIS** (Automatic Identification System). Dzięki temu wiemy, gdzie są kontenerowce, promy czy jachty. Ale czasem coś jest nie tak: statek nagle „przeskakuje” o kilka kilometrów, jedzie z prędkością 50 węzłów (jak szybka motorówka, a nie wielki tankowiec) albo po prostu stoi w miejscu przez godziny.

Takie dziwne sytuacje mogą oznaczać zwykły błąd GPS, awarię nadajnika albo… celowe oszustwo (tzw. **spoofing AIS**). W tym wpisie pokażę w prosty sposób, jak za pomocą sztucznej inteligencji (konkretnie modelu Transformer) można automatycznie wyłapywać takie podejrzane zachowania.

## Skąd dane?

Użyto prawdziwych, publicznie dostępnych danych AIS z jednego dnia – **1 stycznia 2025 roku** (źródło: duńska agencja morska). Dla uproszczenia wzięto trajektorię jednego statku – ponad 27 tysięcy punktów z jego pozycją, prędkością i kursem w ciągu całej doby.

## Co zrobiono z danymi?

1. **Wyczyszczono je** – usunięto oczywiste błędy (np. pozycje poza mapą świata czy prędkości ujemne). 
2. **Dodano przydatne informacje** – obliczono rzeczywistą odległość między kolejnymi punktami, prawdziwą prędkość i kierunek ruchu.
3. **Ujednolicono czas** – statek nie raportuje danych co sekundę, więc "wypełniono" luki, żeby ruch był płynny (jak film zamiast serii zdjęć).

### Jak działa detektor?

Zbudowano model typu **autoencoder** oparty na Transformerze (tym samym mechanizmie, co ChatGPT).

Prosto mówiąc:
- Pokazujemy modelowi tysiące przykładów „normalnego” ruchu statku.
- Model uczy się, jak wygląda typowa trajektoria.
- Kiedy dostanie coś dziwnego, nie potrafi tego dobrze odtworzyć → pojawia się duży błąd → alarm: „coś jest nie tak!”.

Nie musimy ręcznie pisać reguł typu „jeśli prędkość > 30 węzłów, to podejrzane”. Model sam zauważa, że coś nie pasuje do wzorca.

### Co potrafi wykryć?

Przetestowano go na sztucznie przygotowanej trajektorii z kilkoma celowo wprowadzonymi problemami:

- Statek nagle „teleportuje się” o kilka kilometrów → **wykryte!**
- Przyspiesza do zupełnie nierealnej prędkości → **wykryte!**
- Zatrzymuje się na długo i dryfuje → **wykryte!**
- Zaczyna chaotycznie zmieniać kierunek (jakby „szwendał się” po morzu) → **wykryte!**

Na wykresie błędu rekonstrukcji widać wyraźne szczyty dokładnie w miejscach, gdzie coś było nie tak.

### Dlaczego to fajne?

- Działa automatycznie na ogromnych ilościach danych.
- Nie trzeba znać wszystkich możliwych rodzajów oszustw – model sam się ich uczy.
- Można go uruchomić w czasie rzeczywistym, żeby od razu alarmować o podejrzanym statku.

### Podsumowanie

Sztuczna inteligencja coraz lepiej radzi sobie z wykrywaniem anomalii w danych morskich. Dzięki prostemu modelowi typu autoencoder możemy w kilka sekund sprawdzić, czy ruch statku wygląda normalnie, czy coś budzi wątpliwości.

Jeśli zajmujesz się bezpieczeństwem morskim, logistyką albo po prostu lubisz dane i morze – takie narzędzia mogą być bardzo przydatne. A najlepsze jest to, że cały proces da się powtórzyć na własnych danych w kilka godzin.


## Link do projektu

Poniżej znajduje się repozytorium związane z zagadnieniem o którym była mowa.
-  [Link do repozytorium](https://github.com/BobakMariusz/ais-anomaly-detection)
-  [Plik Jupiter Notebook z wykresami](https://github.com/BobakMariusz/ais-anomaly-detection/blob/main/ais-prediction-lstm.ipynb)

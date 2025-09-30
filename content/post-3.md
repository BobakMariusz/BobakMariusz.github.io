+++
title = "Przewidywanie Trajektorii Okrętów z Wykorzystaniem Sztucznej Inteligencji: Analiza Danych AIS"
date = 2025-09-30

[taxonomies]
tags = ["Analiza danych", "LSTM", "Sieci neuronowe", "Transofrmers", "AIS"]
categories = ["AI", "DL"]

+++

W dzisiejszych czasach systemy AIS (Automatic Identification System) rewolucjonizują zarządzanie ruchem morskim, zapewniając bezpieczeństwo i efektywność na morzach. Wykorzystując dane GPS, systemy te pozwalają statkom nadawać informacje o swojej pozycji, kursie i prędkości. Jednak dane AIS to nie tylko narzędzie do monitorowania bieżącej sytuacji – mogą być kluczem do przewidywania przyszłych lokalizacji jednostek, co otwiera drzwi do budowy zaawansowanych systemów wczesnego ostrzegania, np. w ochronie infrastruktury morskiej. W tym artykule przybliżymy, jak sztuczna inteligencja (AI), a konkretnie sieci neuronowe LSTM i Transformer, może zostać wykorzystana do przewidywania trajektorii okrętów na podstawie danych AIS.



## Cel i metodologia badań


Celem badania było zbadanie i porównanie efektywności dwóch modeli sztucznej inteligencji – sieci LSTM (Long Short-Term Memory) oraz Transformerów – w przewidywaniu przyszłych pozycji okrętów na podstawie danych AIS. Kluczowe pytanie brzmiało: czy współczesne metody AI są w stanie sprostać wyzwaniom związanym z przewidywaniem ruchu jednostek morskich?Dane do analizy pochodziły z publicznie dostępnego zbioru AIS (https://web.ais.dk/aisdata/aisdk-2025-01-01.zip), obejmującego informacje z 1 stycznia 2025 roku. Zbiór zawierał takie parametry jak znacznik czasu, pozycja geograficzna (długość i szerokość), prędkość względem ziemi (SOG), kurs względem ziemi (COG) oraz unikalny identyfikator MMSI.




## Przygotowanie danych: Inżynieria cech i czyszczenie


Pierwszym krokiem w analizie było przygotowanie danych, co obejmowało inżynierię cech i czyszczenie zbioru. Dane AIS często zawierają braki – w analizowanym zbiorze brakowało około 8,5% wartości SOG i 15% COG. Aby uzupełnić te luki, wykorzystano wzór Haversine’a do obliczania prędkości (SOG) na podstawie różnic w pozycjach i czasie, a COG wyznaczono na podstawie azymutu między punktami. Dzięki temu zredukowano brakujące wartości SOG do zaledwie 0,5%.Kolejnym etapem było przekształcenie współrzędnych geograficznych z układu EPSG:4326 (długość i szerokość geograficzna) na EPSG:4978, który lepiej odwzorowuje ruch w przestrzeni trójwymiarowej (X, Y, Z). Dodano także składowe prędkości w osiach X i Y (vx, vy), obliczone na podstawie SOG i COG, co wzbogaciło zbiór danych o informacje kluczowe dla modeli uczenia maszynowego.Dane zostały podzielone na 60-minutowe segmenty, próbkowane co 1 sekundę z interpolacją liniową, a następnie znormalizowane za pomocą MinMaxScaler, aby wartości cech (X, Y, Z, SOG, COG, vx, vy) mieściły się w przedziale [0, 1]. Finalny zbiór danych podzielono na: 70% do treningu, 15% do walidacji i 15% do testów.




## Modele AI: LSTM i Transformer

Do przewidywania pozycji okrętów wykorzystano dwa modele:

- LSTM: Sieć Long Short-Term Memory, idealna do analizy sekwencji czasowych, takich jak trajektorie. Model miał dwie warstwy, 128 jednostek w warstwie ukrytej, dropout 0.2, funkcję aktywacji ReLU oraz funkcję straty MSE. Optymalizatorem był Adam z prędkością uczenia 0.00001. Trening trwał maksymalnie 1000 epok z mechanizmem wczesnego zatrzymania (patience=100).
- Transformer: Architektura oparta na mechanizmie atencji, skuteczna w modelowaniu złożonych zależności w sekwencjach. Model miał jedną warstwę, 8 głowic atencji, d_model=64, dropout 0.2, a także kodowanie pozycyjne. Parametry treningu były analogiczne do LSTM.



## Modele AI: LSTM i Transformer

Oba modele trenowano na danych jednego okrętu (MMSI: 255806463) z uwagi na ograniczone zasoby obliczeniowe. Wyniki predykcji zostały zwizualizowane na wykresach trajektorii oraz mapach, a także ocenione poprzez analizę odległości między przewidywanymi a rzeczywistymi pozycjami.

- LSTM: Model osiągnął średnie odchylenie predykcji na poziomie około 300 metrów, co jest obiecującym wynikiem, szczególnie dla krótkich sekwencji. Wykresy trajektorii pokazały, że model dobrze odwzorowuje ruch okrętu, choć w niektórych przypadkach odchylenia były zauważalne.
- Transformer: Model ten lepiej radził sobie z modelowaniem złożonych zależności w dłuższych sekwencjach, jednak średnie odchylenie było nieco większe niż w przypadku LSTM, co może wynikać z mniejszej ilości danych treningowych.

## Wnioski

Proces przewidywania trajektorii okrętów wymaga starannego przygotowania danych i odpowiedniego doboru modelu. Kluczowe etapy to:

- Inżynieria cech: Obliczenie SOG, COG i składowych prędkości oraz transformacja współrzędnych na układ kartezjański.
- Segmentacja i skalowanie: Podział trajektorii na krótsze segmenty i normalizacja danych.
- Modelowanie: LSTM sprawdziło się lepiej w przypadku krótkich sekwencji i mniejszych zasobów obliczeniowych, podczas gdy Transformer wykazał potencjał w bardziej złożonych scenariuszach, choć wymaga większej ilości danych.

Średnie odchylenie predykcji na poziomie 300 metrów (dla LSTM) jest obiecującym wynikiem, choć jego znaczenie zależy od kontekstu – w ochronie infrastruktury morskiej może to być wystarczające, ale w bardziej precyzyjnych zastosowaniach wymaga dalszej poprawy.


## Co dalej?

Przyszłe badania mogą skupić się na:
 - Zwiększeniu ilości danych treningowych, obejmujących więcej okrętów i dłuższe okresy.
 - Testowaniu bardziej zaawansowanych architektur, np. hybrydowych modeli łączących LSTM i Transformery.
 - Wdrożeniu modeli w systemach czasu rzeczywistego do wczesnego ostrzegania.

Sztuczna inteligencja otwiera nowe możliwości w zarządzaniu ruchem morskim. Dzięki danym AIS i zaawansowanym modelom, takim jak LSTM czy Transformer, możemy nie tylko monitorować, ale i przewidywać ruch okrętów, zwiększając bezpieczeństwo i efektywność na morzach.


## Link do projektu

Poniżej znajduje się repozytorium związane z zagadnieniem o którym była mowa.
-  [Link do repozytorium](https://github.com/BobakMariusz/ais-trajectory-prediciton)
-  [Plik PDF z wykresami](https://github.com/BobakMariusz/ais-trajectory-prediciton/blob/main/0004-ais-prediction-r%26d-v1.pdf)

# RobotyMobilne
Tytuł projektu: Filtr estymujący orientację robota
Katedra Systemów Automatyki

Cel projektu.
Zaimplementowano 3 filtry:
filtr Kalmana,
filtr Komplementarny,
filtr Madgwicka.
W projekcie należało dokonać filtracji danych wejściowych oraz zwizualizować implementacje filtrów. Jako próbki wejściowe wykorzystane zostały dane z sugerowanego źródła danych: https://github.com/SMAC-Group/imudata (navchip). Są to wyniki 4 godzinnych pomiarów  z nieruchomego żyroskopu i akcelerometru.

Implementacji dokonano w środowisku Matlab. Kody skryptów wraz z plikiem navchip.csv znajdują się w pliku o nazwie ‘FiltryOrientacji.zip’.

Implementacja filtru Kalmana.
Implementując filtr Kalmana należało wyznaczyć estymatę wektora stanu modelu układu dynamicznego. Filtr ten można zastosować do układów liniowych. Podczas implementacji założono, że błąd procesowy i pomiarowy mają charakter gaussowski. Filtr Kalmana przypomina układ obserwatora stanu, opisuje się go równaniami:

x(t+1) = Ax(t) + Bu(t) + V(t),
y(t) = Cx(t) + W(t).
Gdzie:  A - macierz przejścia, B - macierz wejścia, C - macierz wyjścia, V(t) - wektor szumu procesowego, W(t) - wektor szumu pomiarowego, x(t) - wektor stanu, y(t) - wektor wyjścia.
Kroki implementacji:
Wyznaczenie stanu bez wpływu pomiarów z akcelerometru.
Wyznaczenie macierzy kowariancji bez wpływu pomiarów z akcelerometru.
Wyznaczenie macierzy pomocniczej oraz macierzy wzmocnień Kalmana.
Właściwe wyznaczenie stanu, przy wykorzystaniu pomiarów z akcelerometru. 
Właściwe wyznaczenie macierzy kowariancji, przy wykorzystaniu pomiarów z akcelerometru.
 
3. Implementacja filtru komplementarnego.
Realizacja filtru komplementarnego sprowadza się do fuzji danych z obu czujników z wykorzystaniem odpowiednich wzmocnień.
Kroki implementacji:
Położenie wyznaczone na podstawie pomiarów z żyroskopu (scałkowane pomiary z żyroskopu) zostaje wzmocnione (pomnożone) o współczynnik ‘K’, gdzie ‘K’ Є <0; 1> (w praktyce jest bliskie 1).
Położenie wyznaczone na podstawie pomiarów z akcelerometru zostaje wzmocnione o współczynnik ‘1 -K’.
Położenie kątowe wyznacza się sumując wartości uzyskane w kroku 1. i 2.
Suma wzmocnień dla obu typów danych jest zawsze równa 1. Scałkowane pomiary z żyroskopu są obarczone błędem - dryfem położenia kątowego, natomiast akcelerometr jest bardzo podatny na zakłócenia wynikające z ruchu i wibracji obiektu. Stąd wielokrotnie niższa wartość wzmocnienia danych z akcelerometru niż z żyroskopu.
 
Implementacja filtru Madgwicka.
Filtracja danych z czujników w przypadku tego filtru odbywa się poprzez odpowiednie skalowanie danych, czyli zastosowanie regulatora proporcjonalnego oraz wykorzystanie elementu całkującego.
Kroki implementacji:
Chwilowa wartość położenia kątowego jest sumą estymaty kąta z poprzedniej pętli oraz scałkowanego pomiaru z żyroskopu.
Błąd konta ‘e’ to różnica pomiaru z akcelerometru i estymaty kąta z poprzedniej pętli.
Bieżąca wartość położenia kątowego wyznaczana jest jako chwilowa wartość położenia kątowego oraz wzmocniona i scałkowana  wartość błędu ‘e’.
 
5. Wyniki implementacji.
Po zastosowaniu 3 wyżej wymienionych filtrów na próbkach z akcelerometru i żyroskopu otrzymano położenie obiektu pozbawione większych szumów, zakłóceń i dryfu pomiarowego. Każdy filtr pokazał, że fuzja danych z akcelerometru i żyroskopu daje dokładniejsze położenie niż wykorzystanie pojedynczego czujnika. Wiedząc, że zebrane dane dotyczą nieruchomego obiektu, widzimy, że zaimplementowane filtry poprawnie estymują położenie, choć mają różny czas ustalania i różną efektywność tłumienia zakłóceń. Po zastosowaniu filtrów (inaczej niż przy obserwacji danych z samego żyroskopu) widzimy, że obiekt nie porusza się, a ma stałą orientację w przestrzeni kątowej.
Wnioski na podstawie testów:
Najlepsze wyniki dał filtr Madgwicka, choć cechuje go wysokie przeregulowanie, ma jednak szybki czas ustalania, a przebieg jego wyjścia jest wolnozmienny, przez co skutecznie eliminuje szumy i zakłócenia.
Najkrótszy czas ustalania miał filtr komplementarny, jednak ma bardzo niską w porównaniu do filtru Madgwicka odporność na zakłócenia wprowadzane przez pomiar z akcelerometru.
Filtr Kalmana mógłby działać lepiej przy zmienionej macierzy kowariancji, gdyż obecna macierz została wyznaczona doświadczalnie.
 
Podział prac:
Irena Połumackanycz - implementacja filtrów w środowisku MATLAB: Madgwicka, komplementarny.
Karolina Kogut - implementacja filtrów w środowisku MATLAB: Kalmana. Wykonanie  raportu z projektu.

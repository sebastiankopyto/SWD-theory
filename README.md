# Statystyczne metody wspomagania decyzji - wiedza przed 1 kolokwium

## Podstawy R

### Typy danych: 
* podstawowe
  * atomowe (wektory + NULL)
  * lista i inne
* złożone
  * czynniki, ramka danych


### Wektory logiczne

To są już wektory logiczne: 
> TRUE<br>
> FALSE

To nie jest wektor logiczny:
> true 

ponieważ R rozróżnia wielkość znaków.
<br><br>


### Funkcje 

**Funkcja do tworzenia wektorów:**

> c(TRUE, FALSE, TRUE)
---
**Funkcja do sprawdzania długości wektora**

> length(c(TRUE, FALSE, TRUE))
---
**Funkcja replikacji**

> rep(TRUE, 5)<br>
> rep(c(TRUE, FALSE), 2)
---
**Dokumentacja**

> ?rep

---
**Duplikacja każdego**

>rep(c(TRUE, FALSE), each=2)

efekt:

>TRUE, TRUE, FALSE, FALSE

---
<br>

### Wektory liczbowe 

> c(2, 3.8, -5)


> 3:10 = 3, 4, 5, 6, 7, 8, 9, 10

> 2.4:6 = 2.4, 3.4, 4.4, 5.4

Powtarzanie co określony interwał

> seq(0,100,20) = 0, 20, 40, 60, 80, 100

---

<br>

## Wektor napisów

> "ania"
> <br>
> 'ania'

<br>

> c('a', 'b')

> rep('nnn', 5)

---
<br><br>


## Funkcja sprawdzająca typ

>typeof('ania') = "character"

>typeof(rep(c(TRUE, FALSE), 2)) = "logical"

>typeof(c(2, 3.8, -5)) = "double"

>typeof(3:9) = "integer"

>mode(c(2, 3.8, -5)) = "numeric"

>mode(3:9) = "numeric"


---

<br><br>

## Zadania zestaw 1 - statystyka opisowa

### Zadanie 1

Wczytuje liczbę kont zarejestrowanych w ciągu kolejnych 10 dni do zmiennej "x" jako wektor liczbowy wyników: 

> x <- c(43, 37, 50, 51, 58, 105, 52, 45, 45, 10)

Sprawdzamy liczbę elementów w wektorze

>length(x)

#### a) 

Wyciągamy wartości podstawowe dla wektora<br>
Wartość minimalna, 1 kwartyl, mediana, średnia, 3 kwartyl, wartość maksymalna
> summary(x)

Obliczamy odchylenie standardowe: 
> sd(x)

**Uwaga na sposób liczenia kwartyli**

Możemy posortować otrzymane wartości:

> sort(x)

Wychodzi wynik: 
> 10, 37, 43, 45, 45, 50, 51, 52, 58, 105

Mediana to 45 + 50 / 2 = 47.50

Natomiast kwartyl - dzielimy wektory na dwie połówki.<br>
Kwartyl 1/4 to mediana pierwszej połówki (43) a wyszło 43.5<br>
Kwartyl 3/4 to mediana drugiej połówki (52) a wyszło 51.75<br>
<br>

R domyślnie używa funkcji
> ?quantile

i okazuje się że jest 9 algorytmów do liczenia kwantyli. R używa typu 7, natomiast na RPS używaliśmy 1 lub 2.

> quantile(x, 0.25, type=2)
> <br>
> quantile(x, 0.75, type2)


#### b) 

Obserwacje odstające - to te, które wystają poza skrzynkami.

Wykres skrzynkowy:
> boxplot(x)

Aby wyłuskać obserwacje wystające należy obliczyć IQR
> IQR(x)

Czy są obserwacje o wartościach mniejszych niż..<br>
43.50 - 1.5*8.25 czyli Q1 - 1.5 * IQR = 31.125

> x[x<31.125] - wektor logiczny - sprawdza po kolei każdą wartość czy spełnia warunek<br>
> x[1]<br>
> x[0]<br>
> x[0:1]<br>

Czy są obserwacje o wartościach większych niż...<br>
51.75 + 1.5 * 8.25 czyli Q3 + 1.5 * IQR = 64.125<br>

> x[x>64.125]

Odp: Tak, są obserwacje odstające: 10, 105

#### c) 

Usuwanie wartości odstających  i obliczanie ponownie średniej, mediany etc.

Zapisuje prawidłowe wartości do nowego wektora:
> y <- [x>=31.125 & x<=64.125]

wyświetlam nowy wektor
> y

Sprawdzam statystyki i porównuje dla starego i nowego wektora
> summary(x)<br>
> summary(y)

Obliczam odchylenie standardowe

>sd(x)<br>
>sd(y)

### Zadanie 2
*W pliku samochody.csv (UBI) zamieszczono dane dotyczące parametrów samochodów kilku wybranych marek.*

Sprawdzam ścieżkę do katalogu roboczego
> getwd()

Ewentualnie zmiana ścieżki to
> setwd()

#### a)
*Wczytaj dane z pliku do ramki danych – funkcja read.csv2(). Podaj rozmiar ramki danych (liczba obserwacji i liczba zmiennych) – funkcja dim().*

Wczytuje dane do obiektu auta
> auta <- read.csv2('samochody.csv')

Sprawdzam wymiar danych
> dim(auta)

155 11

155 - liczba obserwacji<br>
11 - liczba zmiennych

<br>
Wyświetlenie danych w postaci tabeli

> View(auta)

#### b)

Podsumowanie wartości
>summary(auta)

Funkcja, która na każdej kolumnie (dla każdej zmiennej) wywołuje daną funkcję. Sprawdzamy typ każdej kolumny na naszej ramce danych.

>sapply(auta, typeof)

Aby wyświetlić wartości konkretnej kolumny należy użyć polecenia:

>auta$producent

Zmiana typu producenta na factor

> auta$producent <- factor(auta$producent)

Sprawdzamy dostępne poziomy czynnika

>levels(auta$producent)

Sprawdzamy, czy kolumna jest czynnikiem (factor)

> is.factor(auta$producent)

Faktorom można nazwać różne wartości

> levels(auta$producent) <- c('A', 'E', 'J')

powoduje to podmianę wartości dla faktoru

#### c) 
*Usuń braki danych w utworzonej ramce danych – funkcja na.omit().*

Aby usunąć samochody, które na którejkolwiek wartości mają brak danych używamy funkcji

> auta <- na.omit(auta)


#### d) 
*Zmienna mpg opisuje zużycie paliwa w liczbie mil przejechanych na 1 galonie. Utwórz zmienną zp opisującą zużycie paliwa mierzone w litrach na 100 kilometrów.*

*Wskazówka: <br>
1 mila = 1609 m <br>
1 galon (amerykański) = 3,785 l*

Przypisuje do zmiennej zp w auta nową wartość

> auta$zp <- (100 * 3.785) / (auta$mpg * 1.609) 

#### e) 

*Utwórz histogram dla zmiennej zp – funkcja hist(). Jak zmienia się kształt histogramu przy różnych liczbach klas (parametr breaks)?*

Tworzymy histogram za pomocą funkcji: 
> hist(auta$zp, col=3)

Histogram pocięty na 5 klas

> hist(auta$zp, breaks=5)

Histogram pocięty na 20 klas

> hist(auta$zp, breaks=20)

Dla lepszej prezentacji histogramów można użyć polecenia
> par(mfrow=c(2,2))

które tworzy okno 2x2, aby zmieścić 4 histogramy na 1 ekranie.

Histogramy można konfigurować co do widoczności danych, zakresów etc.


#### f) 

*Utwórz wykres łodygowo-liściowy – funkcja stem().*

Aby utworzyć wykres łodygowo-liściowy należy użyć polecenia
>stem(auta$zp)
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

Aby sprawdzić ile jest braków w wektorze należy użyć funkcji

> is.na(c(4,NA,6.7))

Suma braków:

> sum(is.na(c(4,NA,6.7)))


Sprawdzenie liczby braków na całym zbiorze danych

> sapply(auta, function(n) sum(is.na(n)))


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
> hist(auta$zp, col=3, labels = TRUE)

col = kolor

Histogram pocięty na 5 klas

> hist(auta$zp, breaks=5)

Histogram pocięty na 20 klas

> hist(auta$zp, breaks=20)

Dla lepszej prezentacji histogramów można użyć polecenia
> par(mfrow=c(2,2))

które tworzy okno 2x2, aby zmieścić 4 histogramy na 1 ekranie.

Histogramy można konfigurować co do widoczności danych, zakresów etc.


<br>

Histogram można przypisać do zmmiennej

> H <- hist(auta$zp, col=3, labels = TRUE)<br>
> length(H$counts) - liczba klas

<br>
Moda = najczęściej występujące wartości, maksima lokalne

Jeśli istnieją 2 maksima lokalne (np. między 6 a 7 oraz między 10 a 11) wykres nazywamy dwumodalnym.

Określenia: prawostronnie skośny, lewostronnie skośny.


#### f) 

*Utwórz wykres łodygowo-liściowy – funkcja stem().*

Aby utworzyć wykres łodygowo-liściowy należy użyć polecenia
>stem(auta$zp)

Wykres łodygowo-liściowy

   5 | 03334578<br>
   6 | 0002222223344455555555677888999999<br>
   7 | 0001222333334444445566667788999<br>
   8 | 0123444446666677778889<br>
   9 | 0133447788899<br>
  10 | 01255799<br>
  11 | 1344556666689<br>
  12 | 113333679<br>
  13 | 003444889<br>
  14 | 35<br>
  15 | 2<br>

Po lewej stronie część całości, po prawej liczba po przecinku (zaokrąglona do 1 miejsca)



#### g)
*Oblicz i zinterpretuj podstawowe statystyki próbkowe dla danych opisujących zużycie paliwa (takie jak: średnia, 
mediana, kwartyle, 10. i 90. percentyl, wartości ekstremalne, wariancja, odchylenie standardowe, rozstęp, rozstęp międzykwartylowy, współczynnik asymetrii, kurtoza, współczynnik zmienności) – np. funkcje mean(), median(), 
quantile(), min(), max(), range(), var(), sd(), IQR(), skewness(), kurtosis().*


>mean(auta$zp) #średnia, <br>
>median(auta$zp) #mediana, <br>
>quantile(auta$zp) #kwartyle, <br>
>quantile(auta$zp, c(0.1, 0.9)) #10. i 90. percentyl, <br>
>min(auta$zp); max(auta$zp) #wartości ekstremalne, <br>
>range(auta$zp) <br>
>var(auta$zp) #wariancja, <br>
>sd(auta$zp) #odchylenie standardowe, <br>
>diff(range(auta$zp)) #rozstęp, <br>
>IQR(auta$zp) #rozstęp międzykwartylowy, <br>

>install.packages('e1071') - instalacja pakietu<br>
>library(e1071) - ładowanie pakietu - za każdym uruchomieniem<br>
>skewness(auta $zp) - współczynnik asymetrii (przyjmuje wartości pomiędzy -1 a 1 NAJCZĘŚCIEJ, jeśli 0 to wykres jest symetryczny, jeśli bliżej 1 lub -1 to wykres skośny, jeśli + to prawostronny, jeśli - to lewostronny), <br>
>kurtosis(auta $zp) - kurtoza, <br>

>mean(auta$zp) / sd(auta $zp) współczynnik zmienności - zróżnicowanie próby<br>


#### h)
*Utwórz wykres skrzynkowy (ramkowy, pudełkowy) dla zmiennej opisującej zużycie paliwa – funkcja boxplot().*

Tworzenie wykresu skrzynkowego

> boxplot(auta$zp)

**Znaczenia:** <br>
gruba kreska w środku skrzynki - mediana<br>
Początek skrzynki na dole - kwartyl 1<br>
Koniec skrzynki na górze - kwartyl 3<br>
"Wąsy" czyli końce poza boksem - minimum i maksimum<br>
W lewym wąsie znajduje się 25% obserwacji, w prawym - 25% obserwacji<br>

<br>

Wykres poziomy:
>boxplot(auta$zp, horizontal = TRUE, col="blue") 

Naniesienie punktów na wykres
>points(auta$zp, rnorm(150, 1, 0.02))

Generuje 150 punktów wokół 1 z rozrzutem 0.02 (odchylenie standardowe) z rozkładu normalnego

>rnorm(150, 1, 0.02)


#### Interpretacja

* Średnie zużycie paliwa wynosi 8,8 l/100km z odchyleniem standardowym 2,43 l/100km. 
* Najmniejsze zużycie to 5l/100km, a największe 15.2l/100km. 
* Dla co najmniej 50% aut zużycie paliwa jest nie większe niż 8.1 l/100km i jednocześnie dla co najmniej 50% 
aut zużycie paliwa jest nie mniejsze niż 8.1 l/100km. 
* Dla co najmniej 25% samochodów zużycie wyniosło nie więcej niż 6,9 l/100km (Q1) i jednocześnie dla co 
najmniej 75% aut zużycie paliwa jest nie mniejsze niż 6,9 l/100km. 
* Dla co najmniej 75% aut zużycie nie przekracza 10,4 i/100km (Q3) i jednocześnie dla co najmniej 25% aut 
zużycie paliwa jest nie mniejsze niż 10,4 l/100km. 
* Dla co najmniej 10% aut spalanie jest nie mniejsze niż 12.3 l/100km (90.percentyl). 
* Rozkład zużycia paliwa jest rozkładem prawostronnie skośnym – co jest widoczne na histogramie i wykresie skrzynkowym (wolniej opadające prawe ramię, dłuższy prawy wąs, mediana przesunięta w lewo) oraz dodatnia wartość skośności. 

Ad. średnia
Średnia i jej połączenie z odchyleniem (czyli średnie zużycie paliwa wynosi 8,8 +/- 2,43l) jest prawdziwe gdy wykres jest symetryczny lub lekko skośny. Natomiast jeśli rozkład jest skośny już nie do końca. Jeśli rozkład jest silnie skośny, lepiej jest liczyć medianę (zamiast średniej) oraz IQR (zamiast odchylenia).

Ad. Q2, Q3, Q1, Q0,90 (3,4,5,6) 


### Zadanie 3

#### a) 

*Utworzenie zmiennej zp_kat opisującej kategorię zużycia paliwa*

>auta$zp_kat[auta $zp <=7 ] <- 'mało

>auta$zp_kat[auta $zp > 7 && auta $zp <= 10 ] <- 'mało

>auta$zp_kat[auta $zp > 10 ] <- 'dużo

#### b) 

*Zliczenie kategorii (poziomów) czynnika zp_kat*

> table(auta$zp_kat)

Jeśli chcemy nadać inny porządek kategoriom czynnika, można wykonać polecenie

>auta$zp_kat <- factor(auta $zp_kat, ordered=TRUE, levels=c('mało', 'średnio', 'dużo'))

>table(auta$zp_kat)


Obliczenie udziałów procentrowych poszczególnych kategorii czynnika zp_kat

>prop.table(table(auta$zp_kat)) - odsetki<br>
>prop.table(table(auta$zp_kat))*100 - procenty

Wykres słupkowy

>barplot(table(auta$zp_kat), col=5:7, ylab='liczba aut', xlab = 'kategoria spalania')

Wykres kołowy

>pie(table(auta$zp_kat), col=5:7)


### Zadanie 4

Funkcja tapply(), dla której pierwszym argumentem jest wektor liczbowy, drugim - wektor lub czynnik określający 
grupy, trzecim funkcja, która zostanie wyznaczona na wektorze liczbowym względem grup, np. średnia i odchylenie 
standardowe zużycia paliwa względem producenta:

> tapply(auta$zp, auta$producent, mean) # 9.847964 7.933492 7.214859
> 
> tapply(auta$zp, auta$producent, sd) # 2.352059 2.564224 1.259573 
> 
> boxplot(auta$zp~auta$producent, col=2:4) 

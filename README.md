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


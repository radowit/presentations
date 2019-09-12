%title: Patologicznie nadużywane antywzorce
%author: Radosław Ganczarek
%date: 2019-09-11

-> # Patologicznie nadużywane antywzorce <-

-> ## porady dla potłuczonych <-

-> * PyCon 2019, Zawiercie * <-

-------------------------------------------------

-> # O mnie <-

-> ## Powtórka z rozrywki <-

<br>
-> *...kolejny rant na złe praktyki w kodowaniu* <-

-------------------------------------------------

-> # Rozterka <-

-> ## Czy wszystko jest złe? <-

<br>
Programowanie jest trudne
=========================
<br>
* tyle właściwości języka
<br>
* tyle różnych projektów
<br>
* tyle sposobów użycia tych samych narzędzi
<br>

-> *☠ niektóre z nich są błędne ☠* <-

-------------------------------------------------

-> # Dobre praktyki <-

## Pomysły na rozwiązanie problemów:
* wzorce projektowe
* czysta architektura
* SOLID
* CQS
* pylint

<br>
-> * Nikt nie zwalnia cię od myślenia * <-

-------------------------------------------------

-> # Zasady <-

* Żadne narzędzie nie jest złe

* Ślepe podąrzanie za metodykami i zasadami
  nie zawsze jest dobre

* W razie wątpliwości pytaj starszych (stażem)
  kolegów lub StackOverflow

* Myśl

-------------------------------------------------

-> # Antywzorzec #1 <-

-> ## At the Mixins of Madness <-

<br>
-> dziedziczenie vs agregacja <-

-------------------------------------------------

-> # Dziedziczenie <-

-> ## Dobry przykład - wzorzec budowniczego <-

```
class DateInputField(MultiFieldMixin, Field):
    ...

```

-------------------------------------------------

-> # Dziedziczenie <-

-> ## Zły przykład <-

```
class FormAPIView(EmailReportMixin, APIView):
    ...

```

-------------------------------------------------

-> # Agregacja <-

-> ## Dobry przykład <-

```
class FormAPIView(APIView):
    def __init__(self, report_sender):
        self._report_sender = report_sender

```

-------------------------------------------------

-> # Antywzorzec #1 <-

Występowanie:
=============
* Django (esp. DRF, Tastypie)
* duże systemy robione bez wcześniejszego
  przemyślenia sprawy
* Kod maniaków programowania obiektowego
  (byłem jednym z nich!)

-------------------------------------------------

-> # Antywzorzec #2 <-

-> ## Call of Decorators <-

<br>
-> czy one są nam potrzebne? <-

-------------------------------------------------

-> # Dobre dekoratory <-

Przykłady:
==========
* rejestr
* cache
* rozszerzenia języka (np. context manager,
  singledispatch)
* serializacja/opakowywanie wyniku
* za małe na własną klasę, za ogólne na metodę

-------------------------------------------------

-> # Złe dekoratory <-

Przykłady:
==========
* dodawanie funkcjonalności (użyj agregacji
  albo mixiny)
* zamiast refaktoringu
* zmienianie pól i metod klasy (użyj metaklasy)
* dodawanie getterów i setterów
  (użyj deskryptorów)
* wyciągnięcie wspólnego kodu z kilku funkcji
  (użyj klas i dziedziczenia albo po prostu
  wywołaj funkcję ze wspólną częścią kodu)
* zamiast context managera

-------------------------------------------------

-> # Antywzorzec #3 <-

-> ## The Hardcode horror <-

<br>
Trzymanie dużych stringów w kodzie:
===================================
* nieczytelność
* trudność w aktualizacji
* zmiany w stringach mieszają się ze zmianami
  w kodzie
* długie linie, które trudno złamać
* utrudniają orientację w terenie
* łatwo przegapić
* trudniej testować

-------------------------------------------------

-> # Zamiast hardkodowania stringów <-

-> *Jeśli boisz się, że ładowanie stringów* <-
-> *spowolni twój kod, to rób to w globalnym scopie* <-
-> *jakiegoś modułu, który jest odpalany na początku* <-
-> *działania programu* <-

<br>
Sposoby:
========
* serializacja: json, yaml, xml
* pliki z danymi: csv
* pliki konfiguracyjne: ini
* pliki plaintextowe: txt
* arkusze kalkulacyjne: xlsx, ods

-------------------------------------------------

-> # Antywzorzec #4 <-

-> ## The Lurking Comprehensions   <-

<br>
```
ps = []
[
    ps.append(x) or x for x in range(2,100)
    if not any([y for y in ps if x%y==0 and x != y])
]
ps = ?

print(''.join([
    '\n' if y==9 else str(
        x if y == 0 else y if x == 0 else x*y
    ).rjust(3) for x in range(10) for y in range(10)
])) 
```

-------------------------------------------------

-> # List comprehensions <-

-> Stosować zamiast map i filter <-

-> Nie stosować zamiast logiki aplikacji <-
===========================================

-> *Unikać skutków ubocznych w środku* <-
-> (od tego jest reduce!) <-

-------------------------------------------------

-> # Antywzorzec #5 <-

-> ## The Methods Out of Space <-

<br>
-> nadużywanie *classmethod* i *staticmethod* <-

-> "żeby się nie czepiał pylint" <-

-------------------------------------------------

-> # Metody statyczne i metody klas <-

Mogą pomóc w:
=============
* tworzeniu przestrzeni nazw
* tworzeniu fabryk
* dostęp do stałych w klasie

<br>
-> *Nie służą do zawłaszczania przez klasy* <-
-> *kodu, który powinien leżeć w innej klasie* <-
-> *albo funkcji* <-

-------------------------------------------------

-> # Antywzorzec #6 <-

-> ## The Whisperer in Django <-

<br>
-> *Django* to framework warstwy prezentacji <-

-> Nie może mówić Ci jak masz żyć! <-
===================================

-------------------------------------------------

-> # "Django way" <-

Problemy:
=========
* brak miejsca na logikę
* pomieszanie wszystkich warstw

<br>
-> Lekarstwo: *hexagonal architecture* <-

-------------------------------------------------

-> # Wzorzec #1 <-

-> ## Koniec rantu? <-

<br>
-> *Context manager rządzi!* <-

-------------------------------------------------

-> # Context manager <-

Co robi:
========
- pliki otwiera
- wyjątki łapie
- ma swoją składnię (with)
- można go stworzyć używając klasy lub funkcji
- buty wiąże
- robi kawę
- ...

-------------------------------------------------

-> # Ten slajd gdy wszyscy robią zdjęcie <-

<br>
* https://docs.quantifiedcode.com/python-anti-patterns/
* https://www.pylint.org/
* https://www.youtube.com/watch?v=Er5K_nR5lDQ
* https://www.youtube.com/watch?v=wf-BqAjZb8M

* https://www.slideshare.net/mvschaik/hexagonal
* https://skillsmatter.com/skillscasts/5280-hexagonal-microservices

-------------------------------------------------

-> # Moi mili, dziękuję za uwagę <-

-> ## Pytanka? <-



-> Radosław Ganczarek <-
-> *PGS Software* <-

-------------------------------------------------
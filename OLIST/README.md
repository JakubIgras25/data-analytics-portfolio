![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)


# Olist E-Commerce — analiza sprzedaży i logistyki w SQL

Projekt poświęcony analizie sprzedaży, logistyki i satysfakcji klientów prawdziwego brazylijskiego marketplace'u Olist. Dane obejmują zamówienia, ich pozycje, płatności, recenzje, produkty i sprzedawców — osiem powiązanych tabel, około pół miliona wierszy łącznie, w okresie od września 2016 do października 2018.

<img width="1300" height="912" alt="image" src="https://github.com/user-attachments/assets/d829d0ff-127d-4e7f-af20-e74ebde65cce" />


---

## O czym jest ten projekt

**Ile zamówień naprawdę dociera po terminie i dlaczego zależy to od tego, jak zada się to pytanie.** Zbiór zawiera pole z obiecanym terminem dostawy, zapisane zawsze jako północ danego dnia, oraz pole z rzeczywistą datą i godziną doręczenia. Proste porównanie tych dwóch znaczników czasu liczyło jako spóźnienie każdą paczkę doręczoną w obiecanym dniu, tylko po godzinie zero, co zawyżało wskaźnik o blisko tysiąc trzysta zamówień. Po korekcie definicji odsetek spóźnień spadł z ośmiu do niespełna siedmiu procent, a rozbicie na stany pokazuje ponad trzykrotną różnicę między najszybszym a najwolniejszym regionem kraju.

**Skąd bierze się przychód i czy dwie różne jego definicje dają tę samą liczbę.** Przychód liczony z faktycznie zaksięgowanych płatności i przychód liczony jako suma cen pozycji zamówienia w większości przypadków się pokrywają, ale nie zawsze — dla trzystu kilku zamówień różnią się nawet o sto kilkadziesiąt złotych. Raport opiera się na kwocie faktycznie zapłaconej i pokazuje ją w podziale na miesiące, kategorie produktów oraz koncentrację wśród sprzedawców.

**Ilu klientów naprawdę wraca, skoro w bazie każde zamówienie dostaje osobne konto.** Odpowiedź zależy od tego, którego z dwóch pól identyfikujących klienta się użyje — jedno opisuje zamówienie, drugie osobę. Po przejściu na właściwe pole okazuje się, że do sklepu wraca nieco ponad trzy procent kupujących.

**Czy spóźniona dostawa realnie psuje opinię o sklepie.** Średnia ocena zamówień dostarczonych na czas i tych spóźnionych różni się o niemal dwie gwiazdki, a zależność powtarza się niezależnie od tego, który miesiąc się sprawdzi.

**Który sprzedawca faktycznie nie dotrzymuje terminów, a nie tylko ma pecha z kurierem.** Rozbicie łącznego czasu dostawy na etap płatności, etap po stronie sprzedawcy i etap logistyki pokazuje, że transport odpowiada za większość opóźnienia. Osobna miara rozlicza sprzedawców wyłącznie z terminu, na który mają realny wpływ, a nie z całej trasy do klienta.

---

## Co było nie tak w danych i jak to naprawiłem

Tabela klientów wygląda na tabelę osób, ale nią nie jest ,każde nowe zamówienie dostaje własny identyfikator klienta, nawet gdy robi je ta sama osoba. Liczenie klientów po tym polu daje dziewięćdziesiąt dziewięć tysięcy, praktycznie tyle samo co zamówień. Dopiero drugie pole w tej samej tabeli pozwala policzyć realną liczbę osób, a różnica między nimi jest tym, co decyduje o poprawności każdej miary dotyczącej retencji.

Drugi problem znalazłem już we własnej pracy, nie w danych źródłowych. Pierwsza wersja miary opóźnionych dostaw porównywała pełny znacznik czasu zamiast samej daty, co opisałem powyżej. Zostawiłem tę pomyłkę w dokumentacji projektu razem z poprawką, bo pokazuje coś ważniejszego niż sam wynik — że wiarygodnie wyglądająca liczba nie musi być poprawna, dopóki nie sprawdzi się jej niezależnie.

Recenzje kryły dwa osobne problemy naraz. Ten sam identyfikator recenzji bywał przypisany do kilku różnych zamówień jednocześnie, zawsze z tą samą oceną i datą, co wskazuje na jedną ankietę rozesłaną po zamówieniu złożonym z kilku transakcji, a nie na błąd generatora danych. Osobno ponad dwieście zamówień miało po dwie recenzje o różnej ocenie, co wymagało decyzji, jak liczyć średnią, zamiast wybierania, która ocena jest tą prawdziwą. Podobny problem remisów pojawił się przy ustalaniu pierwszego zamówienia klienta — prawie trzysta par zamówień tej samej osoby miało identyczny znacznik czasu co do sekundy, więc bez dodatkowej reguły sortowania wynik zależałby od przypadku.

Model celowo nie ma klucza obcego między tabelą produktów a słownikiem tłumaczeń kategorii. Dwie kategorie nie mają w nim odpowiednika, a wymuszenie relacji odrzuciłoby przy imporcie kilkanaście prawdziwie istniejących produktów tylko po to, żeby baza zgodziła się z niekompletnym słownikiem. Z podobnego powodu tabela geolokalizacji, dostarczona jako kilkutysięczna losowa próbka pełnego zbioru, w ogóle nie jest połączona z resztą modelu — każdy wskaźnik policzony na jej podstawie byłby artefaktem próbkowania, nie rzeczywistością.


---

## Pobranie plików

Plik SQL oraz baza danych w formacie CSV przekraczają limit rozmiaru obowiązujący na GitHubie i są dostępne pod linkiem:

**[LINK DO GOOGLE DRIVE](https://drive.google.com/drive/folders/1dGuleoKaT6HKSh_SeplZtS9exWVx9BYQ?usp=sharing)**

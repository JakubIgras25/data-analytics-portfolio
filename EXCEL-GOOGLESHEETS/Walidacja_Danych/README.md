![Excel](https://img.shields.io/badge/-Excel-217346?style=for-the-badge&logo=microsoftexcel&logoColor=white)
![Google Sheets](https://img.shields.io/badge/-Google%20Sheets-34A853?style=for-the-badge&logo=googlesheets&logoColor=white)
![Data Validation](https://img.shields.io/badge/-Data%20Validation-2C2C2C?style=for-the-badge)

# Walidacja i kontrola jakości danych zamówień — Excel

Mini-projekt poświęcony kontroli jakości danych w arkuszu zamówień sprzedażowych. Na wejściu — tabela z celowo wprowadzonymi błędami: nieprawidłowym formatem ID, datą sprzed dopuszczalnego zakresu, zerowymi i ujemnymi ilościami, rabatem przekraczającym limit. Efektem jest zestaw siedmiu reguł Data Validation z niestandardowymi komunikatami, wsparty formatowaniem warunkowym i osobnym arkuszem słownikowym dla list rozwijanych.


---

## Co obejmuje projekt

Skoroszyt ma trzy arkusze: `complete data` (dane źródłowe do audytu), `input` (formularz wejściowy z pełnym zestawem reguł) i `rules` (słownik dozwolonych wartości, z którego korzystają listy rozwijane).

**Jak wymusić spójne wartości w polach Region i Product_Category?** Obie kolumny mają listy rozwijane zasilane z osobnego arkusza `rules` (a nie wpisane na sztywno w regule) — Region: NA / EMEA / APAC / LATAM, Product_Category: Electronics / Home Appliances / Accessories. Każda ma własny komunikat wejściowy i alert błędu, tłumaczący użytkownikowi, co dokładnie wybrać.

**Jak nie dopuścić do niepoprawnych ilości, cen i rabatów?** Quantity przyjmuje wyłącznie liczby całkowite większe od 0, Unit_Price — dowolną liczbę większą od 0, Discount — wartość z przedziału 0–0,5 (0–50%). Wszystkie trzy pola mają też formatowanie warunkowe w arkuszu `complete data`, które wizualnie wyróżnia naruszenia.

**Jak ograniczyć Order_Date do sensownego zakresu?** Reguła dopuszcza wyłącznie daty od 01.01.2023 do dzisiaj (`TODAY()`), więc zarówno stare, jak i "przyszłe" wpisy są odrzucane od razu przy wprowadzaniu.

**Jak zweryfikować format Order_ID bez regexa?** To była najtrudniejsza reguła. Excel nie ma natywnego dopasowywania wzorców, więc format `oYYYY-NNNN` sprawdza złożona formuła oparta na funkcjach tekstowych: `LEWY` sprawdza literę "o", `FRAGMENT.TEKSTU` i `PRAWY` wycinają rok i cyfry zamówienia, `ISNUMBER(VALUE(...))` potwierdza, że wycięte fragmenty są rzeczywiście liczbami, a `DŁ` pilnuje całkowitej długości ciągu — wszystko spięte jednym `ORAZ`.

**Jak przetestować, czy reguły faktycznie łapią błędy?** Te same reguły (lista dla Region, formuły dla pozostałych pól) zostały nałożone też na arkusz `complete data`, gdzie funkcja Poprawność danych → Zakreśl nieprawidłowe dane pozwoliła wizualnie namierzyć rekordy niezgodne ze słownikiem i regułami, zanim trafiły do formularza `input`.

---

## Błędy znalezione w danych źródłowych

Na 20 zamówieniach w arkuszu `complete data` reguły wyłapały sześć naruszeń:
- **Order_ID** w wierszu 1011 zapisane jako liczba (`1011`) zamiast tekstu w formacie `oYYYY-NNNN`.
- **Order_Date** w wierszu 1002 — `2022-12-05`, sprzed dolnej granicy 01.01.2023.
- **Quantity** — `0` (wiersz 1003), `-2` (wiersz 1010) i puste pole (wiersz 1016).
- **Discount** — `0,6` w wierszu 1005, czyli 60%, powyżej dopuszczalnego limitu 50%.

## Pobranie plików

**[LINK DO GOOGLE DRIVE]([https://docs.google.com/spreadsheets/d/1ipdUBLgYvnuIltiRh5bFiTcfxXJ_fdae/edit?usp=sharing&ouid=110939012766072819563&rtpof=true&sd=true](https://drive.google.com/drive/folders/1gxUw-E0l0mgaQPg01_CV7kaIZ2LCVyG9?usp=drive_link))**

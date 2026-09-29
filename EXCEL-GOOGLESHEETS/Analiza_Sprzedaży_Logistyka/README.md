![Excel](https://img.shields.io/badge/-Excel-217346?style=for-the-badge&logo=microsoftexcel&logoColor=white)
![Google Sheets](https://img.shields.io/badge/-Google%20Sheets-34A853?style=for-the-badge&logo=googlesheets&logoColor=white)
![Pivot Tables](https://img.shields.io/badge/-Pivot%20Tables-2C2C2C?style=for-the-badge)

# Analiza efektywności sprzedaży i logistyki — Excel / Arkusze Google

Projekt poświęcony analizie efektywności sprzedaży i logistyki na podstawie 1330 zamówień z 46 krajów. Dane obejmują pełny cykl transakcji — kanał sprzedaży, priorytet zamówienia, koszty i przychody, czas wysyłki — rozłożone na osiem arkuszy: tabelę źródłową, siedem tabel przestawnych i trzynaście wykresów zasilających dashboard.

<img width="1126" height="797" alt="image" src="TU_WKLEJ_LINK_DO_SCREENSHOTA" />
<img width="1126" height="797" alt="image" src="TU_WKLEJ_LINK_DO_SCREENSHOTA" />
<img width="1126" height="797" alt="image" src="TU_WKLEJ_LINK_DO_SCREENSHOTA" />

---

## O czym jest ten raport

Raport odpowiada na pytania, jakie zadałby zarząd firmy handlowej rozważającej optymalizację portfela produktów i logistyki.

**Czy krótszy czas wysyłki oznacza wyższy zysk?** Nie ma bezpośredniej, silnej zależności liniowej między czasem wysyłki a zyskiem jednostkowym — ten zależy głównie od kategorii produktu i marży. Wpływ czasu dostawy ujawnia się dopiero w dłuższym terminie: zamówienia z czasem wysyłki powyżej 35–40 dni wiążą się z wyższymi kosztami operacyjnymi i ryzykiem zwrotów, a to właśnie w regionach o najdłuższej logistyce (Europa Wschodnia) najbardziej "zamraża" kapitał.

**Skąd pochodzi przychód i gdzie firma ryzykuje w logistyce?** Europa — zwłaszcza Środkowa i Wschodnia oraz Bałkany — generuje największą liczbę zamówień i najwyższy przychód całkowity. Paradoksalnie kraje o najwyższych przychodach, jak Bułgaria czy Rosja, mają też ekstremalnie długi czas dostawy (do 50 dni), co zagraża satysfakcji klienta. Jednocześnie duża część krajów (głównie w Afryce i Azji) generuje pojedyncze zamówienia o niskiej marży — rozproszenie, które kosztuje w logistyce więcej, niż zwraca w przychodzie.

**Które kategorie produktów faktycznie zarabiają, a które tylko generują wolumen?** Największy przychód dają Office Supplies (ponad 402 mln USD) i Household (ponad 294 mln USD), ale to Cosmetics — mimo mniejszej liczby sprzedanych jednostek niż np. Beverages — generuje najwyższy zysk, jednostkowy i całkowity (92,7 mln USD). Fruits i Beverages sprzedają się w dużym wolumenie, ale ich wkład w zysk całkowity jest marginalny — sygnał, by kierować marketing na marżę, nie na liczbę zamówień.

**Czy kanał sprzedaży (online vs offline) robi różnicę?** Firma utrzymuje niemal idealnie zrównoważony model: Offline generuje ok. 5% wyższy przychód całkowity niż Online, ale oba kanały są niemal identycznie rentowne — różnica w zysku to zaledwie 1 punkt procentowy.

**Kiedy klienci najchętniej kupują?** Najwięcej zamówień składanych jest w niedzielę (207), poniedziałek (202) i sobotę (201) — klienci wyraźnie preferują weekend i początek tygodnia. Najsłabszy jest czwartek (167).

---

## Struktura danych

Tabela źródłowa (`Data`) zawiera 1330 zamówień i 20 kolumn — dane transakcyjne (ID zamówienia, daty, kanał sprzedaży, przychód, koszt, zysk), geograficzne (kraj, region, subregion, kod ISO) i logistyczne (priorytet zamówienia, czas wysyłki). Na tej podstawie zbudowano siedem tabel przestawnych i trzynaście wykresów rozłożonych na arkuszach: `mini-dashboard` (kluczowe KPI), `ABC` (klasyfikacja kategorii według reguły Pareto — grupy A/B/C wg udziału w zysku), `Geographical Analysis`, `Sales Channels`, `Shipping Analysis` i `Time Trends`.

## Pobranie plików

**[LINK DO GOOGLE DRIVE](https://docs.google.com/spreadsheets/d/1ipdUBLgYvnuIltiRh5bFiTcfxXJ_fdae/edit?usp=sharing&ouid=110939012766072819563&rtpof=true&sd=true)**

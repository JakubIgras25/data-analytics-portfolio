# 📊 Global E-Commerce Performance & Customer Behavior Analysis

## 📌 O projekcie
Kompleksowa analiza danych sprzedażowych, zachowań użytkowników oraz wydajności kanałów marketingowych dla globalnej platformy e-commerce. Analiza obejmuje **349 545 unikalnych sesji** z okresu od **listopada 2020 do stycznia 2021 roku**. Dane zostały pobrane bezpośrednio z chmury **Google BigQuery** za pomocą zaawansowanego zapytania SQL łączącego tabele sesji, zamówień, produktów i kont użytkowników.

Głównym celem projektu była identyfikacja najbardziej dochodowych rynków i kategorii produktów, ocena efektywności urządzeń i kanałów ruchu oraz rygorystyczna weryfikacja hipotez statystycznych dotyczących zachowań zakupowych klientów.

W projekcie przeprowadzono pełną eksploracyjną analizę danych (EDA), zbudowano autorskie wizualizacje w Matplotlib/Seaborn z dedykowanym stylowaniem oraz zrealizowano testy statystyczne: parametryczne (T-Student, ANOVA, korelacja Pearsona) i nieparametryczne (Kruskal-Wallis, Chi-kwadrat, korelacja Spearmana, Shapiro-Wilk, test Levene'a).

## 📊 Podgląd Wykresów

### Kompleksowy Raport Analityczny Sprzedaży e-Commerce
<img width="1214" height="411" alt="image" src="https://github.com/user-attachments/assets/771f6c48-2f40-4579-a1b0-175b27b11492" />
<img width="1216" height="390" alt="image" src="https://github.com/user-attachments/assets/061d7d67-fb94-46a4-9415-4f31bbf005a6" />
<img width="1214" height="378" alt="image" src="https://github.com/user-attachments/assets/b5a37875-b315-44e3-b494-65a9e12ed0da" />
<img width="1038" height="616" alt="image" src="https://github.com/user-attachments/assets/e5dced12-5983-4d32-9424-f98b76ca94f4" />
<img width="458" height="345" alt="image" src="https://github.com/user-attachments/assets/d2018e5d-0545-4ff4-baee-982d0a45d167" />
<img width="458" height="294" alt="image" src="https://github.com/user-attachments/assets/ec4dc033-6cb9-4f44-b171-52797c055370" />
<img width="1008" height="466" alt="image" src="https://github.com/user-attachments/assets/deff392c-b7b7-4ae8-bb64-3a59733577ca" />
<img width="346" height="251" alt="image" src="https://github.com/user-attachments/assets/ecf51f5c-88fa-44b9-86e0-e122b7516848" />






## 💡 Kluczowe wskaźniki i wnioski biznesowe

* **Geograficzna dominacja rynku (Americas & USA):** Najwyższy przychód wygenerował kontynent **Americas (~17.67M USD)**, wyprzedzając Azję (~7.60M USD) i Europę (~5.93M USD). Głównym motorem sprzedaży na poziomie krajów są **Stany Zjednoczone (~13.94M USD)**, za którymi plasują się Indie (~2.81M USD) oraz Kanada (~2.44M USD).
* **Wpływ rejestracji na AOV (Test T-Studenta $p = 0.3033$):** Porównanie zarejestrowanych i niezarejestrowanych użytkowników nie wykazało statystycznie istotnej różnicy w średniej wartości zamówienia ($p > 0.05$). Sugeruje to kierowanie działań w stronę ogólnej personalizacji oferty dla wszystkich klientów, zamiast różnicowania ich pod kątem statusu weryfikacji.
* **Korelacja natężenia ruchu ze sprzedażą (Test Spearmana):** Wykorzystanie nieparametrycznego testu Spearmana potwierdziło silną, dodatnią zależność między dzienną liczbą sesji (`ga_session_id`) a całkowitym dziennym przychodem (`price`).
* **Weryfikacja rozkładu wartości zamówień (Test Shapiro-Wilk):** Test normalności wykazał, że rozkład zmiennej przychodu odbiega od rozkładu normalnego ($p < 0.05$), co uzasadniło zastosowanie testów nieparametrycznych (m.in. Kruskal-Wallis).
* **Różnice w strukturze ruchu (Test Chi-kwadrat):** Analiza tabeli wielodzielczej potwierdziła istotne statystycznie różnice w udziale ruchu organicznego (`Organic Search`) między Europą a Ameryką.

## 🛠️ Wykorzystane technologie i biblioteki

* **Python 3.10+** – środowisko analityczne.
* **Google BigQuery & `pandas_gbq`** – zapytania SQL z relacyjnym łączeniem tabel (`LEFT JOIN` na sesjach, zamówieniach i kontach).
* **Pandas & NumPy** – czyszczenie i transformacja danych, tabele przestawne (`pivot_table`, `crosstab`) oraz agregacje statystyczne.
* **Matplotlib & Seaborn** – budowa autorskich funkcji wizualizacyjnych (`beauty_bar`, `beauty_donut`) z niestandardowym stylowaniem, paletami kolorów i adnotacjami.
* **SciPy (`scipy.stats`)** – zaawansowana weryfikacja hipotez statystycznych:
  * Test T-Studenta (`ttest_ind`) oraz test jednorodności wariancji Levene'a (`levene`).
  * Jednoczynnikowa ANOVA (`f_oneway`) oraz test Kruskala-Wallisa (`kruskal`).
  * Test niezależności Chi-kwadrat (`chi2_contingency`).
  * Test normalności Shapiro-Wilka (`shapiro`).
  * Korelacja rangowa Spearmana (`spearmanr`).

## 📁 Pliki do pobrania
* 🔗 **[Pobierz pliki projektu z Google Drive / GitHub](https://drive.google.com/drive/folders/1WABFGCU9IX_M_V0DkG20TvpP3bi9Nlt5?usp=drive_link)** *(Notatnik Google Colab z kodem Python, zapytaniami SQL oraz wygenerowanym dashboardem)*

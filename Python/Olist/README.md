# 📦 E-Commerce Performance & Logistics Analysis – Olist Brazil
![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-3776AB?style=for-the-badge)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white)



## 📌 O projekcie
Kompleksowa analiza danych sprzedażowych, finansowych i logistycznych brazylijskiej platformy e-commerce **Olist** obejmująca ponad 100 000 zamówień. Celem projektu była identyfikacja wąskich gardeł w procesach dostaw, analiza wpływu opóźnień na wskaźniki satysfakcji klientów (CSAT) oraz wyznaczenie rekomendacji biznesowych poprawiających Rentowność i LTV.

W projekcie przeprowadzono zaawansowaną eksploracyjną analizę danych (EDA), testowanie hipotez statystycznych (test ANOVA dla AOV a liczby rat) oraz zbudowano czytelne wizualizacje trendów i rozkładów.

## 📊 Podgląd Wykresów




### Dynamika Przychodu GMV vs Średni Czas Dostawy

<img width="1170" alt="GMV vs Delivery Time" src="https://github.com/user-attachments/assets/a24565f2-819b-40d1-a6bb-5bbfb5e6a34a" />

### Rozkład Czasu Dostawy a Ocena Klienta (CSAT)


<img width="1170" alt="CSAT vs Delivery Time Violin Plot" src="https://github.com/user-attachments/assets/b2186efa-44bd-4815-874a-eef49142adf5" />
## 💡 Kluczowe wskaźniki i wnioski biznesowe

* **Wysoka koncentracja obrotu (Ryzyko Vendorów):** Top 5% sprzedawców (146 firm) odpowiada za **52.99% łącznego przychodu GMV** platformy. Zidentyfikowano 104 sprzedawców wysokiego ryzyka ze wskaźnikiem opóźnień przekraczającym 20%.
* **Wpływ logistyki na CSAT:** Klienci wystawiający ocenę 5.0 otrzymują paczki średnio w **10.5 dnia**. Przekroczenie progu **12–14 dni** dostawy drastycznie zwiększa prawdopodobieństwo otrzymania oceny 1.0 (średni czas dostawy dla ocen negatywnych to ~17 dni z długim ogonem sięgającym 45 dni).
* **Wpływ płatności ratalnych na AOV (Test ANOVA $p < 0.001$):** Zamówienia opłacane jednorazowo generują średnią wartość koszyka na poziomie **97.15 BRL**, podczas gdy zakupy rozłożone na 6+ rat osiągają AOV równe **304.39 BRL**.
* **Sezonowość i Black Friday:** W listopadzie 2017 r. odnotowano skok GMV o **+54.13% MoM**, co wywołało przeciążenie sieci logistycznej i szczytowe opóźnienia w dostawach na początku 2018 roku.

## 🛠️ Wykorzystane technologie i biblioteki

* **Python 3.10** – środowisko analityczne.
* **Pandas & NumPy** – czyszczenie danych, transformacje strukturalne, agregacje i obliczenia statystyczne.
* **Matplotlib** – budowa zaawansowanych wykresów złożonych (wykresy dwuosiowe z adnotacjami).
* **Seaborn** – wizualizacja rozkładów statystycznych (Violin plots, Point plots).
* **SciPy (`scipy.stats`)** – weryfikacja hipotez statystycznych (jednoczynnikowa analiza wariancji ANOVA).

## 📁 Pliki do pobrania
* 🔗 **[Pobierz pliki projektu z Google Drive](https://drive.google.com/drive/folders/1WABFGCU9IX_M_V0DkG20TvpP3bi9Nlt5?usp=drive_link)** *(Kompletny zestaw danych oraz notatnik z analizą)*


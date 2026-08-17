# 🚚 Supply Chain Performance & Profitability Optimization Analysis

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-3776AB?style=for-the-badge)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white)

## 📌 O projekcie
Kompleksowa analiza danych operacyjnych i finansowych łańcucha dostaw (180 519 transakcji). Celem projektu było zidentyfikowanie wąskich gardeł w procesach logistycznych, ocena efektywności polityki cenowo-rabatowej oraz weryfikacja hipotez statystycznych dotyczących ryzyka opóźnień dostaw.

---

## 🛠️ Wykorzystany stos technologiczny
* **Język:** Python 3.x
* **Analiza i obróbka danych:** Pandas, NumPy
* **Statystyka:** SciPy (`scipy.stats` - Chi-square test, One-way ANOVA)
* **Wizualizacja:** Matplotlib, Seaborn

---

## 📊 Kluczowe wnioski z analizy 

1. **Błędy harmonogramowania (SLA) w logistyce:**
   * Metoda wysyłki **First Class** wykazała **100,0% opóźnień** względem zaplanowanego terminu, co generuje ryzyko utraty klientów priorytetowych.
   * Najpopularniejszy tryb **Standard Class** (60% wolumenu) okazał się najbardziej efektywny – tylko **39,8%** opóźnień.

2. **Sztywność polityki cenowej:**
   * Średnia stopa rabatowa na wszystkich 5 rynkach mieści się w wąskim przedziale **10,15% – 10,18%**.
   * Test ANOVA potweirdził brak istotnych statystycznie różnic w udzielanych rabatach pomiędzy segmentami klientów ($p = 0.873$).

3. **Anomalia wolumenowa w Q4 2017:**
   * Zidentyfikowano spadek rejestrowanych przychodów o **70,6%** pomiędzy wrześniem 2017 a styczniem 2018 roku, co wskazuje na niekompletność potoku danych źródłowych.

---

## 📈 Wizualizacja wyników

<img width="1270" height="479" alt="image" src="https://github.com/user-attachments/assets/7abb95c1-c72e-4436-8550-379d649b9ebf" />



---

## 📁 Pliki 
* 🔗 **[Pobierz pliki projektu z Google Drive](https://drive.google.com/drive/folders/1WABFGCU9IX_M_V0DkG20TvpP3bi9Nlt5?usp=drive_link)** *(Kompletny zestaw danych oraz notatnik z analizą)*
* 🔗 **[Link do bazy danych kaggle](https://www.kaggle.com/datasets/shashwatwork/dataco-smart-supply-chain-for-big-data-analysis)**

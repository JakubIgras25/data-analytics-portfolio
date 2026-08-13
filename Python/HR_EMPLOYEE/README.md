# 📊 HR Analytics – Employee Attrition & Retention Analysis
![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-3776AB?style=for-the-badge)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white)


Projekt poświęcony eksploracyjnej analizie danych (EDA) w obszarze HR, dotyczący rotacji pracowników (**Employee Attrition**). Celem analizy jest identyfikacja głównych czynników wpływających na odejścia pracowników, wyznaczenie obszarów wysokiego ryzyka oraz sformułowanie rekomendacji operacyjnych.

---

## 🚀 Kluczowe Wnioski 

* **Ogólny wskaźnik rotacji (Attrition Rate):** **18,07%** (542 odejścia na 3 000 pracowników).
* **Koncentracja w działach:** Rotacja ma charakter lokalny – dotyczy głównie działów **Production (21,39%)** oraz **Software Engineering (20,00%)**.
* **Krytyczny okres pierwszych 2 lat:** Byli pracownicy odchodzili średnio po **1,41 roku** pracy (w porównaniu do **3,80 roku** stażu u osób aktywnych).
* **Słaba wartość predykcyjna ankiet:** Oceny satysfakcji, zaangażowania i Work-Life Balance u osób odchodzących są niemal identyczne jak u aktywnych (~3,0 w skali 1–5), co wskazuje na niską skuteczność deklaratywnych ankiet ilościowych.

---

## 🛠️ Stos Technologiczny

* **Język:** Python 3.x
* **Akwizycja i obróbka danych:** `pandas`, `numpy`
* **Wizualizacja danych:** `matplotlib`, `seaborn`
* **Środowisko:** Google Colab / Jupyter Notebook

---


## 📊 Przeprowadzone Etapy Analizy

1. **Integracja i audyt danych:**
   * Scalenie danych kadrowych i wyników ankiet po identyfikatorze pracownika.
   * Walidacja spójności typów (formatowanie dat `DOB`, `StartDate`, `ExitDate`) oraz wyliczenie stażu i wieku.
2. **Exploratory Data Analysis (EDA):**
   * Analiza wskaźnika rotacji w rozbiciu na działy, strefy płacowe (`PayZone`), typ umowy (`EmployeeType`) oraz demografię.
3. **Wizualizacja & Weryfikacja Hipotez:**
   * Wykresy gęstości (KDE) stażu pracy (Aktywni vs Zwolnieni).
   * Wykres pierścieniowy struktury przyczyn odejść (`TerminationType`).
   * Boxploty rozkładu ocen z ankiet i macierz korelacji.
4. **Wnioski Biznesowe & Rekomendacje HR:**
   * Sformułowanie strategii retencyjnej ukierunkowanej na onboarding (0–2 lata) oraz audyty pracy w działach produkcyjnych.

---


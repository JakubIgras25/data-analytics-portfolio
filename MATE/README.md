# 📊 Email Campaign & User Engagement Analytics

![Google BigQuery](https://img.shields.io/badge/Google_BigQuery-4285F4?style=for-the-badge&logo=googlecloud&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-CC292B?style=for-the-badge&logo=microsoftsqlserver&logoColor=white)
![Looker Studio](https://img.shields.io/badge/Looker_Studio-4285F4?style=for-the-badge&logo=googlelookerstudio&logoColor=white)

Projekt poświęcony kompleksowej analizie efektywności kampanii e-mailowych oraz aktywności użytkowników na podstawie zintegrowanych danych sesyjnych z Google Analytics i dzienników zdarzeń pocztowych. Celem analizy jest ewaluacja leja konwersji (sent -> open -> visit), identyfikacja kluczowych rynków (Top 10 krajów) oraz segmentacja bazy wg weryfikacji kont.

## 🖼️ Podgląd Raportu & Schematu Bazy

<img width="1354" height="656" alt="image" src="https://github.com/user-attachments/assets/8cf4721e-c960-4348-b28d-8f2dac93a55d" />
<img width="940" height="738" alt="image" src="https://github.com/user-attachments/assets/420c7881-b5d1-4d5c-8e26-d0639a2f2b56" />

---
## 📊 Wyniki Zapytania SQL & Kluczowe Wnioski

### Podgląd Tabeli Wynikowej (Google BigQuery)
<img width="940" height="485" alt="image" src="https://github.com/user-attachments/assets/e711ef29-bb94-4b00-a5d0-8f8bafc678d8" />
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/c8885cf8-6851-4543-8ad1-3f90503577a5" />


## 🚀 Kluczowe Wnioski

* **Dominacja rynku amerykańskiego:** Stany Zjednoczone odpowiadają za główny wolumen ruchu – **12 384 zarejestrowanych kont** oraz **251 909 wysłanych wiadomości e-mail**.
* **Sezonowość wysyłek:** Szczyt intensywności kampanii przypadał na okres **grudzień 2020 – styczeń 2021** (~6 000 wiadomości dziennie), po czym nastąpiło stopniowe wygaszanie wysyłek do lutego 2021.
* **Wysoki wskaźnik weryfikacji:** Zdecydowana większość aktywnych odbiorców e-maili posiada status konta zweryfikowanego (`is_verified = 1`), co świadczy o dobrej jakości pozyskiwanego ruchu.
* **Geograficzny podział aktywności:** Rynek indyjski (**2 687 kont**) oraz kanadyjski (**2 067 kont**) plasują się na kolejnych miejscach w rankingu zaangażowania, stanowiąc kluczowe rynki wzrostowe.

---

## 🛠️ Stos Technologiczny

* **Baza danych & Dialekt:** Google Cloud BigQuery (SQL)
* **Wizualizacja danych:** Looker Studio
* **Narzędzia wspomagające:** Git / GitHub

---

## 📑 Przeprowadzone Etapy Analizy & Wyzwania Techniczne

1. **Integracja heterogenicznych źródeł danych:**
   * Połączenie relacyjnych tabel bazy danych (`account`, `account_session`) ze strukturami sesyjnymi Google Analytics (`session`, `session_params`) oraz tabelami zdarzeń e-mailowych (`email_sent`, `email_open`, `email_visit`).

2. **Zaawansowana Rekonstrukcja Osi Czasu :**
   * **Wyliczanie daty wysyłki:** Użycie funkcji `DATE_ADD` w celu przesunięcia daty sesji początkowej o względną liczbę dni z logów wysyłki (`INTERVAL es.sent_date DAY`).
   * **Agregacja wielopoziomowa (`UNION ALL`):** Połączenie w jednej strukturze czasowej zdarzeń tworzenia kont (0 wiadomości) oraz zdarzeń wysyłki/otwarcia/kliknięcia bez dublowania zliczeń, z wyliczeniem unikalnych wiadomości (`COUNT(DISTINCT)`).

3. **Optymalizacja Rankingu i Filtrowania:**
   * **Funkcje Okna (Window Functions):** Wyliczenie zagregowanych sum skumulowanych per kraj (`SUM(...) OVER (PARTITION BY country)`) oraz nadanie osobnych DENSE_RANK dla liczby kont i liczby wysłanych e-maili.
   * **Klauzula `QUALIFY`:** Zastosowanie zaawansowanej konstrukcji `QUALIFY` specyficznej dla BigQuery w celu przefiltrowania wyników funkcji okna (`rank <= 10`) bezpośrednio w zapytaniu głównym, unikając zbędnego zagnieżdżania w kolejnych podzapytaniach (CTE).

4. **Wizualizacja & Reporting w Looker Studio:**
   * Budowa interaktywnego dashboardu prezentującego rozkład wysyłek w czasie, strukturę weryfikacji kont oraz ranking Top 10 rynków.

---

## 💻 Kod SQL (BigQuery)

```sql
WITH
  account_base AS (
    SELECT
      a.id AS account_id,
      MIN(s.date) AS created_date,
      sp.country,
      a.send_interval,
      a.is_verified,
      a.is_unsubscribed
    FROM `data-analytics-mate.DA.account` a
    JOIN `data-analytics-mate.DA.account_session` acc ON a.id = acc.account_id
    JOIN `data-analytics-mate.DA.session` s ON acc.ga_session_id = s.ga_session_id
    JOIN `data-analytics-mate.DA.session_params` sp ON s.ga_session_id = sp.ga_session_id
    GROUP BY a.id, sp.country, a.send_interval, a.is_verified, a.is_unsubscribed
  ),
  aggregated_stats AS (
    SELECT
      date, country, send_interval, is_verified, is_unsubscribed,
      SUM(account_cnt) AS account_cnt,
      SUM(sent_msg) AS sent_msg,
      SUM(open_msg) AS open_msg,
      SUM(visit_msg) AS visit_msg
    FROM (
        SELECT
          DATE(created_date) AS date, country, send_interval, is_verified, is_unsubscribed,
          COUNT(account_id) AS account_cnt, 0 AS sent_msg, 0 AS open_msg, 0 AS visit_msg
        FROM account_base
        GROUP BY 1, 2, 3, 4, 5

        UNION ALL

        SELECT
          DATE_ADD(DATE(s.date), INTERVAL es.sent_date DAY) AS date,
          sp.country, a.send_interval, a.is_verified, a.is_unsubscribed,
          0,
          COUNT(es.id_message),
          COUNT(DISTINCT eo.id_message),
          COUNT(DISTINCT ev.id_message)
        FROM `data-analytics-mate.DA.email_sent` es
        JOIN `data-analytics-mate.DA.account` a ON es.id_account = a.id
        JOIN `data-analytics-mate.DA.account_session` acc ON a.id = acc.account_id
        JOIN `data-analytics-mate.DA.session` s ON acc.ga_session_id = s.ga_session_id
        JOIN `data-analytics-mate.DA.session_params` sp ON s.ga_session_id = sp.ga_session_id
        LEFT JOIN `data-analytics-mate.DA.email_open` eo ON es.id_message = eo.id_message
        LEFT JOIN `data-analytics-mate.DA.email_visit` ev ON es.id_message = ev.id_message
        GROUP BY 1, 2, 3, 4, 5
      )
    GROUP BY 1, 2, 3, 4, 5
  ),
  country_totals AS (
    SELECT
      *,
      SUM(account_cnt) OVER (PARTITION BY country) AS total_country_account_cnt,
      SUM(sent_msg) OVER (PARTITION BY country) AS total_country_sent_cnt
    FROM aggregated_stats
  )
SELECT
  *,
  DENSE_RANK() OVER (ORDER BY total_country_account_cnt DESC) AS rank_total_country_account_cnt,
  DENSE_RANK() OVER (ORDER BY total_country_sent_cnt DESC) AS rank_total_country_sent_cnt
FROM country_totals
WHERE total_country_account_cnt >= 0
QUALIFY rank_total_country_account_cnt <= 10 OR rank_total_country_sent_cnt <= 10
ORDER BY date DESC;

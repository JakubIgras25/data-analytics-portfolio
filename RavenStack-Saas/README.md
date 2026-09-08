![Power BI](https://img.shields.io/badge/-Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/-DAX-2C2C2C?style=for-the-badge)
![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)
![Python](https://img.shields.io/badge/-Python-3776AB?style=for-the-badge&logo=python&logoColor=white)

# RavenStack — analiza SaaS w Power BI

Projekt poświęcony kompleksowej analizie kondycji finansowej i operacyjnej fikcyjnej firmy SaaS. Dane obejmują konta klientów, historię ich subskrypcji, użycie produktu, zgłoszenia do supportu i zdarzenia churnu — pięć powiązanych tabel, około 33 tysięcy wierszy łącznie.

<img width="1126" height="797" alt="image" src="https://github.com/user-attachments/assets/9d5140eb-2ceb-49e1-a1e9-4b8ab528a746" />
<img width="1125" height="798" alt="image" src="https://github.com/user-attachments/assets/05710302-700e-4bdd-97e4-b3083755624e" />
<img width="1142" height="797" alt="image" src="https://github.com/user-attachments/assets/5c5f6956-1046-4dde-8439-acc15265e369" />
<img width="1147" height="802" alt="image" src="https://github.com/user-attachments/assets/35651e5f-6467-4ced-977a-9971791cb52e" />
<img width="1163" height="795" alt="image" src="https://github.com/user-attachments/assets/e1cba265-6e16-4bfc-bb0a-2e4d2ddd25d8" />


---

## O czym jest ten raport

Raport ma pięć stron: przegląd finansowy, analizę churnu i retencji, konwersję z trialu na plan płatny, użycie produktu oraz kondycję klientów wspieraną przez dane z supportu. Każda strona odpowiada na konkretne pytanie, jakie zadałby zarząd rosnącej firmy SaaS.

**Ile realnie tracimy klientów i dlaczego to trudniejsze pytanie, niż się wydaje.** W bazie znalazły się trzy niezależne pola, z których każde dawało inną odpowiedź na pytanie „ilu klientów odeszło" — 110, 312 i 352 konta na pięćset. Żadne z nich nie było poprawne w kontekście raportu z filtrem czasowym: dwa opisywały zakończenie pojedynczego kontraktu, nie utratę klienta, a trzecie było polem statycznym, niereagującym na wybrany okres. Finalna metryka opiera się na dynamicznej definicji, zbudowanej od podstaw, i daje 35 kont — liczbę spójną z resztą modelu i zmieniającą się razem ze slicerem.

**Skąd bierze się przychód i czy firma jest bezpiecznie zdywersyfikowana.** Rozbicie MRR według kraju, branży i planu pokazuje wyraźną koncentrację w Stanach Zjednoczonych i planie Enterprise — ponad połowa przychodu pochodzi z jednego rynku.

**Jak skutecznie trial zamienia się w płatnego klienta.** Wskaźnik konwersji sięga 93%, ale ciekawszym wnioskiem jest to, że różni klienci decydują się w bardzo różnym tempie — jedna trzecia konwertuje dopiero po trzydziestym dniu, co ma bezpośrednie znaczenie dla długości okresu próbnego.

**Czy support obsługuje zgłoszenia w rozsądnym czasie.** Deklarowany priorytet zgłoszenia praktycznie nie wpływa na rzeczywisty czas jego rozwiązania, a próg SLA okazał się ustawiony poniżej realnych możliwości zespołu.

**Które konta wymagają uwagi już dziś.** Zestawienie liczby zgłoszeń i oceny satysfakcji w jeden wskaźnik zdrowia klienta, gotowe do użycia przez zespół Customer Success.

---

## Co było nie tak w danych i jak to naprawiłem

To jest część, która zajęła najwięcej czasu i miała największy wpływ na finalny kształt modelu.

Kolumna oznaczająca koniec subskrypcji była wypełniona w zaledwie dziesięciu procentach wierszy. Powód okazał się prosty do znalezienia, ale trudny do naprawienia: przy każdej zmianie planu generator danych tworzył nowy wiersz subskrypcji, nigdy nie zamykając poprzedniego. W praktyce każde konto miało średnio dziewięć jednocześnie „otwartych" subskrypcji, mimo że realnie płaciło za jedną. Naiwne liczenie aktywnych klientów na tej podstawie zawyżało wynik niemal dziesięciokrotnie — testowa metryka liczby aktywnych licencji pokazywała sto trzydzieści pięć tysięcy zamiast czternastu tysięcy stu dwudziestu sześciu.

Rozwiązaniem była kolumna obliczeniowa ustawiająca subskrypcje każdego konta w chronologiczną kolejkę i wskazująca dokładnie jedną jako aktualną — tę, po której nic już nie wystartowało. Dopiero na tym jednym wierszu data zakończenia niesie realną informację o statusie klienta. Budowa tej kolumny wymagała też rozwiązania problemu remisów: sto pięćdziesiąt dwie pary subskrypcji tego samego konta zaczynały się dokładnie tego samego dnia, co bez dodatkowej reguły sortowania prowadziłoby do podwójnego liczenia.

Osobnym wątkiem było poprawne liczenie przychodu miesięcznego. Miesięczny przychód powtarzalny jest z natury stanem, nie przepływem, więc sumowanie go po okresach bez odpowiedniego filtra prowadzi do wielokrotnego liczenia tej samej subskrypcji. Pierwsza wersja tej metryki dawała sto trzydzieści sześć milionów, wersja pośrednia sześćdziesiąt dwa miliony, a finalna, poprawna — jedenaście i siedem dziesiątych miliona. Wszystkie trzy wyniki wyglądały wiarygodnie, co pokazuje, jak łatwo takiemu błędowi umknąć bez niezależnej weryfikacji.

Model celowo nie ma relacji między tabelą kalendarza a tabelą subskrypcji. Gdyby taka relacja istniała, filtr czasowy propagowałby się po dacie rozpoczęcia subskrypcji, a nie po tym, czy była wtedy faktycznie aktywna — co przywróciłoby dokładnie ten sam błąd sumowania przychodu przy każdym nowym wizualu. Cena tej decyzji jest wyraźna w kodzie: każda miara operująca na tej tabeli musi ręcznie odczytać i nałożyć okno czasowe.

Wszystkie pięćdziesiąt miar DAX oraz kolumny pomocnicze zostały niezależnie przeliczone w SQL, dla kilku różnych okresów czasowych, żeby upewnić się, że reagują poprawnie na filtr daty, a nie tylko wyglądają dobrze dla jednego, przetestowanego przypadku.

---

## Pobranie plików

Plik Power BI,SQL oraz baza danych w formacie CSV przekraczają limit rozmiaru obowiązujący na GitHubie i są dostępne pod linkiem:

**[LINK DO GOOGLE DRIVE](https://drive.google.com/drive/folders/1CjGHYQPCoLEYc1cvr6AU-b3SwZz1MshK?usp=sharing)**

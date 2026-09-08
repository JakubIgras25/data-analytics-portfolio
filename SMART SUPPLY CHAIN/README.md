![Python](https://img.shields.io/badge/-Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/-Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/-NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![Jupyter](https://img.shields.io/badge/-Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)
![Matplotlib](https://img.shields.io/badge/-Matplotlib-11557C?style=for-the-badge&logo=matplotlib&logoColor=white)
![Seaborn](https://img.shields.io/badge/-Seaborn-444876?style=for-the-badge)

# DataCo Smart Supply Chain — analiza łańcucha dostaw w Pythonie

Projekt poświęcony analizie danych sprzedażowych i logistycznych firmy działającej na pięciu kontynentach. Dane obejmują 180 519 pozycji zamówień z lat 2015–2018 oraz blisko pół miliona odsłon stron produktowych z ostatnich pięciu miesięcy tego okresu — dwa pliki, bez wspólnego klucza poza nazwą produktu.

<img width="907" height="572" alt="image" src="https://github.com/user-attachments/assets/82da7862-cca7-4ae6-81ec-b25f919c951e" />
<img width="703" height="322" alt="image" src="https://github.com/user-attachments/assets/7ed630c3-2c46-4761-ba90-7a2ff292da17" />
<img width="630" height="307" alt="image" src="https://github.com/user-attachments/assets/816716f4-d7d1-4e38-b62c-70513f3a6f9f" />
<img width="768" height="310" alt="image" src="https://github.com/user-attachments/assets/98fc334d-602b-4627-8afc-4f4f6d97d33e" />
<img width="722" height="322" alt="image" src="https://github.com/user-attachments/assets/8f9cb3a4-3278-42b0-aba3-b19f82de8c2d" />


---

## O czym jest ten projekt

Notebook odpowiada na pięć pytań, jakie zadałby zespół operacyjny takiej firmy, i na koniec pokazuje trzy pytania, na które te dane nie potrafią odpowiedzieć.

**Ile firma naprawdę traci na opóźnionych dostawach i czy to problem logistyki, czy złożonej obietnicy.** Ponad połowa przesyłek w całym zbiorze przychodzi po obiecanym terminie. Jeden z czterech trybów wysyłki ma sto procent opóźnień, bo deklarowany czas dostawy jest krótszy niż jakikolwiek realny transport tym trybem — nie zdarzył się ani jeden przypadek dotrzymania terminu. Sama korekta tej jednej liczby w cenniku sprowadza ogólny wskaźnik opóźnień z pięćdziesięciu siedmiu do czterdziestu dwóch procent, bez zmiany czegokolwiek w operacjach.

**Skąd bierze się przychód i jak bardzo jest skoncentrowany w kilku produktach.** Klasyfikacja ABC pokazuje, że większość katalogu ma sprzedaż śladową, a niewielka grupa produktów odpowiada za zdecydowaną większość przychodu.

**Czy geografia klienta rzeczywiście tłumaczy różnice w czasie dostawy.** Surowe zestawienie po rynkach sugeruje, że jedne regiony obsługuje się gorzej niż inne. Po rozbiciu na tryb wysyłki ta różnica znika niemal całkowicie — to wybór trybu, nie lokalizacja, decyduje o czasie dostawy.

**Co logi odwiedzin strony mówią o realnym zainteresowaniu produktami.** Zestawienie prawie pół miliona odsłon ze sprzedażą pokazuje produkty oglądane znacznie częściej, niż kupowane, i odwrotnie — kandydatów do przeglądu ceny albo opisu.

**Których analiz świadomie nie zrobiłem, bo dane na to nie pozwalają.** Rentowność kategorii i ryzyko oszustwa płatniczego wyglądają na policzalne, dopóki nie sprawdzi się ich statystycznie. Marża produktowa nie koreluje z niczym w danych, a status podejrzenia oszustwa występuje wyłącznie przy jednej metodzie płatności — wpisany w konstrukcję zbioru, nie w zachowanie klientów. Zamiast prezentować wnioski oparte na szumie, oba tematy są w raporcie udokumentowane jako niemożliwe do policzenia na tych danych.

---

## Co było nie tak w danych i jak to naprawiłem

Kolumna oznaczająca ryzyko opóźnienia dostawy wyglądała na gotową metrykę do dalszej analizy. Okazała się nie być liczona z rzeczywistych dni dostawy, tylko przepisana ze statusu zamówienia — dla ponad czterech tysięcy wierszy ze statusem anulowanej wysyłki flaga była na sztywno ustawiona na zero, mimo że transport i tak przekroczyłby termin, gdyby się odbył. Użycie tej kolumny wprost jako cechy w jakimkolwiek modelu przewidującym opóźnienia byłoby wyciekiem zmiennej celu. Policzyłem ją od nowa bezpośrednio z dat.

Dwie kolumny nosiły nazwy sugerujące gotowy agregat — zysk na zamówieniu i sprzedaż na klienta. Sprawdzenie ziarna tabeli pokazało, że obie zmieniają wartość między pozycjami tego samego zamówienia, więc opisują pojedynczą pozycję, nie zamówienie ani klienta. Zsumowanie ich wprost jako metryki klienckiej dałoby liczbę bez pokrycia w rzeczywistości.

Osobnym wątkiem były zamówienia anulowane i oznaczone jako podejrzane o oszustwo. Miały w pełni wypełnione daty wysyłki, czasy transportu i kwoty, mimo że nigdy fizycznie nie wyjechały — dane wygenerowano dla nich tak samo jak dla zrealizowanych zamówień. Zostawienie ich w mianowniku wskaźnika dostaw zaniżało odsetek opóźnień o kilka punktów procentowych.

Deklarowany termin dostawy okazał się w stu procentach wyznaczony przez wybrany tryb wysyłki, bez żadnej innej zmiennej — nie ma w danych ani jednego wyjątku od tej reguły. To ustalenie stoi za całą analizą w pierwszej sekcji: skoro termin jest funkcją jednej kolumny, testowanie różnych scenariuszy SLA per tryb wysyłki daje bezpośrednio przełożenie na wynik operacyjny.

Wolumen zamówień gwałtownie spada od października ostatniego roku objętego zbiorem i zostaje niski aż do końca danych. To nie jest spadek sprzedaży, tylko efekt obcięcia zbioru w połowie miesiąca — każdy wykres trendu kończący się na ostatnim miesiącu pokazywałby załamanie, którego nigdy nie było. Wnioski o dynamice sprzedaży w czasie ograniczyłem do pełnych miesięcy.

Przy analizie marży policzyłem nie tylko korelację z rabatem i wartością sprzedaży, ale też błąd standardowy średniej w każdej kategorii produktowej, żeby odróżnić realną różnicę w rentowności od zwykłego szumu przy tej liczbie wierszy. Żadna z dwudziestu czterech przetestowanych kategorii nie odchyla się od średniej globalnej bardziej, niż wynikałoby z samego losowania — dokładnie tego oczekuje się po czystym przypadku. To był argument decydujący, żeby nie budować dalej żadnej metryki rentowności na tej kolumnie.

---

## Pobranie plików

Notebook oraz baza danych w formacie CSV przekraczają limit rozmiaru obowiązujący na GitHubie i są dostępne pod linkiem:

**[LINK DO GOOGLE DRIVE](https://drive.google.com/drive/folders/13o8g_DggXTSoykM9cqPqBQzc3Jq7_Wu7?usp=sharing)**

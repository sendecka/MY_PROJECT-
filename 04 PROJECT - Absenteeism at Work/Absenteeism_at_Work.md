# 📊 Absenteeism at Work — Power BI Report Documentation

**Wersja:** 1.0  
**Data:** 2026  
**Autor:** Power BI Developer  
**Dataset:** Absenteeism_at_work.csv

---

## Spis treści

1. [Opis projektu](#1-opis-projektu)
2. [Źródło danych](#2-źródło-danych)
3. [Model danych](#3-model-danych)
4. [Struktura raportu](#4-struktura-raportu)
5. [Strona: Executive Summary](#5-strona-executive-summary)
6. [Strona: Time Trend](#6-strona-time-trend)
7. [Strona: Root Cause](#7-strona-root-cause)
8. [Strona: Risk](#8-strona-risk)
9. [Strona: Recommendations](#9-strona-recommendations)
10. [Miary DAX](#10-miary-dax)
11. [Konwencje i standardy](#11-konwencje-i-standardy)
12. [Znane ograniczenia](#12-znane-ograniczenia)

---

## 1. Opis projektu

Raport **Absenteeism at Work** to interaktywny dashboard Power BI analizujący wzorce absencji pracowniczej w firmie produkcyjno-biurowej (USA). Celem raportu jest identyfikacja pracowników wysokiego ryzyka, głównych przyczyn nieobecności oraz dostarczenie rekomendacji dla działu HR.

### Cele biznesowe

- Monitorowanie łącznych godzin absencji i ich kosztu
- Identyfikacja pracowników wysokiego ryzyka metodą Bradford Factor
- Analiza wzorców czasowych (miesiąc, dzień tygodnia, sezon)
- Analiza przyczyn absencji wg kodów ICD
- Dostarczenie konkretnych rekomendacji HR opartych o dane

### Odbiorcy raportu

| Odbiorca | Zakładki |
|---|---|
| Zarząd | Executive Summary, Recommendations |
| HR Manager | Risk, Root Cause, Recommendations |
| Kierownicy operacyjni | Time Trend, Root Cause |

---

## 2. Źródło danych

### Plik CSV

| Parametr | Wartość |
|---|---|
| Nazwa pliku | `Absenteeism_at_work.csv` |
| Źródło | `www.kaggle.com` |
| Liczba rekordów | 740 |
| Liczba kolumn | 21 |
| Kraj | USA (firma produkcyjno-biurowa) |

### Kolumny datasetu

| Kolumna | Typ | Opis |
|---|---|---|
| `ID` | Integer | Identyfikator pracownika (1–36) |
| `Reason for absence` | Integer | Kod ICD powodu nieobecności (0–28) |
| `Month of absence` | Integer | Miesiąc nieobecności (0=brak, 1–12) |
| `Day of the week` | Integer | Dzień tygodnia (2=Pon, 3=Wt, 4=Śr, 5=Czw, 6=Pt) |
| `Seasons` | Integer | Pora roku (1=Spring, 2=Summer, 3=Autumn, 4=Winter) |
| `Transportation expense` | Integer | Miesięczny koszt dojazdu ($) |
| `Distance from Residence to Work` | Integer | Dystans dom–praca (km) |
| `Service time` | Integer | Staż pracy (lata) |
| `Age` | Integer | Wiek pracownika |
| `Work load Average/day` | Float | Średnie obciążenie pracą dziennie |
| `Hit target` | Integer | Realizacja celu (%) |
| `Disciplinary failure` | Integer | Naruszenia dyscypliny (0/1) |
| `Education` | Integer | Wykształcenie (1=High school, 2=Graduate, 3=Postgrad) |
| `Son` | Integer | Liczba dzieci |
| `Social drinker` | Integer | Pijący alkohol (0/1) |
| `Social smoker` | Integer | Palący (0/1) |
| `Pet` | Integer | Liczba zwierząt domowych |
| `Weight` | Integer | Waga (kg) |
| `Height` | Integer | Wzrost (cm) |
| `Body mass index` | Integer | BMI |
| `Absenteeism time in hours` | Integer | Godziny nieobecności |

### Mapowanie kodów ICD (Reason for absence)

| Kod | Nazwa |
|---|---|
| 0 | Unknown |
| 1 | Infectious diseases |
| 2 | Neoplasms | 
| 3 | Blood diseases | 
| 4 | Endocrine diseases |
| 5 | Mental disorders |
| 6 | Nervous system diseases |
| 7 | Eye diseases |
| 8 | Ear diseases |
| 9 | Circulatory diseases |
| 10 | Respiratory diseases |
| 11 | Digestive diseases |
| 12 | Skin diseases |
| 13 | Musculoskeletal diseases |
| 14 | Genitourinary diseases |
| 15 | Pregnancy |
| 16 | Perinatal conditions |
| 17 | Congenital anomalies |
| 18 | Symptoms & abnormal findings |
| 19 | Injury & poisoning |
| 21 | Health status factors |
| 22 | Patient follow-up |
| 23 | Medical consultation |
| 24 | Blood donation |
| 25 | Laboratory examination |
| 26 | Unjustified absence |
| 27 | Physiotherapy |
| 28 | Dental consultation |

### Mapowanie pór roku (Seasons)

| Kod | Pora roku |
|---|---|
| 1 | Spring |
| 2 | Summer |
| 3 | Autumn |
| 4 | Winter |

### Transformacje Power Query

- Dodanie nowych tabel wymiarów w celu optymalizacji modelu
- Zmiana typów kolumn na odpowiednie do zawartości

---

## 3. Model danych

Typ modelu - Model przyjmuje strukturę schematu gwiazdy (Star Schema).

### Tabele

| Tabela | Typ | Opis |
|---|---|---|
| `AbsenteeismAtWork` | Fact table | Główna tabela z rekordami absencji |
| `Education` | Dimension table | Tabela przechowująca poziom wykształcenia pracownika |
| `MonthOfAbsence` | Dimension table | Tabela przechowująca miesiąc w którym nastąpiła nieobecność |
| `ReasonForAbsence` | Dimension table | Tabela przechowująca powód nieobecności pracownika według kodów ICD |
| `Seasons` | Dimension table | Tabela przechowująca porę roku w której nastąpiła nieobecność |
| `DayOfTheWeek` | Dimension table | Tabela przechowująca dzień tygodnia w którym nastąpiła nieobecność |
| `Service time` | Dimension table | Tabela przechowująca liczbę lat w firmie |

### Tabela pomocnicza — Measure

### Relacje

Model oparty jest o relacje *1: (one-to-many)**

| Wymiar | Fakt | Typ relacji |
|---|---|---|
| Education	| AbsenteeismAtWork | 1:* |
| MonthOfAbsence | AbsenteeismAtWork | 1:* |
| ReasonForAbsence | AbsenteeismAtWork | 1:* |
| Seasons | AbsenteeismAtWork | 1:* |
| DayOfTheWeek | AbsenteeismAtWork | 1:* |
| Service time | AbsenteeismAtWork | 1:* |

Wszystkie relacje są jednokierunkowe (single direction – standard Power BI).

### Kluczowe parametry

| Parametr | Wartość | Uzasadnienie |
|---|---|---|
| Stawka godzinowa | $42/h | Średnia BLS dla sektora produkcyjno-biurowego USA (2026) |
| Próg High Risk (Bradford) | ≥600 | Standardowy próg HR wymagający natychmiastowego działania |
| Benchmark miesięczny | 427h | Średnia miesięczna z datasetu |
| Alert miesięczny | 480h | Q3 (75. percentyl) z datasetu |

---

## 4. Struktura raportu

### Nawigacja

Raport zawiera 6 zakładek z górnym menu nawigacyjnym:

```
Home | Summary | Time Trend | Root Cause | Risk | Recommendations
```

### Paleta kolorów

Główne kolory zastosowane w raporcie.

| Kolor | Hex | Zastosowanie |
|---|---|---|
| Granatowy (główny) | `#1B3B6F` | Linia wykresu, border-left, wartości KPI |
| Jasnoniebieski | `#BDD0EF` | Border-left (odwrót kart), elementy drugorzędne |
| Czerwony | `#E24B4A` | Alerty, Critical Bradford, Autumn |
| Amber | `#BA7517` | Urgent Bradford, elevated |
| Zielony | `#639922` | Normal/Low risk |
| Niebieski akcent | `#378ADD` | Monitor Bradford |


### Typografia

- Font: `-apple-system, Segoe UI, sans-serif`
- Border radius kart: 20px
- Box shadow: 0 10px 10px 

---

## 5. Strona: Executive Summary

### Opis

Strona główna raportu z kluczowymi wskaźnikami absencji dla całej organizacji.

### KPI Cards

| Miara | Front | Odwrót |
|---|---|---|
| `Summary KPI 1` | Total Absence Hours: **5 124h** / Total Absence Hours Previous Year: **4 876** |
| `Summary KPI 2` | Avg Hours / Employee: **6,9h** | Top 4 pracownicy wg godzin |
| `Summary KPI 3` | High Risk Employees: **18** | Bradford Factor 5 poziomu |
| `Summary KPI 4` | Est. Annual Cost: **$215k** | Koszt wg poziomów Bradford |

### Wykresy

| Wizualizacja | Typ | Opis |
|---|---|---|


---

## 6. Strona: Time Trend

### Opis

Analiza trendów czasowych absencji — miesięczna, tygodniowa i sezonowa.

### KPI Cards

| Miara | Front | Odwrót |
|---|---|---|
| `Time Trend KPI 1` | Peak Month: **March** | Peak Month Hours: **765h** |
| `Time Trend KPI 2` | Peak Day: **Monday** | Peak Day Hours: **1,489h** |
| `Time Trend KPI 3` | Peak Season: **Summer** | Peak Season Hours: **1,492h** |
| `Time Trend KPI 4` | Monday Effect: **29.1%** | Mon vs Tue–Fri porównanie |
| `Time Trend KPI 5` | Longest Absence: **120h** | Szczegóły: ID #14, Listopad |

### Wykresy

| Wizualizacja | Typ | Opis |
|---|---|---|
| Monthly Trend | SVG line chart (HTML) | Trend miesięczny z benchmark 480h (czerwona linia) i etykietami szczytów |
| Day of Week Pattern | HTML bar chart | Dni tygodnia posortowane wg godzin malejąco |
| Seasonality | HTML column chart | 4 pory roku z % udziałami |
| Absence Heatmap | HTML grid | Siatka 12×5 (miesiąc × dzień) z kolumną Total |

### Miary pomocnicze

| Miara | Wartość | Opis |
|---|---|---|
| `Benchmark AVG` | 427 | Stała — średnia miesięczna |
| `Benchmark Alert` | 480 | Stała — Q3 miesięczny |
| `Peak Month Label` | IF(month=3 OR 7; hours; BLANK()) | Etykiety szczytów na wykresie |

---

## 7. Strona: Root Cause

### Opis

Analiza przyczyn absencji — kody ICD, profil pracownika, korelacje.

### KPI Cards

| Miara | Front | Odwrót |
|---|---|---|
| `Root Cause KPI Season` | Peak Season: **Summer** | Peak Season Hours |
| `Root Cause KPI Weekend` | Monday Effect: **29.1%** | Mon vs Tue-Fri |

### Wykresy

| Wizualizacja | Typ | Opis |
|---|---|---|
| Top Absence Reason | SVG horizontal bars | Top 7 powodów + Other |
| Total Absence by Service Years | SVG column chart | 5 grup stażu pracy |
| ICD Reason Codes Chart | HTML bar chart | Top 7 kodów ICD + Other dynamiczny |
| Absence vs Distance | HTML scatter plot | 36 pracowników, statyczny |
| Age KPI Row | HTML 3 KPI | Unique employees, Avg age, Age range |
| Absence by Age | HTML column chart | 4 grupy wiekowe z gradientem |
| Social & Education | HTML dual bar | Social habits + Education level |
| Distance from Home | HTML bar chart | 4 grupy dystansu |

---

## 8. Strona: Risk

### Opis

Analiza ryzyka pracowniczego oparta o Bradford Factor.

### Bradford Factor

**Formuła:** `B = S² × D`

gdzie:
- `S` = liczba epizodów absencji (rekordy gdzie hours > 0)
- `D` = łączna liczba dni absencji (hours / 8)

### Poziomy ryzyka

| Poziom | Próg Bradford | Pracownicy | Kolor |
|---|---|---|---|
| Critical | ≥ 600 | 18 | `#E24B4A` |
| Urgent | 400–599 | 2 | `#BA7517` |
| Formal | 200–399 | 2 | `#C49A00` |
| Monitor | 100–199 | 3 | `#378ADD` |
| Normal | < 100 | 11 | `#639922` |

### KPI Cards Bradford

5 osobnych kart: `Bradford KPI Critical`, `Bradford KPI Urgent`, `Bradford KPI Formal`, `Bradford KPI Monitor`, `Bradford KPI Normal`

### Tabela pracowników

Miara `Top High Risk Table` — dynamiczna tabela HTML z `CONCATENATEX` zawierająca:
- ID (format EMP-XXX)
- Age
- Absences (liczba epizodów)
- Total Hours
- Bradford Factor
- Disciplinary failures
- Segment (Critical/Urgent/Formal/Monitor/Normal)

### Heatmapa

Miara `Heatmap Chart` — siatka 12×6 (miesiąc × dzień + Total) z kolorowaniem `rgba(27,59,111, opacity)`.

---

## 9. Strona: Recommendations

### Opis

Strona z rekomendacjami HR opartymi o dane z raportu.

### Komponenty

| Komponent | Opis |
|---|---|
| Alert bar | Amber — Monday Effect 29.1% |
| KPI Row | Est. Cost $215k, Savings $43k, Critical Bradford 18 |
| 6 kart rekomendacji | Podzielone na tagi: Critical/Observation/Preventive |
| KPIs to monitor | Bradford, Absence Rate, Est. Cost — cele na 12 miesięcy |

### Rekomendacje

| # | Tytuł | Tag | Podstawa danych |
|---|---|---|---|
| 1 | Referral to occupational medicine | Critical | Musculoskeletal 842h + Injury 729h |
| 2 | HR conversation + flexible hours | Critical | Dental 424h + Medical 293h |
| 3 | Preventive manager 1:1 | Observation | Bradford Urgent segment |
| 4 | Work culture audit | All departments | Monday 29.1% |
| 5 | Onboarding process review | Preventive | 0-5 years 338h |
| 6 | Hybrid eligibility expansion | Preventive | 30km+ korelacja |

---

## 10. Miary DAX

### Miary globalne

```dax
-- Stawka godzinowa
Hourly Rate = SELECTEDVALUE('Hourly Rate'[Hourly Rate Value]; 42)

-- Łączne godziny absencji
Total Absence Hours = SUM('AbsenteeismAtWork'[Absenteeism time in hours])

-- Średnia godzin per pracownik
AVG Hours per Employee =
AVERAGEX(
    VALUES('AbsenteeismAtWork'[ID]);
    CALCULATE(SUM('AbsenteeismAtWork'[Absenteeism time in hours]))
)

-- Szacowany koszt roczny
Est Annual Cost =
SUM('AbsenteeismAtWork'[Absenteeism time in hours]) * [Hourly Rate]
```

### Bradford Factor

```dax
Bradford Factor per Employee =
VAR _EmpData =
    ADDCOLUMNS(
        VALUES('AbsenteeismAtWork'[ID]);
        "Episodes"; CALCULATE(COUNTROWS(FILTER('AbsenteeismAtWork';
            'AbsenteeismAtWork'[Absenteeism time in hours] > 0)));
        "Days"; CALCULATE(SUMX('AbsenteeismAtWork';
            'AbsenteeismAtWork'[Absenteeism time in hours] / 8))
    )
RETURN
ADDCOLUMNS(_EmpData; "Bradford"; [Episodes] * [Episodes] * [Days])
```

### Benchmarki

```dax
Benchmark AVG = 427        -- średnia miesięczna
Benchmark Alert = 480      -- Q3 miesięczny (próg alertu)
```

### Etykiety szczytów

```dax
Peak Month Label =
VAR _CurrentMonth = SELECTEDVALUE('AbsenteeismAtWork'[Month of absence])
VAR _CurrentHours = SUM('AbsenteeismAtWork'[Absenteeism time in hours])
RETURN
IF(_CurrentMonth = 3 || _CurrentMonth = 7; _CurrentHours; BLANK())
```

---

## 11. Konwencje i standardy

### Nazewnictwo miar

| Prefiks | Znaczenie | Przykład |
|---|---|---|
| `Summary KPI` | Karty Executive Summary | `Summary KPI 1` |
| `Time Trend KPI` | Karty Time Trend | `Time Trend KPI 1` |
| `Root Cause KPI` | Karty Root Cause | `Root Cause KPI Season` |
| `Bradford KPI` | Karty Bradford | `Bradford KPI Critical` |
| `_VarName` | Zmienne lokalne DAX | `_HighRisk`, `_TotalH` |

### Styl kart HTML Content

Wszystkie karty używają jednolitego stylu:

```css
border-radius: 20px;
padding: 16px;
background: #ffffff;
box-shadow: 0 10px 10px rgba(0,0,0,0.06);
font-family: -apple-system, Segoe UI, sans-serif;
```

Flip cards:
- Front: `border-left: 3px solid #1B3B6F`
- Back: `border-left: 3px solid #BDD0EF`
- Hover trigger: `transform: rotateY(180deg)`

### Separator DAX

Raport używa **polskiego separatora** (`;`) zamiast angielskiego (`,`) w funkcjach DAX.

---

## 12. Znane ograniczenia

| Ograniczenie | Opis | Wpływ |
|---|---|---|
| Dataset akademicki | Dane z UCI ML Repository, nie produkcyjne | Wartości mogą być nierealistyczne (np. 482h absencji) |
| Brak roku | Kolumna `Month of absence` bez roku | Brak porównań YoY |
| Brak działu | Brak kolumny Department | Analiza wg działu niemożliwa |
| Brak daty | Tylko miesiąc i dzień tygodnia | Brak kalendarza, time intelligence ograniczone |
| HTML Content — brak JS | Wizualizacje HTML nie obsługują JavaScript | Brak tooltipów hover, brak interaktywności |
| Scatter plot statyczny | Wykres dystans vs absencja zahardkodowany | Nie reaguje na slicery |
| `Month = 0` | 3 rekordy pracowników bez absencji | Filtrowane w Power Query |
| Stawka $42/h | Założona stawka BLS 2026 | Nie pochodzi z danych — wymaga weryfikacji |

---

## Appendix — Kluczowe wyniki

| KPI | Wartość |
|---|---|
| Łączne godziny absencji | 5,124h |
| Szacowany koszt roczny | $215,208 |
| Potencjalne oszczędności (20%) | $43,042 |
| Unikalnych pracowników | 36 |
| Szczytowy miesiąc | Marzec (765h) |
| Szczytowy dzień | Poniedziałek (1,489h = 29.1%) |
| Szczytowy sezon | Autumn (1,492h) |
| Najdłuższa absencja | 120h (ID #14, Listopad) |
| Critical Bradford (≥600) | 18 pracowników (50%) |
| Benchmark miesięczny | 427h |
| Miesięcy powyżej benchmarku | 4/12 (Marzec, Kwiecień, Lipiec, Listopad) |
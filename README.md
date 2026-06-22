# Projekt końcowy – Relacyjne bazy danych

## Temat projektu

**System zarządzania danymi bioinformatycznymi:** pacjenci, próbki biologiczne, badania oraz wyniki analiz genetycznych.

## Cel projektu

Celem projektu było utworzenie relacyjnej bazy danych w systemie MySQL, zaimportowanie danych z plików CSV, wykonanie zapytań SQL oraz wyeksportowanie wybranych wyników do plików CSV.

## Struktura bazy danych

Projekt zawiera pięć głównych tabel:

| Tabela | Opis |
|---|---|
| `patients` | dane pacjentów |
| `samples` | próbki biologiczne pobrane od pacjentów |
| `tests` | wykonane badania genetyczne |
| `patient_test` | tabela pośrednia dla relacji pacjent–badanie |
| `results` | wyniki badań, czyli wykryte warianty genetyczne |

Tabela `patient_test` pełni funkcję tabeli pośredniej i definiuje relację wiele-do-wielu między pacjentami i badaniami.

## Relacje w bazie danych

- Jeden pacjent może mieć wiele próbek.
- Jedna próbka może być powiązana z badaniem.
- Jeden pacjent może uczestniczyć w wielu badaniach.
- Jedno badanie może dotyczyć wielu pacjentów.
- Jedno badanie może generować wiele wyników.

## Liczba rekordów w tabelach

Po imporcie danych do bazy uzyskano następującą liczbę rekordów:

| Tabela | Liczba rekordów |
|---|---:|
| `patients` | 100 |
| `samples` | 150 |
| `tests` | 150 |
| `patient_test` | 150 |
| `results` | 500 |

## Zapytania SQL

### Zapytanie 4i

Wyświetlenie pacjentów, ich badań oraz daty wykonania badania.

```sql
SELECT 
    p.patient_id,
    p.name,
    t.test_type,
    t.test_date
FROM patients p
JOIN patient_test pt ON p.patient_id = pt.patient_id
JOIN tests t ON pt.test_id = t.test_id
ORDER BY p.patient_id, t.test_date;
```

Wynik zapytania został wyeksportowany do pliku:

```text
patients_tests.csv
```

Liczba wyeksportowanych rekordów: **150**.

---

### Zapytanie 4ii

Wyświetlenie nazw testów, w których uczestniczyło więcej niż 30 pacjentów, oraz liczby pacjentów wykonujących dany test.

```sql
SELECT 
    t.test_type,
    COUNT(DISTINCT pt.patient_id) AS num_patients
FROM tests t
JOIN patient_test pt ON t.test_id = pt.test_id
GROUP BY t.test_type
HAVING COUNT(DISTINCT pt.patient_id) > 30;
```

**Wynik:**

| test_type | num_patients |
|---|---:|
| NGS-panel | 38 |
| SNP Array | 39 |

---

### Zapytanie 4iii

Wyświetlenie nazw testów oraz obliczonej średniej liczby wariantów dla każdego typu testu.

```sql
SELECT 
    t.test_type,
    ROUND(COUNT(r.result_id) / COUNT(DISTINCT t.test_id), 2) AS avg_variants
FROM tests t
LEFT JOIN results r ON t.test_id = r.test_id
GROUP BY t.test_type;
```

**Wynik:**

| test_type | avg_variants |
|---|---:|
| NGS-panel | 3.58 |
| SNP Array | 3.00 |
| WES | 3.43 |
| WGS | 3.48 |

---

### Zapytanie 4iv

Wyświetlenie pacjentów, którzy uczestniczyli zarówno w teście `SNP Array`, jak i `WGS`.

```sql
SELECT 
    p.patient_id,
    p.name
FROM patients p
JOIN patient_test pt ON p.patient_id = pt.patient_id
JOIN tests t ON pt.test_id = t.test_id
WHERE t.test_type IN ('SNP Array', 'WGS')
GROUP BY p.patient_id, p.name
HAVING COUNT(DISTINCT t.test_type) = 2;
```

Wynik zapytania został wyeksportowany do pliku:

```text
patients_both_tests.csv
```

Liczba wyeksportowanych rekordów: **9**.

---

### Zapytanie 4v

Wyświetlenie pacjentów i ich wyników testów `SNP Array` o wpływie biologicznym `Pathogenic`.

```sql
SELECT 
    p.patient_id,
    p.name,
    t.test_type,
    r.variant,
    r.position,
    r.impact
FROM patients p
JOIN patient_test pt ON p.patient_id = pt.patient_id
JOIN tests t ON pt.test_id = t.test_id
JOIN results r ON t.test_id = r.test_id
WHERE t.test_type = 'SNP Array'
  AND r.impact = 'Pathogenic'
ORDER BY p.patient_id;
```

Wynik zapytania został wyeksportowany do pliku:

```text
pathogenic_results.csv
```

Liczba wyeksportowanych rekordów: **34**.

## Eksport wyników

W projekcie wyeksportowano trzy wymagane pliki CSV:

| Plik | Opis |
|---|---|
| `patients_tests.csv` | lista pacjentów, rodzaj testu i data wykonania badania |
| `patients_both_tests.csv` | lista pacjentów, którzy uczestniczyli w testach `SNP Array` i `WGS` |
| `pathogenic_results.csv` | lista pacjentów z wariantami o wpływie `Pathogenic` w teście `SNP Array` |

## Pliki w repozytorium

Repozytorium zawiera:

- `README.md`
- `patients_tests.csv`
- `patients_both_tests.csv`
- `pathogenic_results.csv`

## Podsumowanie

W ramach projektu utworzono relacyjną bazę danych dla danych bioinformatycznych, zaimportowano dane z plików CSV, wykonano wymagane zapytania SQL oraz wyeksportowano wyniki do plików CSV. 

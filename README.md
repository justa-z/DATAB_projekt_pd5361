# Projekt końcowy - Relacyjne bazy danych

## Temat projektu

System zarządzania danymi bioinformatycznymi: pacjenci, próbki, badania i wyniki analiz genetycznych.

## Struktura bazy danych

Projekt zawiera pięć tabel:

- patients
- samples
- tests
- patient_test
- results

Tabela `patient_test` pełni funkcję tabeli pośredniej i definiuje relację wiele-do-wielu między pacjentami i badaniami.

## Relacje

- Jeden pacjent może mieć wiele próbek.
- Jedna próbka może być powiązana z badaniem.
- Jeden pacjent może uczestniczyć w wielu badaniach.
- Jedno badanie może dotyczyć wielu pacjentów.
- Jedno badanie może generować wiele wyników.

## Zapytanie 4ii

Testy, w których uczestniczyło więcej niż 30 pacjentów.

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

Tutaj wklej wynik z MySQL w formie tabeli.

## Zapytanie 4iii

Nazwy testów i średnia liczba wariantów dla każdego testu.

```sql
SELECT 
    t.test_type,
    ROUND(COUNT(r.result_id) / COUNT(DISTINCT t.test_id), 2) AS avg_variants
FROM tests t
LEFT JOIN results r ON t.test_id = r.test_id
GROUP BY t.test_type;
```

**Wynik:**

W projekcie wyeksportowano trzy pliki CSV:

- patients_tests.csv
- patients_both_tests.csv
- pathogenic_results.csv


4.
mysql> SELECT COUNT(*) FROM patients;
+----------+
| COUNT(*) |
+----------+
|      100 |
+----------+
1 row in set (0.06 sec)

mysql> SELECT COUNT(*) FROM samples;
+----------+
| COUNT(*) |
+----------+
|      150 |
+----------+
1 row in set (0.00 sec)

mysql> SELECT COUNT(*) FROM tests;
+----------+
| COUNT(*) |
+----------+
|      150 |
+----------+
1 row in set (0.00 sec)

mysql> SELECT COUNT(*) FROM patient_test;
+----------+
| COUNT(*) |
+----------+
|      150 |
+----------+
1 row in set (0.00 sec)

mysql> SELECT COUNT(*) FROM results;
+----------+
| COUNT(*) |
+----------+
|      500 |
+----------+
1 row in set (0.00 sec)

4.1.
mysql> SELECT 
    ->     p.patient_id,
    ->     p.name,
    ->     t.test_type,
    ->     t.test_date
    -> FROM patients p
    -> JOIN patient_test pt ON p.patient_id = pt.patient_id
    -> JOIN tests t ON pt.test_id = t.test_id
    -> ORDER BY p.patient_id, t.test_date;
+------------+-------------------+-----------+------------+
| patient_id | name              | test_type | test_date  |
+------------+-------------------+-----------+------------+
|          1 | David Miller      | WES       | 2023-01-05 |
|          1 | David Miller      | NGS-panel | 2023-10-15 |
|          2 | John Garcia       | WGS       | 2023-05-27 |
|          5 | Laura Johnson     | NGS-panel | 2023-02-22 |
|          6 | James Miller      | NGS-panel | 2023-01-16 |
|          6 | James Miller      | NGS-panel | 2023-12-20 |
|          7 | Bob Brown         | SNP Array | 2023-08-20 |
|          8 | Sarah Brown       | SNP Array | 2023-03-28 |
|          8 | Sarah Brown       | NGS-panel | 2023-10-15 |
|          8 | Sarah Brown       | WES       | 2023-10-27 |
|          9 | Jane Martinez     | SNP Array | 2023-11-13 |
|         12 | John Johnson      | NGS-panel | 2023-06-11 |
|         12 | John Johnson      | WGS       | 2023-09-22 |
|         13 | Laura Smith       | NGS-panel | 2023-11-03 |
|         15 | David Smith       | WES       | 2023-10-27 |
|         16 | Sarah Johnson     | NGS-panel | 2023-12-20 |
|         17 | Alice Jones       | NGS-panel | 2023-04-19 |
|         17 | Alice Jones       | SNP Array | 2023-07-14 |
|         18 | James Davis       | WES       | 2023-01-05 |
|         20 | John Hernandez    | WES       | 2023-02-11 |
|         20 | John Hernandez    | NGS-panel | 2023-08-06 |
|         21 | Laura Davis       | WES       | 2023-06-16 |
|         21 | Laura Davis       | WGS       | 2023-09-16 |
|         21 | Laura Davis       | WES       | 2023-10-24 |
|         21 | Laura Davis       | WES       | 2023-10-27 |
|         24 | Bob Jones         | WES       | 2023-09-17 |
|         25 | Jane Johnson      | WGS       | 2023-04-04 |
|         25 | Jane Johnson      | WES       | 2023-04-04 |
|         25 | Jane Johnson      | WGS       | 2023-05-23 |
|         25 | Jane Johnson      | NGS-panel | 2023-06-15 |
|         26 | Michael Hernandez | SNP Array | 2023-01-10 |
|         27 | John Smith        | SNP Array | 2023-04-26 |
|         27 | John Smith        | WGS       | 2023-10-04 |
|         27 | John Smith        | NGS-panel | 2023-10-08 |
|         29 | John Smith        | NGS-panel | 2023-11-13 |
|         30 | Bob Jones         | NGS-panel | 2023-01-16 |
|         30 | Bob Jones         | WGS       | 2023-04-15 |
|         30 | Bob Jones         | NGS-panel | 2023-11-03 |
|         31 | Alice Miller      | SNP Array | 2023-06-11 |
|         31 | Alice Miller      | WES       | 2023-11-01 |
|         32 | Michael Hernandez | SNP Array | 2023-01-08 |
|         32 | Michael Hernandez | NGS-panel | 2023-02-11 |
|         33 | Alice Miller      | WGS       | 2023-10-08 |
|         34 | Sarah Hernandez   | WGS       | 2023-09-16 |
|         34 | Sarah Hernandez   | SNP Array | 2023-10-24 |
|         34 | Sarah Hernandez   | WGS       | 2023-11-24 |
|         35 | Jane Brown        | WES       | 2023-02-02 |
|         35 | Jane Brown        | SNP Array | 2023-06-19 |
|         36 | Jane Jones        | NGS-panel | 2023-05-14 |
|         38 | Alice Brown       | WES       | 2023-04-23 |
|         39 | Jane Garcia       | SNP Array | 2023-05-17 |
|         39 | Jane Garcia       | SNP Array | 2023-05-19 |
|         39 | Jane Garcia       | WGS       | 2023-08-25 |
|         39 | Jane Garcia       | SNP Array | 2023-09-19 |
|         39 | Jane Garcia       | WES       | 2023-11-01 |
|         40 | Alice Williams    | WGS       | 2023-05-27 |
|         41 | David Martinez    | SNP Array | 2023-01-08 |
|         41 | David Martinez    | SNP Array | 2023-01-13 |
|         42 | Jane Johnson      | NGS-panel | 2023-03-24 |
|         43 | Bob Garcia        | WES       | 2023-04-03 |
|         45 | James Garcia      | SNP Array | 2023-08-25 |
|         46 | Laura Garcia      | SNP Array | 2023-10-23 |
|         47 | John Johnson      | NGS-panel | 2023-07-12 |
|         47 | John Johnson      | SNP Array | 2023-11-03 |
|         47 | John Johnson      | WGS       | 2023-11-20 |
|         48 | Michael Williams  | WGS       | 2023-01-09 |
|         48 | Michael Williams  | SNP Array | 2023-05-15 |
|         49 | James Garcia      | WES       | 2023-11-08 |
|         50 | James Miller      | SNP Array | 2023-08-24 |
|         51 | Emma Martinez     | NGS-panel | 2023-06-12 |
|         52 | Michael Smith     | WES       | 2023-04-01 |
|         52 | Michael Smith     | WES       | 2023-04-03 |
|         53 | Emma Brown        | WES       | 2023-05-26 |
|         54 | Laura Garcia      | NGS-panel | 2023-01-01 |
|         55 | Bob Miller        | NGS-panel | 2023-01-16 |
|         55 | Bob Miller        | NGS-panel | 2023-06-03 |
|         55 | Bob Miller        | SNP Array | 2023-07-01 |
|         56 | Michael Jones     | WES       | 2023-05-13 |
|         56 | Michael Jones     | SNP Array | 2023-07-01 |
|         57 | Laura Hernandez   | SNP Array | 2023-06-19 |
|         58 | Sarah Brown       | NGS-panel | 2023-02-24 |
|         58 | Sarah Brown       | SNP Array | 2023-11-13 |
|         59 | James Brown       | WGS       | 2023-05-23 |
|         59 | James Brown       | SNP Array | 2023-12-05 |
|         60 | Sarah Miller      | NGS-panel | 2023-05-25 |
|         60 | Sarah Miller      | NGS-panel | 2023-06-12 |
|         60 | Sarah Miller      | WGS       | 2023-07-14 |
|         61 | Sarah Johnson     | SNP Array | 2023-01-13 |
|         61 | Sarah Johnson     | SNP Array | 2023-12-11 |
|         62 | David Miller      | SNP Array | 2023-06-02 |
|         62 | David Miller      | WGS       | 2023-10-14 |
|         63 | Bob Martinez      | SNP Array | 2023-02-26 |
|         63 | Bob Martinez      | SNP Array | 2023-07-14 |
|         64 | David Hernandez   | WGS       | 2023-04-04 |
|         64 | David Hernandez   | NGS-panel | 2023-09-11 |
|         66 | James Miller      | SNP Array | 2023-08-20 |
|         67 | Bob Martinez      | SNP Array | 2023-07-19 |
|         67 | Bob Martinez      | SNP Array | 2023-08-25 |
|         67 | Bob Martinez      | SNP Array | 2023-10-13 |
|         68 | Laura Brown       | SNP Array | 2023-02-26 |
|         70 | James Miller      | SNP Array | 2023-07-14 |
|         71 | Michael Williams  | WGS       | 2023-01-26 |
|         72 | Bob Miller        | NGS-panel | 2023-08-04 |
|         73 | Michael Jones     | NGS-panel | 2023-06-12 |
|         73 | Michael Jones     | SNP Array | 2023-06-19 |
|         74 | Bob Brown         | WES       | 2023-09-04 |
|         75 | Bob Brown         | NGS-panel | 2023-02-22 |
|         75 | Bob Brown         | SNP Array | 2023-12-11 |
|         76 | Bob Smith         | WES       | 2023-08-24 |
|         77 | Alice Johnson     | NGS-panel | 2023-02-24 |
|         78 | Jane Smith        | SNP Array | 2023-02-13 |
|         78 | Jane Smith        | SNP Array | 2023-06-02 |
|         78 | Jane Smith        | NGS-panel | 2023-07-26 |
|         79 | David Williams    | SNP Array | 2023-04-12 |
|         79 | David Williams    | NGS-panel | 2023-06-12 |
|         80 | James Smith       | WGS       | 2023-09-28 |
|         81 | James Hernandez   | NGS-panel | 2023-10-15 |
|         83 | James Brown       | WES       | 2023-09-17 |
|         86 | Emma Brown        | WES       | 2023-05-26 |
|         86 | Emma Brown        | SNP Array | 2023-09-03 |
|         87 | David Miller      | WES       | 2023-03-10 |
|         87 | David Miller      | WES       | 2023-10-01 |
|         88 | Jane Johnson      | NGS-panel | 2023-01-01 |
|         88 | Jane Johnson      | WGS       | 2023-01-09 |
|         88 | Jane Johnson      | SNP Array | 2023-05-03 |
|         88 | Jane Johnson      | SNP Array | 2023-09-22 |
|         89 | David Johnson     | NGS-panel | 2023-02-14 |
|         89 | David Johnson     | WES       | 2023-12-08 |
|         90 | John Smith        | WGS       | 2023-01-09 |
|         90 | John Smith        | WGS       | 2023-05-27 |
|         90 | John Smith        | SNP Array | 2023-05-28 |
|         90 | John Smith        | WGS       | 2023-07-14 |
|         90 | John Smith        | SNP Array | 2023-07-19 |
|         91 | Emma Jones        | SNP Array | 2023-02-20 |
|         91 | Emma Jones        | WES       | 2023-06-26 |
|         91 | Emma Jones        | NGS-panel | 2023-10-15 |
|         92 | Alice Jones       | NGS-panel | 2023-02-11 |
|         94 | Emma Miller       | NGS-panel | 2023-06-03 |
|         94 | Emma Miller       | NGS-panel | 2023-06-15 |
|         95 | Laura Jones       | NGS-panel | 2023-04-06 |
|         95 | Laura Jones       | SNP Array | 2023-10-02 |
|         96 | Alice Miller      | NGS-panel | 2023-04-28 |
|         96 | Alice Miller      | WES       | 2023-05-26 |
|         96 | Alice Miller      | WGS       | 2023-09-12 |
|         97 | Michael Martinez  | SNP Array | 2023-04-12 |
|        100 | Bob Garcia        | WGS       | 2023-02-18 |
|        100 | Bob Garcia        | NGS-panel | 2023-08-04 |
|        100 | Bob Garcia        | WGS       | 2023-08-25 |
|        100 | Bob Garcia        | WGS       | 2023-09-22 |
|        100 | Bob Garcia        | WGS       | 2023-09-28 |
+------------+-------------------+-----------+------------+
150 rows in set (0.00 sec)

4.2.
mysql> SELECT 
    ->     p.patient_id,
    ->     p.name,
    ->     t.test_type,
    ->     t.test_date
    -> FROM patients p
    -> JOIN patient_test pt ON p.patient_id = pt.patient_id
    -> JOIN tests t ON pt.test_id = t.test_id
    -> ORDER BY p.patient_id, t.test_date
    -> INTO OUTFILE '/var/lib/mysql-files/patients_tests.csv'
    -> FIELDS TERMINATED BY ','
    -> ENCLOSED BY '"'
    -> LINES TERMINATED BY '\n';
Query OK, 150 rows affected (0.06 sec)

4.3.
mysql> SELECT 
    ->     p.patient_id,
    ->     p.name
    -> FROM patients p
    -> JOIN patient_test pt ON p.patient_id = pt.patient_id
    -> JOIN tests t ON pt.test_id = t.test_id
    -> WHERE t.test_type IN ('SNP Array', 'WGS')
    -> GROUP BY p.patient_id, p.name
    -> HAVING COUNT(DISTINCT t.test_type) = 2
    -> INTO OUTFILE '/var/lib/mysql-files/patients_both_tests.csv'
    -> FIELDS TERMINATED BY ','
    -> ENCLOSED BY '"'
    -> LINES TERMINATED BY '\n';
Query OK, 9 rows affected (0.01 sec)

4.4.
mysql> SELECT 
    ->     p.patient_id,
    ->     p.name,
    ->     t.test_type,
    ->     r.variant,
    ->     r.position,
    ->     r.impact
    -> FROM patients p
    -> JOIN patient_test pt ON p.patient_id = pt.patient_id
    -> JOIN tests t ON pt.test_id = t.test_id
    -> JOIN results r ON t.test_id = r.test_id
    -> WHERE t.test_type = 'SNP Array'
    ->   AND r.impact = 'Pathogenic'
    -> ORDER BY p.patient_id
    -> INTO OUTFILE '/var/lib/mysql-files/pathogenic_results.csv'
    -> FIELDS TERMINATED BY ','
    -> ENCLOSED BY '"'
    -> LINES TERMINATED BY '\n';
Query OK, 34 rows affected (0.00 sec)

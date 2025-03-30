# Performance

## Tip 1: Attribute indexieren
**Problem ohne Index**: Ein Full-Table-Scan wird durchgeführt, wenn kein Index vorhanden ist. \
Beispiel:
```sql
-- Ohne Index
SELECT customer_id, customer_name FROM customers WHERE customer_id='140385';
EXPLAIN select customer_id, customer_name from customers where customer_id='140385';
+----+-------------+-----------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table     | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-----------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | customers | NULL       | ALL  | NULL          | NULL | NULL    | NULL |  500 |    10.00 | Using where |
+----+-------------+-----------+------------+------+---------------+------+---------+------+------+----------+-------------+
    
-- Mit Index
CREATE INDEX customer_id ON customers (customer_id);
EXPLAIN SELECT customer_id, customer_name FROM customers WHERE customer_id='140385';
+----+-------------+-----------+------------+------+---------------+-------------+---------+-------+------+----------+-------+
| id | select_type | table     | partitions | type | possible_keys | key         | key_len | ref   | rows | filtered | Extra |
+----+-------------+-----------+------------+------+---------------+-------------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | customers | NULL       | ref  | customer_id   | customer_id | 13      | const |    1 |   100.00 | NULL  |
+----+-------------+-----------+------------+------+---------------+-------------+---------+-------+------+----------+-------+
```
Der Index verbessert die Performance, da nur der relevante Datensatz abgefragt wird.


## Tip 2 : UNIONS verwenden
**Problem mit "OR"**: Wenn "OR" in der `WHERE`-Klausel verwendet wird, kann dies zu einem Full-Table-Scan führen. \
Beispiel:
```sql
-- Schlechte Abfrage mit "OR"
SELECT * FROM students WHERE first_name LIKE 'Ade%' OR last_name LIKE 'Ade%';

-- Bessere Lösung mit UNION
SELECT * FROM students WHERE first_name LIKE 'Ade%' 
UNION ALL 
SELECT * FROM students WHERE last_name LIKE 'Ade%';
```
Der Einsatz von `UNION` führt zu effizienteren Abfragen, indem der Scan nur für relevante Daten erfolgt.

## Tip 3: Wildcards vermeiden
**Problem mit führendem `%`**: Führende Wildcards verhindern die Nutzung von Indizes und verlangsamen die Suche. \
Beispiel:
```sql
-- Schlechte Abfrage mit führendem Wildcard
SELECT * FROM students WHERE first_name LIKE '%Ade';
```

## Tip 4: Full-Text-Searches nutzen
**Problem mit Wildcards**: Wildcards (z. B. `%Ade%`) sind ineffizient, insbesondere bei großen Datenmengen. \
Beispiel:
```sql
-- Mit Full-Text-Suche
ALTER TABLE students ADD FULLTEXT (first_name, last_name);
SELECT * FROM students WHERE MATCH(first_name, last_name) AGAINST ('Ade');
mysql> explain Select * from students where match(first_name, last_name) AGAINST ('Ade');
+----+-------------+----------+------------+----------+---------------+------------+---------+-------+------+----------+-------------------------------+
| id | select_type | table    | partitions | type     | possible_keys | key        | key_len | ref   | rows | filtered | Extra                         |
+----+-------------+----------+------------+----------+---------------+------------+---------+-------+------+----------+-------------------------------+
|  1 | SIMPLE      | students | NULL       | fulltext | first_name    | first_name | 0       | const |    1 |   100.00 | Using where; Ft_hints: sorted |
+----+-------------+----------+------------+----------+---------------+------------+---------+-------+------+----------+-------------------------------+
```
Die Full-Text-Suche ist schneller und präziser als Wildcards.

## Tip 5: Datenbank-Design
**Problem mit unnötigen Daten und NULL-Werten**: Eine schlecht designte Datenbank kann die Performance beeinträchtigen. \
Im Wesentlichen sollten Sie auf folgende Dinge achten:
1. Tabellen normalisieren / keine redudanten Daten <- ausser, Sie wissen was Sie tun
2. Geschickte Datentypen verwenden
3. NULL-Werte vermeiden
4. Zu viele Spalten(Attribute) im Design vermeiden
5. Bei SQL-Joins vorsichtig sein (nur die Tabellen einbeziehen, die auch nötig sind) - nur Daten ausgeben die nötig sind (keine "Select *")

Beispiel:
```sql
-- Schlechte Designpraxis: Zu viele Spalten und NULL-Werte
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    address VARCHAR(100) NULL, -- NULL-Werte vermeidbar
    phone_number VARCHAR(15) NULL -- NULL-Werte vermeidbar
);

-- Besser: Reduziere Spalten und vermeide NULL-Werte
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    phone_number VARCHAR(15)
);
```
Ein sauberes Datenbankdesign ohne unnötige Spalten und NULL-Werte verbessert die Performance und Wartbarkeit.

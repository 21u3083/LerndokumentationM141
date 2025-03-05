# Indexierung
Die Indexierung dient dazu, den Zugriff auf Daten zu beschleunigen und somit Abfragen zu optimieren. \
Ein Index funktioniert ähnlich wie ein Inhaltsverzeichnis in einem Buch: Statt die gesamte Tabelle nach einem Wert zu durchsuchen, kann mithilfe des Indexes gezielt auf die relevanten Daten zugreifen.

## Primärindex (PRIMARY KEY)  
- Wird automatisch für eine `PRIMARY KEY`-Spalte erstellt.  
- Jeder Wert im Index muss eindeutig sein.  
- Dieser Index wird verwendet, um Zeilen effizient zu identifizieren.  

Beispiel:  
```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(255)
);
```
Ein Index wird automatisch auf `customer_id` erstellt, da sie der `PRIMARY KEY` ist.


## Sekundärindex (INDEX oder KEY)
- Wird für schnelle Suchabfragen in nicht-primären Spalten verwendet.  
- Ermöglicht eine beschleunigte Suche, ohne dass die gesamte Tabelle durchsucht wird.  

Beispiel: 
```sql
CREATE INDEX idx_email ON customers (email);
```
Dieser Index optimiert Abfragen, die die `email`-Spalte durchsuchen:  
```sql
SELECT * FROM customers WHERE email = 'enea@example.com';
```

## Eindeutiger Index (UNIQUE INDEX) 
- Ähnlich wie ein `INDEX`, stellt aber sicher, dass alle Werte in der Spalte einzigartig sind.  
- Verhindert doppelte Einträge.  

Beispiel: 
```sql
CREATE UNIQUE INDEX idx_unique_email ON customers (email);
```
Jetzt kann kein zweiter Kunde mit derselben E-Mail-Adresse hinzugefügt werden.


## Volltextindex (FULLTEXT INDEX)
- Wird für Textsuche in `TEXT`- oder `VARCHAR`-Spalten verwendet.  
- Ermöglicht eine effiziente Suche nach Wörtern in grossen Textfeldern.  

Beispiel:
```sql
CREATE TABLE articles (
    id INT PRIMARY KEY,
    title VARCHAR(255),
    content TEXT,
    FULLTEXT (title, content)
);
```
Suche nach bestimmten Wörtern:  
```sql
SELECT * FROM articles WHERE MATCH(title, content) AGAINST ('MySQL Index');
```


## Zusammengesetzter Index (Composite Index)
- Besteht aus mehreren Spalten.  
- Verbessert Abfragen, die mehrere Bedingungen nutzen.  

Beispiel:
```sql
CREATE INDEX idx_name_email ON customers (name, email);
```
Optimiert folgende Abfrage:  
```sql
SELECT * FROM customers WHERE name = 'Max' AND email = 'max@example.com';
```


## Anwendung von Indexen

| Index-Typ        | Anwendung |
|------------------|-----------|
| **PRIMARY KEY**  | Eindeutige Identifizierung von Datensätzen |
| **INDEX**        | Häufige Suchabfragen auf einer Spalte |
| **UNIQUE INDEX** | Verhindert doppelte Werte |
| **FULLTEXT**     | Suche in grossen Textfeldern |
| **Composite**    | Optimierung von Abfragen mit mehreren Bedingungen |


# Datentypen

Es gibt verschiedene Datentypen, um Daten in einer Datenbank zu speichern. Hierzu zählen numerische Datentypen, Zeichenketten, Datums- und Zeittypen sowie spezielle Datentypen wie JSON und ENUM.

## JSON
Mit dem JSON-Datentyp können Daten im JSON-Format gespeichert werden.  Er erlaubt die Speicherung komplexer, strukturierter Daten wie Arrays und Objekte in einer einzigen Spalte.  Der JSON-Datentyp wird oft genutzt, um semi-strukturierte Daten zu speichern.

### Beispiel:
```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    preferences JSON
);

INSERT INTO users (id, name, preferences) VALUES
(1, 'Max Mustermann', '{"color": "blue", "language": "German"}');
```

Das Beispiel zeigt die Erstellung einer Tabelle `users`, die eine Spalte `preferences` vom Typ JSON enthält.  Dort ist es möglich, komplexe Daten im JSON-Format zu speichern.

## ENUM
Der ENUM-Datentyp ist ein spezieller Datentyp, der eine Liste von Werten vordefiniert. Jede Spalte, die den ENUM-Typ verwendet, kann nur einen dieser vordefinierten Werte enthalten. ENUM eignet sich gut für Felder, bei denen die Werte eine festgelegte Auswahl darstellen, wie z.B. Statuswerte oder Kategorien.

### Beispiel
```sql
CREATE TABLE orders (
    id INT PRIMARY KEY,
    status ENUM('Pending', 'Shipped', 'Delivered') NOT NULL
);

INSERT INTO orders (id, status) VALUES
(1, 'Pending'),
(2, 'Shipped'),
(3, 'Delivered');
```

In diesem Beispiel wird eine Tabelle `orders` erstellt, die eine `status`-Spalte vom Typ ENUM enthält. Diese Spalte kann nur einen der drei vordefinierten Werte (`Pending`, `Shipped`, `Delivered`) enthalten.
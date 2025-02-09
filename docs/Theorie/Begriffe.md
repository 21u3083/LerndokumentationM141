# Begriffe in SQL
![SQL](../pictures/SQL.png)

## SQL
SQL steht für Structured Query Language. SQL ist eine Datenbanksprache, mit welcher Daten abgefragt, verwaltet und manipuliert werden können.
Mit SQL kann man Datenbanken erstellen sowie Daten einfügen, abfragen, aktualisieren und löschen.

## SQL DDL
Mit DDL (Data Definition Language) werden Datenstrukturen erstellt, verändert oder gelöscht. \
Hier ein paar Beispiele: 
- Tabelle erstellen
  ```sql
  CREATE TABLE Kunden (
    KundenID INT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Email VARCHAR(255),
    Geburtsdatum DATE,
    Telefonnummer VARCHAR(20)
  );
  ```
- Tabelle löschen
  ```sql
  DROP TABLE Kunden;
  ```
- Tabelle verändern
  ```sql
  -- Spalte hinzufügen
  ALTER TABLE Kunden ADD Adresse VARCHAR(255);
  -- Spaltentyp ändern
  ALTER TABLE Kunden ALTER COLUMN Telefonnummer VARCHAR(30);
  -- Spalte löschen
  ALTER TABLE Kunden DROP COLUMN Geburtsdatum;
  ```

## SQL DML
Mit DML (Data Manipulation Language) werden Daten ausgewählt, gelöscht oder erstellt. \
Hier ein paar Beispiele: 
- Daten auswählen
  ```sql
  -- Alle Spalten
  SELECT * FROM Kunden;
  -- Bestimmte Spalten
  SELECT Name, Email FROM Kunden;
  -- Spalten mit einer Bedingung filtern
  SELECT * FROM Kunden WHERE KundenID = 1;
  ```
- Daten löschen
  ```sql
  DELETE FROM Kunden WHERE KundenID = 5;
  ```
- Daten erstellen
  ```sql
  INSERT INTO Kunden (KundenID, Name, Email, Geburtsdatum, Telefonnummer)  
  VALUES (1, 'Max Mustermann', 'max@example.com', '1990-05-15', '+49123456789');
  ```



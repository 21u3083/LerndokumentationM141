# Import & Export

## Import

### SQL Dump
1. Zuerst Tabelle erstellen, in welche es importiert werden soll
   ```sql
   create database pokemon;
   ```
2. File auf die gewünschte Tabelle importieren **(Import nicht im MySQL drin machen)**
   ```sql
   -- Eignetlich so
   mysql -u root -p pokemon < /home/vagrant/Desktop/SQL/MySQLDump.sql

   -- Falls Probleme mit root user
   sudo mysql -u root -p pokemon < /home/vagrant/Desktop/SQL/MySQLDump.sql
   ```
### CSV

```sql
-- Mit SQL
LOAD DATA INFILE '/tmp/data.csv' 
INTO TABLE country  
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS

-- SQL Dump
mysqlimport --ignore-lines=1 \ # Header überspringen
            --fields-terminated-by=, \ # Feld-Begrenzungen: Komma oder Semikolons
            --local -u root -p Database \ # Zugriff auf die DB
             TableName.csv # Filename
```


## Export

### CSV File
#### Abfragen in csv Dateien exportieren
```sql
-- Export von allen Daten, von der Datenbank Pokemon
Select pokemon.pok_name from pokemon
   inner join pokemon_moves on pokemon.pok_id = pokemon_moves.pok_id
   where pokemon_moves.move_id = 7
into outfile '/var/lib/mysql-files/pokemonsWithFirePunch.csv'
FIELDS ENCLOSED BY '"'
TERMINATED BY ';'
LINES Terminated by '\n';

-- Mit Zeilen headings
(Select 'PokemonName')
UNION
(
   Select pokemon.pok_name from pokemon
      inner join pokemon_moves on pokemon.pok_id = pokemon_moves.pok_id
      where pokemon_moves.move_id = 7
   into outfile '/var/lib/mysql-files/pokemonsWithFirePunch.csv'
   FIELDS ENCLOSED BY '"'
   TERMINATED BY ';'
   LINES Terminated by '\n'
)
```
#### Ganze Datenbank in CSV exportieren
```sql
sudo mysqldump pokemon --fields-terminated-by ',' --fields-enclosed-by '"' --fields-escaped-by '\' --no-create-info --tab /var/lib/mysql-files/

-- Nur Tabelle pokemon
sudo mysqldump pokemon pokemon --fields-terminated-by ',' --fields-enclosed-by '"' --fields-escaped-by '\' --no-create-info --tab /var/lib/mysql-files/
```
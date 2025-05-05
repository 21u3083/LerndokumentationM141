# Couch DB - Backup, Import und Export

## Wichtigste Fakten

* **Dateibackup**: Kopieren der `.couch`-Dateien (nur bei gestoppter CouchDB!)
* **Replikation**: Mit `/_replicate` API – auch live und remote möglich
* **Komplettexport**: Mit `curl` + `jq` oder `couchbackup`-Tool
* **Wiederherstellung**: Per Replikation oder `curl -X POST`, bei Datei-Backup nur im Offline-Zustand
* **Wichtig**: Auch `_design`-Dokumente und `/_security` sichern

## Backup
Ein "normales warm, physical"-Backup-Verfahren bei CouchDB bedeutet: \
**warm** = Backup während CouchDB läuft (kein Shutdown) \
**physical** = Backup auf Dateiebene (nicht über API)

1. Speicherort der Daten finden
```
sudo cat /opt/couchdb/etc/default.ini | grep -i database_dir
```
Ausgabe
```
database_dir = ./data
```
2. Ordner für das Backup erstellen
   ```
   sudo mkdir /home/vagrant/couchdb_real_backup/
   ```
3. Datenbank-Dateien im laufenden Betrieb kopieren
```
sudo cp -r /opt/couchdb/data /home/vagrant/couchdb_real_backup/
```
4. Backup-Inhalt prüfen
```
sudo dir /home/vagrant/couchdb_real_backup/
```
5. CouchDB stoppen und wiederherstellen --> Testing
```
sudo systemctl stop couchdb
sudo cp -r /home/vagrant/couchdb_real_backup/* /opt/couchdb/data/
sudo systemctl start couchdb
```

## Import
Sie erhalten den Auftrag, alle Movies aus dem MySQL-Datenbank Sakila in CouchDB als Documents zu speichern. Setzen Sie diesen Schritt um! (wahrscheinlich müssen Dinge in JSON wandeln...und Python könnte hilfreich sein...)

### Voraussetzungen
1. Update und Python installieren
   ```
   sudo apt update
   sudo apt install python3 python3-pip -y
   ```
2. Python Bibliothek installieren \
   Es ist nicht Best Practise es direkt auf dem System zu erstellen aber hier nicht so schlimm.
   ```
   pip3 install --break-system-packages mysql-connector-python couchdb
   ```

### Variante 1: Komplett per Python
1. File für das Script erstellen
   ```
   sudo nano /home/vagrant/import_films.py
   ```
   Inhalt
   ```python
   import mysql.connector
   import json
   import couchdb

   # Verbindung zur MySQL-Datenbank herstellen
   mysql_conn = mysql.connector.connect(
      host="localhost",
      user="root",
      password="Root1234!",
      database="sakila"
   )

   cursor = mysql_conn.cursor(dictionary=True)
   cursor.execute("SELECT * FROM film")
   films = cursor.fetchall()

   # Verbindung zu CouchDB herstellen
   couch = couchdb.Server("http://admin:AdminCouchDB1234%21@127.0.0.1:5984")
   if 'films_db' in couch:
      db = couch['films_db']
   else:
      db = couch.create('films_db')

   # Movies in JSON-Dokumente umwandeln und in CouchDB speichern
   for film in films:
      # Umwandeln des Film-Datensatzes in ein JSON-Dokument
      film_json = json.dumps(film, default=str)
      film_dict = json.loads(film_json)  # Konvertiert den JSON-String zurück in ein Python-Objekt
      
      # Dokument in CouchDB speichern
      db.save(film_dict)

   # Verbindung schließen
   cursor.close()
   mysql_conn.close()
   ```
2. Script ausführen
   ```
   python3 /home/vagrant/import_films.py
   ```


### Variante 2: Per CSV importieren
1. CSV export von mysql erstellen
   ```sql
   Select * from film
   into outfile '/var/lib/mysql-files/filmExport.csv'
   FIELDS ENCLOSED BY '"'
   TERMINATED BY ';'
   LINES Terminated by '\n';
   ```

   ```sql
   (SELECT 
    'film_id', 
    'title', 
    'description', 
    'release_year', 
    'rental_duration', 
    'rental_rate', 
    'length', 
    'replacement_cost', 
    'rating', 
    'special_features', 
    'last_update')
   UNION
   (SELECT 
      film_id, 
      title, 
      description, 
      release_year, 
      rental_duration, 
      rental_rate, 
      length, 
      replacement_cost, 
      rating, 
      special_features, 
      last_update
   FROM film)
   INTO OUTFILE '/var/lib/mysql-files/filmExport2.csv'
   FIELDS ENCLOSED BY '"'
   TERMINATED BY ';'
   LINES TERMINATED BY '\n';
   ```
2. CSV an anderen Ort kopieren
   ```
   sudo cp /var/lib/mysql-files/filmExport2.csv /home/vagrant/
   sudo chown vagrant:vagrant /home/vagrant/filmExport2.csv
   ```
3. Per Python File in CouchDB einfügen
   ```
   sudo nano /home/vagrant/import_filmsCSV.py
   ```
   Code
   ```python
   import csv
   import couchdb

   # Verbindung zu CouchDB (Anpassen an deine Zugangsdaten)
   couch = couchdb.Server("http://admin:AdminCouchDB1234%21@127.0.0.1:5984/")

   # Zugriff auf vorhandene Datenbank
   db_name = "filmscsv_db"
   db = couch[db_name] if db_name in couch else couch.create(db_name)

   # CSV einlesen
   csv_file = "/home/vagrant/filmExport2.csv"
   with open(csv_file, newline='', encoding='utf-8') as f:
      reader = csv.DictReader(f, delimiter=';', quotechar='"')
      for row in reader:
         # Optional: Felder aufbereiten (z. B. Leerzeichen entfernen)
         cleaned_row = {k.strip(): v.strip() for k, v in row.items()}
         db.save(cleaned_row)

   print("Import abgeschlossen!")
   ```
4. 
5. Script ausführen
   ```
   python3 /home/vagrant/import_filmsCSV.py
   ```

Filme sind nun importiert:

![Mango Query](../pictures/CouchDB/MangoQueryFilme.png)
# Couch DB

## Installation
1. Repository enablen
   ```
   sudo apt update && sudo apt install -y curl apt-transport-https gnupg
    curl https://couchdb.apache.org/repo/keys.asc | gpg --dearmor | sudo tee /usr/share/keyrings/couchdb-archive-keyring.gpg >/dev/null 2>&1
    source /etc/os-release
    echo "deb [signed-by=/usr/share/keyrings/couchdb-archive-keyring.gpg] https://apache.jfrog.io/artifactory/couchdb-deb/ ${VERSION_CODENAME} main" \
        | sudo tee /etc/apt/sources.list.d/couchdb.list >/dev/null
    ```
2. Installieren
   ```
   sudo apt update
   sudo apt install -y couchdb
   ```
3. Standalone auswählen, nicht clustered
4. Magic Cookie eingeben --> `MagischerCookie1234`
5. Bind Address auf localhost belassen, weil wir es nicht öffentlich benötigen
6. Admin user erstellen --> PWD `AdminCouchDB1234!`

## Fauxton
Login unter
```
http://localhost:5984/_utils/#login
```
Username: admin \
Pwd: Das von oben

Über das Gui kann dann beliebig Datenbanken erstellen und verwalten

### Dokument hinzufügen
1. Grünen Button für Dokument hinzufügen drücken
2. Wert angeben
   Beispiel:
   ```
    {
    "_id": "00a271787f89c0ef2e10e88a0c00048b",
    "type": "movie",
    "title": "Princess Mononoke",
    "year": 1997,
    "director": "miyazaki",
    "rating": 8.4
    }
   ```
3. Dokument speichern


### Dokument abrufen Mango Query
1. Run a Query with Mango auswählen
2. Manage indexes auswählen
3. Index auf Jahre ändern, weil wir nach Jahren suchen möchten
   ```
   {
    "index": {
        "fields": [
            "year"
        ]
    },
    "name": "year-json-index",
    "type": "json"
    }
   ```
   ![Index änder](../pictures/CouchDB/MangoQuery.jpg)
4. Wieder auf Edit Query drücken
5. Mango Query ändern, dass nach dem Jahr 1997 gesucht wird und mit Run Query bestätigen
   ```
   {
    "selector": {
        "year": {
        "$eq": 1997
        }
    }
    }
   ```
   ![Mango Query abruf](../pictures/CouchDB/AbrufMangoQuery.jpg)

**Beispiel MangoQuery, für alle Zwischen 1990 und 1980**
```
{
   "selector": {
      "year": {
         "$lt": 1990,
         "$gte": 1980
      }
   }
}
```

## Konsole
Hierbei muss beachtet werden, dass das ! in URL Kodierung umgewandelt wird also zu %21

**Überprüfen ob die aktuelle Instanz läuft**
```
curl http://127.0.0.1:5984/
```
![DB Konsolen Kommandos](../pictures/CouchDB/CurlFürInstanz.jpg)

**Alle Datenbanken abrufen**
```
curl -X GET http://admin:AdminCouchDB1234%21@127.0.0.1:5984/_all_dbs
```

**Datenbank erstellen**
```
curl -X PUT http://admin:AdminCouchDB1234%21@127.0.0.1:5984/eneastestdb
```

**Datenbank löschen**
```
curl -X DELETE http://admin:AdminCouchDB1234%21@127.0.0.1:5984/eneastestdb
```

![DB Konsolen Kommandos](../pictures/CouchDB/DBCommandsKonsole.jpg)


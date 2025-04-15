# Flask App mit CouchDB

## Installation
1. Github Repo holen
   ```
   sudo git clone https://github.com/MAYnerAC/flask-couchdb-api.git
   ```
2. Verzeichnis öffnen
   ```
   cd flask-couchdb-api/flask-couchdb-api/
   ```
3. Dependencies installieren
   ```
   pip install -r requirements.txt --break-system-packages
   ```
4. config.py bearbeiten --> Passwort!
   ```
   sudo nano config.py
   ```
   Konfiguration
   ```
   import os

    COUCHDB_URL = os.getenv('COUCHDB_URL', 'http://admin:AdminCouchDB1234%21@127.0.0.1:5984/')
    DATABASE_NAME = 'movies'
   ```
5. Applikation starten
   ```
   python3 app.py
   ```
6. Im Browser öffnen
   ```
   http://localhost:5000
   ```


## Bedienung
**Film erstellen:**
![Film erstellen](../pictures/CouchDB/Flask/CreateFilm.jpg)

**Film ausgeben:**
![Film ausgeben](../pictures/CouchDB/Flask/GetFilm.jpg)
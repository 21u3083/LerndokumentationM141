# CouchDB - Security

## Authentifizierung

### Admin-User
1. Konfigurationsfile öffnen
   ```
   sudo nano /opt/couchdb/etc/local.ini
   ```
2. Unter admin Abschnitt passwort nach dem gleich schreiben
   ```
   [admins]
   ;admin = Admin1234!  
   ```
   ![Admin Passwort](../pictures/CouchDB/Admin/Passwort.jpg)
3. CouchDB neustarten
   ```
   sudo systemctl restart couchdb
   ```
4. Mit neuem Admin Passwort anmelden
   ```
   curl http://admin:Admin1234%21@127.0.0.1:5984/_up
   ```
   Ausgabe
   ```
   {"status":"ok","seeds":{}}
   ```


#### Per HTTP-API erstellen
```
HOST="http://admin:AdminCouchDB1234%21@127.0.0.1:5984"
NODENAME="_local"
curl -X PUT $HOST/_node/$NODENAME/_config/admins/enea -d '"secret"'
""
```
CouchDB neustarten
```
sudo systemctl restart couchdb
```


### Session-ID
1. Session ID abrufen mit curl
   ```
   curl -i -X POST http://localhost:5984/_session \
      -H "Accept: application/json" \
      -H "Content-Type: application/x-www-form-urlencoded" \
      -d "name=admin&password=AdminCouchDB1234!"
   ```
   ![Session ID abrufen](../pictures/CouchDB/SessionIDabrufen.jpg)
2. Überpürfen ob die Session ID korrekt ist
   ```
   curl -i -X GET http://localhost:5984/_session   
      -H "Host: localhost:5984"
      -H "Accept: application/json"
      -H "Cookie: AuthSession=YWRtaW46NjgxNzNBQkM67yMrkUKAGH3Ro9pQao0Lx7ah9Dk-t9H_VqYzMlz_nIM"
   ```
   ![Session ID überprüfen](../pictures/CouchDB/Session/SessionÜberprüfen.jpg)


#### Dokument via HTTP erstellen
```
curl -X POST http://127.0.0.1:5984/eneastestdb \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Cookie: AuthSession=YWRtaW46NjgxNzNDQTI6Btbr5m39ZR4Gz1MW17IX-YuFB6VCRN1vMqRcmdsAZ04" \
  -d '{"type":"test","message":"Hallo Welt"}'
```


### User

#### Erstellen
```
curl -X PUT http://127.0.0.1:5984/_users/org.couchdb.user:carol \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Cookie: AuthSession=YWRtaW46NjgxNzUwNzE6xUxFKqyUNbW_xMeLNLUrakRIAkTr2YhS0OEn9hTeFAg" \
  -d '{
    "name": "carol",
    "password": "pw4567",
    "roles": [],
    "type": "user"
  }'
```
![User erstellen](../pictures/CouchDB/Session/UserErstellen.jpg)

#### Abrufen
Optional für bessere ansicht jq isntallieren
```
sudo apt install jq
```

```
curl -s -X GET http://127.0.0.1:5984/_users/_all_docs?include_docs=true \
  -H "Accept: application/json" \
  -H "Cookie: AuthSession=YWRtaW46NjgxNzUwNzE6xUxFKqyUNbW_xMeLNLUrakRIAkTr2YhS0OEn9hTeFAg" \
| jq '.rows[].doc | {name, roles}'
```


## Authorisierung

### Einschränkungen

1. **Nur Admins dürfen:**

   * Datenbanken erstellen/löschen
   * Benutzer anlegen (`_users`)
   * Sicherheitsregeln (`_security`) ändern

2. **Normale Benutzer:**

   * Sehen nur eigene Daten (wenn überhaupt)
   * Haben **nur Zugriff**, wenn explizit per `_security` erlaubt

3. **Session-Cookies laufen ab** (Standard: 10 Minuten)

4. **Benutzer dürfen `_users/_all_docs` nicht sehen** (nur Admins)

5. **Rollen (`roles`) sind rein logisch** – CouchDB erzwingt nichts automatisch


### In der Praxis
DB auf einen benutzen einschränken:
```
curl -X PUT http://localhost:5984/hello-world/_security \
   -u admin:AdminCouchDB1234! \
   -H "Content-Type: application/json" \
   -d '{"admins": { "names": [], "roles": [] }, "members": { "names": ["alice"], "roles": [] } }'
```

Danach gehts nur noch mit User:
![Authorisierung](../pictures/CouchDB/Session/Authorization.png)
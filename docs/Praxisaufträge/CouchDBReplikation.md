# CouchDB - Replikation

## CouchDB mit Docker
1. Docker instalieren
   ```
   sudo apt install docker.io docker-compose -y 
   ```
2. Ordner für Docker erstellen
   ```
   sudo mkdir replikation/db/
   sudo mkdir replikation/db/aplha
   sudo mkdir replikation/db/beta
   sudo mkdir replikation/db/gamma
   sudo mkdir replikation/db/aplha/data
   sudo mkdir replikation/db/beta/data
   sudo mkdir replikation/db/gamma/data
   sudo chown -R 1001:1001 ./db/alpha ./db/beta ./db/gamma
   ```
3. Docker compose File erstellen
   ```
   sudo nano /replikation/docker-compose.yml
   ```
   Konfiguration
   ```
   version: '3'

   services:
   couchdb1:
      image: bitnami/couchdb:latest
      container_name: couchdb1
      restart: always
      ports:
         - "5984:5984"
      volumes:
         - ./db/alpha:/bitnami/couchdb
      environment:
         - COUCHDB_USER=admin
         - COUCHDB_PASSWORD=password
         - COUCHDB_SECRET=secret
         - COUCHDB_NODE_NAME=couchdb@node1
      networks:
         - couchdb-cluster
      healthcheck:
         test: ["CMD", "curl", "-f", "http://localhost:5984/_up"]

   couchdb2:
      image: bitnami/couchdb:latest
      container_name: couchdb2
      restart: always
      ports:
         - "5985:5984"
      volumes:
         - ./db/beta:/bitnami/couchdb
      environment:
         - COUCHDB_USER=admin
         - COUCHDB_PASSWORD=password
         - COUCHDB_SECRET=secret
         - COUCHDB_NODE_NAME=couchdb@node2
      networks:
         - couchdb-cluster
      healthcheck:
         test: ["CMD", "curl", "-f", "http://localhost:5984/_up"]

   couchdb3:
      image: bitnami/couchdb:latest
      container_name: couchdb3
      restart: always
      ports:
         - "5986:5984"
      volumes:
         - ./db/gamma:/bitnami/couchdb
      environment:
         - COUCHDB_USER=admin
         - COUCHDB_PASSWORD=password
         - COUCHDB_SECRET=secret
         - COUCHDB_NODE_NAME=couchdb@node3
      networks:
         - couchdb-cluster
      healthcheck:
         test: ["CMD", "curl", "-f", "http://localhost:5984/_up"]

   networks:
   couchdb-cluster:
         driver: bridge
   ```
4. Andere CouchDB stoppen & Docker starten
   ```
   sudo systemctl stop couchdb
   sudo docker-compose up
   ```
5. Alle 3 Nodes öffnen \
   http://localhost:5984/_utils/#login \
   http://localhost:5985/_utils/#login \
   http://localhost:5986/_utils/#login


## Replikation
1. Auf allen 3 dieselbe Datenbank erstellen
   ```
   curl -X PUT http://admin:password@127.0.0.1:5984/mydb
   curl -X PUT http://admin:password@127.0.0.1:5985/mydb
   curl -X PUT http://admin:password@127.0.0.1:5986/mydb
   ```
2. Seite Replikation öffnen **Auf der couchDB 1**
   ![Seite Replikation](../pictures/CouchDB/Replikation/SeiteReplikation.jpg)
3. Replikation konfigurieren \
   couchDB 1 --> couchDB 2
   ![Konfiguration1to2](../pictures/CouchDB/Replikation/Konfiguration1to2.jpg)
   couchDB 2 --> couchDB 1
   ![Konfiguration2to1](../pictures/CouchDB/Replikation/Konfiguration2to1.jpg)
   couchDB 1 --> couchDB 3
   ![Konfiguration1to3](../pictures/CouchDB/Replikation/Konfiguration1to3.jpg)
   couchDB 3 --> couchDB 1
   ![Konfiguration3to1](../pictures/CouchDB/Replikation/Konfiguration3to1.jpg)

So wird alles von allen repliziert:
![Übersicht alle Replikationen](../pictures/CouchDB/Replikation/UebersichtAlleReplikationen.jpg)
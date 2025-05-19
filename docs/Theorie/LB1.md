# Lernziele LB1

## Theorie
Jede/r Lernende

### MySQL


#### kann in eigenen Worten die Entstehung von Datenbanken erläutern

#### kann die Abkürzung ACID erklären
[ACID](/Theorie/Begriffe.md?id=acid)

#### kann die Abkürzungen ACID, BASE und CAP einordnen
[ACID](/Theorie/Begriffe.md?id=acid) \
[BASE](/Theorie/Begriffe.md?id=base) \
[CAP](/Theorie/Begriffe.md?id=cap-theorem) 

#### kann NOSql-Datenbanken mit relationalen Datenbanken vergleichen, und dabei Vor- und Nachteile erklären

**Grundlegender Unterschied**

| Aspekt      | Relationale Datenbanken (SQL)                   | NoSQL-Datenbanken                                                                      |
| ----------- | ----------------------------------------------- | -------------------------------------------------------------------------------------- |
| Datenmodell | Tabellen mit Zeilen und Spalten, feste Schemata | Verschiedene Modelle (Dokumente, Schlüssel-Wert, Graphen, Spalten) ohne starres Schema |
| Struktur    | Stark strukturiert, Schema vorab definiert      | Flexibel, Schema kann dynamisch sein                                                   |
| Sprache     | SQL (Structured Query Language)                 | Unterschiedliche Abfragesprachen (je nach Typ)                                         |

**Vorteile relationaler Datenbanken**

1. **ACID-Eigenschaften** garantieren Datenkonsistenz, Integrität und Zuverlässigkeit
2. **Etabliert und gut dokumentiert**, grosse Community, viele Tools
3. **Komplexe Abfragen** mit Joins und Transaktionen einfach realisierbar
4. Ideal für Anwendungen mit **stark strukturierten Daten** und komplexen Beziehungen (z.B. Finanzsysteme)

**Nachteile relationaler Datenbanken**

1. **Skalierung meist vertikal** (auf einem Server) → bei grossen Datenmengen oft teuer und komplex
2. **Schemata sind starr**, Änderungen am Schema sind oft aufwändig
3. Nicht optimal bei sehr unstrukturierten oder sich schnell ändernden Daten

**Vorteile von NoSQL-Datenbanken**

1. **Hohe Flexibilität:** kein festes Schema → gut für sich schnell ändernde oder unstrukturierte Daten
2. **Horizontale Skalierbarkeit:** einfache Verteilung der Daten auf viele Server (Sharding)
3. Oft besser geeignet für **Big Data** und Echtzeitanalysen
4. Verschiedene Typen passen sich an unterschiedliche Anwendungsfälle an (z.B. Graph DB für Netzwerkdaten, Dokumenten DB für JSON-artige Daten)

**Nachteile von NoSQL-Datenbanken**

1. Oft **kein vollständiges ACID**, stattdessen eventual consistency → kann Datenkonsistenz beeinträchtigen
2. Abfragen und Transaktionen meist weniger mächtig oder komplex als in SQL-Datenbanken
3. Weniger standardisiert, vielfältige Systeme → manchmal steilere Lernkurve
4. Kleinere Community und weniger etablierte Tools (je nach NoSQL-System)


#### kann in eigenen Worten die Problematik bei ACID und Transaktionen beschreibe
Aufgrund von ACID und Transaktion, also etwas wird ganz ausgeführt oder gar nicht, besteht häufig das Problem, dass die Datenbank langsamer wird.

#### kann anhand von einem Beispiel die vier Anomalien bei ACID/Transaktionen herleiten

| Anomalie             | Erklärung                                                                                                                                 | Beispiel                                                                                                                                                           |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Dirty Reads**      | Eine Transaktion kann keine Änderungen sehen, die von einer anderen Transaktion noch nicht bestätigt wurden.                              | T1 zieht 100€ ab, hat aber noch nicht bestätigt. T2 liest den Kontostand mit Abzug. T1 bricht ab. T2 sieht falschen Wert.                                          |
| **Repeatable Reads** | Wenn eine Transaktion eine Zeile zweimal liest, bleibt der Wert gleich, selbst wenn eine andere Transaktion ihn zwischenzeitlich ändert.  | T2 liest Kontostand 1000€. T1 zieht 200€ ab und bestätigt. T2 liest wieder, der Wert bleibt bei 1000€. Die Änderung von T1 wird nicht gesehen.                     |
| **Phantom Reads**    | Neue Datensätze, die in einer anderen Transaktion eingefügt werden, könnten sichtbar werden, wenn eine erneute Abfrage durchgeführt wird. | T2 liest alle Konten mit mehr als 500€. T1 fügt neues Konto mit 600€ hinzu und bestätigt. T2 liest erneut und sieht plötzlich mehr Zeilen.                         |
| **Lost Update**      | Zwei Transaktionen überschreiben sich gegenseitig Änderungen, sodass eine Änderung verloren geht.                                         | T1 liest Kontostand 1000€, zieht 100€ ab und schreibt 900€. T2 liest auch 1000€, zieht 50€ ab und schreibt 950€. Das Ergebnis ist 950€, 100€ von T1 sind verloren. |

#### kann die Storage Engines InnoBD und MyISAM in eigenen Worten beschreiben
[Storage Engines](/Theorie/StorageEngines.md)

#### weiss wo die Log-Files für MySQL gespeichert werden und kann die Log-Dateien betrachten
Logs von MySQL werden unter `/var/log/mysql/`. [Man kann auch eigene Logs definieren](../Praxisaufträge/MySQLKonfiguration.md?id=protokollierung-langsamer-abfragen-aktivieren)

#### kann die Begriffe "cold, warm, hot"-Backup erklären und vergleichen
[Backup](/Theorie/Backup.md)

#### kann die Begriffe "logisches, physisches"-Backup erklären und vergleichen
[Backup](/Theorie/Backup.md)

#### kann den Begriff "Point-In-Time-Restore", in Bezug auf MySQL, erklären
Beim Point-In-Time-Restore (PITR) kann eine MySQL-Datenbank auf einen exakten Zeitpunkt in der Vergangenheit wiederhergestellt werden – z. B. auf 5 Minuten vor einem Fehler oder einer versehentlichen Löschung.
Dazu wird ein vollständiges Backup verwendet (z. B. mit mysqldump oder mysqlbackup) plus die Binärlogs, in denen alle Änderungen seit dem Backup gespeichert sind.
Beim Wiederherstellen wird das Backup eingespielt und anschliessend die Änderungen aus den Binärlogs bis zu einem gewünschten Zeitpunkt nachgezogen.

Beispiel:
Backup von 01:00 Uhr, Benutzer löscht aus Versehen Daten um 02:30 Uhr → PITR auf 02:29 Uhr möglich.

#### kann die möglichen Angriffsvektoren auf eine datenbankgestützte Applikation beschreiben und Gegenmassnahmen beschreiben

**Ausspähen von Benutzerkonten aus der Applikation** \
Angriff: \
Technisch versierte Nutzer können Datenbankzugänge aus der Applikation extrahieren. \
Risiko: \
Direkter Zugriff auf die Datenbank mit potenziell vollen Rechten. \
Gegenmassnahme: \
Keine Passwörter in der App speichern, Zugänge mit Minimalrechten verwenden, ggf. `--skip-show-database` aktivieren.

**Ausspähen über das Netzwerk** \
Angriff: \
Unverschlüsselte Datenbankverbindungen können mitgeschnitten werden. \
Risiko: \
Klartext-Passwörter und Daten sind für Angreifer sichtbar. \
Gegenmassnahme: \
TLS-Verschlüsselung aktivieren, keine offenen DB-Ports ins LAN/Internet, evtl. VPN nutzen.

**SQL-Injection** \
Angriff: \
Manipulierte Eingaben werden ungefiltert in SQL-Abfragen übernommen. \
Risiko: \
Datenklau, -manipulation oder vollständige Kontrolle über die DB. \
Gegenmassnahme: \
Prepared Statements oder Stored Procedures einsetzen, Eingaben validieren (Whitelist).

**Server-Hacking über offenen DB-Port** \
Angriff: \
MySQL-Port ist erreichbar und wird z. B. per Brute-Force angegriffen. \
Risiko: \
Komplette Übernahme des DB-Servers möglich. \
Gegenmassnahme: \
Zugriff auf lokale IPs oder VPN beschränken, starke Passwörter, Root-Zugriff nur lokal.

**Missbrauch von Views/Procedures** \
Angriff: \
Benutzer nutzen Metadaten oder schlecht gesicherte Prozeduren aus. \
Risiko: \
Einsicht in interne Logik oder unberechtigter Datenzugriff. \
Gegenmassnahme: \
Zugriff über Views/Procedures steuern, Rechte sorgfältig vergeben, Metadatenzugriff verhindern.

#### kennt die Anwendungsfälle von Views und Stored Procedures
**Views:** \
Virtuelle Tabellen basierend auf SELECT-Abfragen. \
Anwendungsfälle: \
Vereinfachung komplexer Abfragen \
Zugriffsbeschränkung auf bestimmte Daten \
Wiederverwendbare Abfragen für Reports

**Stored Procedures** \
Gespeicherte SQL-Programme mit Logik und Parametern. \
Anwendungsfälle: \
Kapselung von Geschäftslogik (z. B. Insert, Update) \
Automatisierung und Transaktionen \
Performance durch weniger Netzwerklast

#### kennt den Anwendungsfälle von Triggers
Automatisch ausgeführter Code bei INSERT, UPDATE oder DELETE. \
Anwendungsfälle:
- Daten prüfen oder korrigieren
- Änderungen protokollieren (Auditing)
- Felder automatisch berechnen (z. B. Zeitstempel)
- Tabellen synchron halten
- Geschäftsregeln durchsetzen

#### kann die beiden wichtigsten Werte bei einer EXPLAIN-Ausgabe interpretieren und erläutern
Mit dem explain Befehl in MySQL, kann man eine Query durchführen, bei welcher man dann nachher die 'Statistiken' sieht.
Die wichtigsten Werte sind:
1. type \
   Gibt an, wie effizient der Join oder Tabellenzugriff ist.
   - ALL = Full Table Scan (am schlechtesten)
   - index = Scan über ganzen Index
   - range = Zugriff auf bestimmten Bereich
   - ref / eq_ref = Zugriff über Schlüssel (gut)
   - const / system = Zugriff auf 1 Zeile (sehr gut)
2. rows \
   Zeigt die geschätzte Anzahl an Zeilen, die für diesen Schritt gelesen werden. Je kleiner, desto besser für die Performance. Hoher Wert → möglicher Optimierungsbedarf (z. B. fehlender Index)

#### kann die Begriffe "Sharding" und "Replication" in eigenen Worten erklären
**Sharding:** \
Bei Sharding geht es darum, dass man ein grosses System aufteilt in mehrer kleinere Systeme. Die einzelnen Systeme sollen nun die Aufgaben unter sich aufteilen und einzelne, spezialisierte Aufgaben ausführen. Man könnte sich zum Beispiel folgendes vorstellen:
- Unsere Applikation besteht aus den Tabellen/Collection A,B,C
- Wir erstellen 3 Nodes mit identischer Installation und verteilen die Tabellen auf die drei Nodes
Damit wir nun unserer Applikation beibringen können, wo auf welchem System welche Daten liegen, müssen wir die Daten "sharden". Dies kann man mittels
- Hashes oder Algorithmen (wir definieren welche Hashes auf welchem System liegen)
- Ranges (wir definieren Datenpools und definieren die Nodes dazu)

erreichen.

**Replication** \
Bei der Replikation ist es, nun im Gegensatz zum Sharding, so, dass man mehrere Nodes "gleich" ausstattet und man die gesamte Arbeitslast auf die Nodes verteilt. Bei CouchDB ist Replikation bereits Teil der Implementierung (sogenannte Replicas).

#### kann Replikation, Active/Passive- & Active/Read-Pools Cluster bei MySQL in eigenen Worten beschreiben
MySQL-Replikation ist ein Mechanismus, bei dem Änderungen einer Datenbank (z. B. Inserts, Updates, Deletes) automatisch auf eine oder mehrere Kopien (Replicas) übertragen werden. Die Basis dafür sind die Binary Logs, in denen alle Transaktionen der Hauptdatenbank (Source) aufgezeichnet werden. Diese Logs werden dann von den Replicas verarbeitet, um den gleichen Zustand herzustellen. \
Es gibt verschiedene Arten der Replikation:
- Active/Passive:\
  Alle Zugriffe passieren auf einer Instanz, die Änderungen werden an Replicas weitergegeben (z. B. für Backups oder Reporting).
- Active/Read Pool: \
  Schreibzugriffe gehen an die Hauptinstanz, Lesezugriffe an die Replicas – das entlastet die Hauptinstanz bei vielen Leseanfragen.

Je nach Einsatz kann die Replikation unterschiedlich konfiguriert werden (z. B. statement- oder row-basiert).

#### kann die Anwendung von Proxies bei Datenbank-Clustern erklären
Hat man nun einen Cluster in Betrieb genommen, benötigt man eine Komponente welche das Routing der Connections und Queries übernimmt. Bei MySQL übernimmt das eine Proxy-Komponente. \
Proxies sind (wie bei Reverse Proxies bei Webservern) typischerweise leichtgewichtige Applikationen (muss so sein) die zwischen den Clients und den Servern stehen. Man könnte diese auch als Man-in-the-Middle-Applikationen bezeichnen.

#### kann verschiedene Proxy-Werkzeuge benennen
1. ProxySQL \
   ProxySQL ist ein sehr leistungsfähiger und flexibler Datenbankproxy, der ursprünglich als Open-Source-Projekt gestartet wurde und mittlerweile auch professionellen Support bietet. Es wurde mit dem Ziel entwickelt, Skalierbarkeit, Hochverfügbarkeit und eine gewisse Unabhängigkeit vom verwendeten Datenbankmanagementsystem zu ermöglichen. Zu den wichtigsten Funktionen zählen:
   - Connection Multiplexing (Reduktion der offenen Verbindungen zur Datenbank)
    - Regelbasierte Steuerung von SQL-Queries
     - Query-Caching, Rewriting und Mirroring
     - Konfigurierbar ohne Neustart
     - Geringer Speicherverbrauch
     - REST-API zur Automatisierung

  ProxySQL ist so ressourcenschonend, dass es oft direkt auf Applikationsservern betrieben wird, um Netzwerklatenz zu minimieren.

1. MariaDB MaxScale \
   MaxScale ist ein Proxy, der speziell für MariaDB entwickelt wurde, aber auch für MySQL einsetzbar ist. Er ermöglicht sowohl Lastverteilung als auch Hochverfügbarkeit. Ein herausragendes Merkmal ist die Modularität durch Plugins, wodurch Unternehmen wie z. B. AirBnB eigene Erweiterungen integrieren konnten (z. B. Anbindung an Apache Kafka zur verbesserten Laststeuerung). Auch MaxScale bietet eine REST-API und eignet sich besonders gut für grosse, skalierbare Infrastrukturen.

2. MySQL-Router \
   MySQL-Router ist ein offiziell von Oracle entwickeltes Proxy-Tool für MySQL. Es ist im Vergleich zu den anderen vorgestellten Werkzeugen das „kleinste Projekt“, bietet aber dennoch zentrale Funktionen wie:
      - Failover
     - Load Balancing
     - Erweiterbarkeit über Plugins
     - REST-API für Automatisierung
  MySQL-Router ist vor allem dann interessant, wenn man in einem Oracle-nahen MySQL-Ökosystem bleibt und eine schlanke Lösung benötigt.

1. HAProxy \
   HAProxy ist ein generischer Open-Source-Proxy für TCP- bzw. HTTP-Verbindungen und kann auch für SQL-Datenbankverbindungen eingesetzt werden. Obwohl er nicht speziell für MySQL entwickelt wurde, kann er entsprechend erweitert werden (z. B. durch Open-Source-Projekte auf GitHub). Aufgrund seiner Komplexität und der Tatsache, dass er weniger „Datenbank-intelligent“ ist als die anderen genannten Tools, wurde er im beschriebenen Szenario jedoch nicht weiterverfolgt.


### CouchDB

#### kann die Vor- und Nachteile von CouchDB als DBMS in eigene Worten aufzählen und erkläutern

**Vorteile**

1. **Flexibel & Schemafrei**
   Statt wie bei relationalen Datenbanken ein festes Tabellenschema vorzugeben, speichert CouchDB Daten in **JSON-Dokumenten**. Diese Dokumente können völlig unterschiedliche Felder und Strukturen haben – das macht die Datenbank extrem flexibel für sich ändernde Anforderungen.

2. **Leicht skalierbar**
   CouchDB ist so gebaut, dass es sich leicht **horizontal skalieren** lässt – also über mehrere Server hinweg. Das ist besonders nützlich, wenn Datenmengen oder Nutzerzahlen wachsen.

3. **Gut für verteilte Systeme**
   CouchDB verwendet eine eingebaute **Replikation**, um Daten automatisch zwischen mehreren Knoten zu synchronisieren. Es eignet sich dadurch hervorragend für verteilte Umgebungen – z. B. für Offline-fähige Apps oder Systeme mit mehreren Standorten.

4. **Effizient bei grossen Datenmengen**
   Durch die Dokumentenstruktur und das **MapReduce-basierte Abfragekonzept** kann CouchDB auch mit grossen Datenmengen effizient umgehen – insbesondere bei Leseoperationen.

5. **RESTful API**
   CouchDB lässt sich vollständig über eine **HTTP-basierte API** ansprechen. Das macht die Integration mit Web-Anwendungen und Microservices sehr einfach.


**Nachteile**

1. **Weniger geeignet für komplexe Abfragen**
   CouchDB unterstützt **keine komplexen Joins** wie bei relationalen Datenbanken. Daten müssen oft mehrfach gespeichert (denormalisiert) werden, was zu Inkonsistenzen führen kann, wenn man das nicht gut plant.

2. **Eingeschränkter Query-Support**
   Die Abfrage-Möglichkeiten sind begrenzt und **ungewohnt für SQL-Entwickler**, da CouchDB auf MapReduce-Views oder Mango Queries setzt – das ist weniger intuitiv und kann bei komplexen Datenanalysen hinderlich sein.

3. **Konsistenz statt sofortiger Aktualität**
   CouchDB ist auf **Eventually Consistent** ausgelegt. In verteilten Systemen kann es daher passieren, dass Daten **nicht sofort synchron** sind – was für bestimmte Anwendungen problematisch sein kann.

4. **Performance bei Schreiboperationen**
   CouchDB verwendet ein Append-only-Dateisystem, was zwar sicher, aber **bei vielen Updates und Deletes langsamer** sein kann. Eine regelmässige Kompaktierung (Cleanup) ist notwendig.

5. **Eingeschränkte Transaktionen**
   CouchDB bietet **keine vollwertigen ACID-Transaktionen über mehrere Dokumente hinweg**. Das erschwert Anwendungen, die starke Konsistenz und atomare Operationen benötigen.


CouchDB eignet sich hervorragend für Anwendungen, die **Flexibilität**, **Offline-Fähigkeit** und **verteilte Datenhaltung** erfordern. Für klassische, relationale Geschäftslogik oder datenbanklastige Anwendungen mit vielen Verknüpfungen und Transaktionen ist es aber weniger geeignet.


#### kann verschiedene Backup-Möglickeiten für CouchDB erklären und durchführen (cold/warm/hot)
Ein "normales warm, physical"-Backup-Verfahren bei CouchDB bedeutet: \
**warm** = Backup während CouchDB läuft (kein Shutdown) \
**physical** = Backup auf Dateiebene (nicht über API)

#### kann beschreiben wie man mit Konflikten bei Replikationsvorgängen umgeht
Ein **Konflikt** entsteht, wenn **zwei Versionen desselben Dokuments** unabhängig voneinander geändert werden – z. B. auf zwei verschiedenen CouchDB-Knoten, die offline gearbeitet haben und dann wieder synchronisiert werden. Beide Änderungen sind technisch gültig, aber CouchDB kann nicht automatisch wissen, welche „die richtige“ ist.

**Wie CouchDB damit umgeht (automatisch):**

1. **CouchDB verwirft keine Versionen.**
   Beide Varianten des Dokuments werden gespeichert. Eine davon wird von CouchDB automatisch als **„Gewinner“** (winner) ausgewählt und als Hauptversion angezeigt.

2. **Die andere Version bleibt als „Konflikt-Version“ erhalten**, ist aber nur über eine spezielle Abfrage sichtbar.

3. Die Auswahl des „winners“ erfolgt **deterministisch**, z. B. basierend auf der Dokument-ID und der Revisions-ID (nicht inhaltlich!).

#### kann die Verwendung von Mango Queries erklären
Mango Queries sind eine einfache, JSON-basierte Abfragesprache in CouchDB, mit der man Dokumente filtern, sortieren und paginieren kann – ganz ohne komplexe MapReduce-Views. Sie funktionieren ähnlich wie Abfragen in MongoDB und erlauben es, Daten schnell über sogenannte Selector-Bedingungen zu durchsuchen. Für eine effiziente Nutzung sollten passende Indizes angelegt werden.

#### kann die Verwendung von Views bei CouchDB erklären
Views in CouchDB sind vordefinierte Abfragen, die mit MapReduce-Funktionen arbeiten, um Daten aus Dokumenten gezielt zu filtern, gruppieren oder zusammenzufassen. Eine View besteht aus einer Map-Funktion (zum Extrahieren von Schlüssel-Wert-Paaren) und optional einer Reduce-Funktion (zur Aggregation, z. B. zählen oder summieren). Views werden in sogenannten Design-Dokumenten gespeichert und beim ersten Aufruf berechnet, danach bei Änderungen inkrementell aktualisiert. Sie sind besonders leistungsfähig für komplexe oder wiederkehrende Auswertungen.

#### kann Umsetzungen von Joins (Begriff aus der relationalen Welt) bei CouchDB erklären
CouchDB unterstützt keine Joins wie relationale Datenbanken, da es sich um eine dokumentenorientierte NoSQL-Datenbank handelt. Stattdessen müssen Joins auf Anwendungsebene umgesetzt werden. Das bedeutet: Man holt sich zunächst ein Dokument (z. B. einen Kunden) und lädt dann separat die zugehörigen Daten (z. B. Bestellungen), oft mithilfe gemeinsamer Felder wie einer Kunden-ID. Alternativ kann man häufig benötigte verknüpfte Daten in einem einzigen Dokument zusammenfassen (denormalisieren), um zusätzliche Abfragen zu vermeiden – das erhöht die Lesegeschwindigkeit, erfordert aber beim Schreiben mehr Pflege.


Variante A: Auf Anwendungsebene#
1. Lade Kundendaten:
   ```
   GET /kunden/kunde_001
   ```
2. Lade alle Bestellungen des Kunden:
   ```
   POST /bestellungen/_find
    {
      "selector": { "kunde_id": "kunde_001" }
    }
   ```

Variante B: Join-ähnliche View
```
PUT /bestellungen/_design/joinview
{
  "views": {
    "kunde_bestellung": {
      "map": "function(doc) {
        if (doc.typ === 'bestellung') {
          emit(['kunde', doc.kunde_id], null);
        } else if (doc.typ === 'kunde') {
          emit(['kunde', doc._id], doc);
        }
      }"
    }
  }
}
```
```
GET /bestellungen/_design/joinview/_view/kunde_bestellung?startkey=["kunde","kunde_001"]&endkey=["kunde","kunde_001"]&include_docs=true
```



## Praxis

### MySQL

#### kann eine Tabelle mit der entsprechenden Storage Engine konfigurieren

#### kann Benutzer mit entsprechenden Berechtigungen für Tabellen und Datenbanken setzen

#### kann die gesetzten Berechtigungen für einen Benutzer anzeigen

#### kann ein DBMS konfiguieren (im Betrieb und über Config-Dateien) und gemachten Änderungen verifizieren

#### kann SQL-Dateien importieren und in SQL-Dateien exportieren

#### kann CSV-Dateien importieren und in CSV-Dateien exportieren

#### kann logische Backups mit einem Script durchführen

#### kann Import und Export (Migration von Daten) lokal und remote (gesichert) durchführen

#### kann Views, Stored Procedures und Triggers auf einer bestehenden Installation anzeigen und bearbeiten

#### kann Views, (einfache) Stored Procedures und (einfache) Triggers für eigene Projekte einsetzen

#### kann bei "einer schlechten Query" Ansätze formulieren, um die Performance zu bessern

#### kann einen Sharding-Cluster in Betrieb nehmen

#### kann einen Replication-Cluster in Betrieb nehmen




### CouchDB

#### kann CouchDB auf einem OS installieren

#### kann CouchDB grundlegend konfigurieren

#### kann Datenbanken und Dokumente erfassen, bearbeiten, löschen, suchen

#### kann Security bei CouchDB erklären und korrekt konfigurieren

#### kann Dokumente, welche in einem Konflikt stehen, suchen

#### kann einfache Mango Queries nutzen

#### kann einfache Views erstellen und nutzen

#### kann Replikationsvorgehen beschreiben

#### kann einen Multimaster-Cluster aufbauen und konfigurieren (uni- und bidirektional)

#### kann beschreiben wie man mit Konflikten bei Replikationsvorgängen umgeht
Um Konflikte sauber zu lösen, geht man folgendermassen vor:

#### 1. **Konflikte erkennen**

Man fragt gezielt nach Dokumenten mit Konflikten:

```http
GET /datenbank/_design/view/_view?conflicts=true
```

Oder direkt über die Dokument-ID:

```http
GET /datenbank/dokument_id?conflicts=true
```

#### 2. **Alle Versionen abrufen**

Man holt sich die aktuelle (aktive) Version **und** die Konflikt-Version(en) über ihre Revisions-IDs:

```http
GET /datenbank/dokument_id?rev=revision_id
```

#### 3. **Konflikt lösen**

Man entscheidet programmatisch oder manuell, **welche Version korrekt** ist (z. B. die aktuellere, oder eine zusammengeführte).

#### 4. **Verlierer-Versionen löschen**

Man „löscht“ die unerwünschten Versionen, indem man sie mit ihrer Revisions-ID als `_deleted` markiert:

```json
PUT /datenbank/dokument_id?rev=revision_id

{
  "_deleted": true
}
```

Damit gilt der Konflikt als gelöst – CouchDB behält nur noch die gewünschte Version.



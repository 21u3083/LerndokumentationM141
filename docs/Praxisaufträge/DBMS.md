# DBMS meiner Wahl - MSSQL

MSSQL ist ein von Microsoft entwickeltes relationales Datenbankmanagementsystem (RDBMS). Es wird oft für geschäftliche Anwendungen eingesetzt und bietet umfangreiche Funktionen zur Verwaltung von Datenbanken, Datenabfragen und Transaktionen. MSSQL ist bekannt für seine zuverlässige, sichere und skalierbare Leistung. Ich habe mich für MSSQL entschieden, weil ich im Betrieb bereits Erfahrungen mit MSSQL gesammelt habe.

## ACID und BASE

### ACID

MSSQL folgt dem Prinzip von [ACID](../Theorie/Begriffe.md?id=acid)
- **Atomicity**\
Entweder werden Transaktionen vollständig ausgeführt oder gar nicht.
- **Consistency**\
Nach einer [Transaktion](../Theorie/Begriffe.md?id=transaktion) befindet sich die Datenbank in einem konsistenten Zustand, was bedeutet, dass alle festgelegten Regeln und Einschränkungen eingehalten wurden und die Daten dem definierten Datenmodell entsprechen. Es gibt keine ungültigen oder fehlerhaften Daten.
- **Isolation**\
Um Konflikte zu vermeiden, werden Transaktionen isoliert voneinander ausgeführt.
- **Durability**\
Einmal abgeschlossene Transaktionen sind dauerhaft und bleiben auch bei Systemausfällen erhalten.

### BASE

MSSQL kann in speziellen Fällen auch Elemente des [BASE-Modells](../Theorie/Begriffe.md?id=base) nutzen:

- **Basically Available**\
MSSQL gewährleistet durch Funktionen wie Always On Availability Groups eine hohe Verfügbarkeit.
- **Soft state und Eventual consistency**\
In verteilten Systemen und bei Replikation kann es zu temporären Inkonsistenzen kommen, was bedeutet, dass verschiedene Kopien der Daten unterschiedliche Informationen enthalten können. Diese Inkonsistenz wird später durch die Synchronisation der Daten wieder konsistent gemacht.

## Fazit
MSSQL folgt vor allem dem ACID-Modell zur Gewährleistung der Datenintegrität. Es bietet allerdings auch Mechanismen an, die BASE ähnlich sind und in verteilten Systemen die Verfügbarkeit gewährleisten.
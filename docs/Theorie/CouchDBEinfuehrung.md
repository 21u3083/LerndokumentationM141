# Couch DB

**Warum wurde CouchDB als eine neue Art von Datenbanksystem entwickelt, und welche Vorteile bietet es gegenüber traditionellen relationalen Datenbanken?**
CouchDB wurde entwickelt, weil klassische relationale Datenbanken, wie MSSQL oder MySQL, oft zu wenig flexibel für manche Anwendungen sind. Statt fester Tabellen nutzt CouchDB flexible, schemafreie Dokumente, die besser zu manchen Anwendungen passen. Es ist leicht skalierbar, kann grosse Datenmengen effizient verarbeiten und funktioniert besonders gut in Systemen, welche aufgteilt sind.

**Was bedeutet das Konzept der „Entspannung“ (Relax) in Bezug auf CouchDB, und wie wirkt es sich auf die Entwicklung und den Betrieb von Anwendungen aus?**
„Relax“ bedeutet, dass CouchDB einfach zu nutzen ist. Entwickler müssen sich nicht mehr mit komplexen Strukturen oder starren Vorgaben herumschlagen. Die Architektur ist fehlertolerant: Wenn ein Problem auftritt, bleibt es isoliert und beeinflusst nicht das ganze System. Auch bei vielen Anfragen bleibt CouchDB stabil, indem es alle Anfragen langsam, aber zuverlässig verarbeitet.

**Wie unterscheidet sich das Datenmodell von CouchDB von dem relationaler Datenbanken, und warum ist das Konzept der „selbstenthaltenen Dokumente“ vorteilhaft?**
CouchDB speichert Daten als eigenständige Dokumente statt als verknüpfte Tabellen. Beispiel: Eine Rechnung enthält direkt alle Infos zu Verkäufer, Käufer und Produkten, statt auf separate Tabellen zu verweisen. Das erleichtert das Arbeiten mit den Daten, da weniger Abfragen nötig sind und die Datenstruktur flexibler ist.

**Welche Rolle spielt die Replikation in CouchDB, und welche Probleme kann sie im Bereich der verteilten Datenhaltung lösen?**
Replikation sorgt dafür, dass mehrere CouchDB-Datenbanken synchron bleiben. Das löst Probleme wie:
- Ausfallsicherheit: Daten werden auf mehreren Servern gespeichert. Fällt einer aus, gibt es noch Kopien.
- Lastverteilung: Anfragen können auf mehrere Server verteilt werden.
- Standortübergreifende Nutzung: Daten können z. B. zwischen Büros in verschiedenen Orten synchronisiert werden.

**Warum ist lokale Datenspeicherung in CouchDB besonders wichtig, und wie trägt sie zur Verbesserung der Benutzererfahrung bei mobilen und dezentralen Anwendungen bei?**
Wenn eine App CouchDB lokal nutzt, kann sie auch ohne Internetverbindung weiterarbeiten. Später werden die Daten automatisch mit der zentralen Datenbank synchronisiert. So bleibt die App jederzeit nutzbar auch wenn das Netz ausfällt.
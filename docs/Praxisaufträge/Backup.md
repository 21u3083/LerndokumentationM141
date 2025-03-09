# Backup

## Lokales Backup
1. Script erstellen **Variablen anpassen!**
   ```
   sudo nano /home/vagrant/Desktop/SQL/Backup.sh
   ```
   Konfiguration
   ```sh
   #!/bin/bash
    # Add the backup dir location, MySQL root password, MySQL and mysqldump location
    DATE=$(date +%d-%m-%Y)
    BACKUP_DIR="/tmp/test-backup"
    MYSQL_USER='root'
    MYSQL_PASSWORD='***'
    MYSQL=/usr/bin/mysql
    MYSQLDUMP=/usr/bin/mysqldump

    # To create a new directory in the backup directory location based on the date
    mkdir -p $BACKUP_DIR/$DATE

    # To get a list of databases
    databases=`$MYSQL -u$MYSQL_USER -p$MYSQL_PASSWORD -e "SHOW DATABASES;" | grep -Ev "(Database|information_schema)"`

    # To dump each database in a separate file
    for db in $databases; do
    echo $db
    $MYSQLDUMP --single-transaction --force --opt --skip-lock-tables --user=$MYSQL_USER -p$MYSQL_PASSWORD --databases $db | gzip > "$BACKUP_DIR/$DATE/$db.sql.gz"
    done

    # Delete the files older than 10 days
    find $BACKUP_DIR/* -mtime +10 -exec rm {} \;
   ```
2. Verzeichnis öffnens
   ```
   sudo cd /home/vagrant/Desktop/SQL/
   ```
3. Berechtigungen für das File so setzen, dass man es ausführen kann
   ```
   chmod +x Backup.sh
   ```
4. Script ausführen
   ```
   ./Backup.sh
   ```


## mydumper und myloader
### Installation

1. Update durchführen
   ```
   sudo apt update
   ```
2. Dependencies installieren
   ```
   sudo apt-get install libatomic1 -y
   sudo apt-get install libpcre3
   ```
3. VM neu starten, damit die Dependencies richtig geladen werden
3. Installieren
   ```
   wget https://github.com/mydumper/mydumper/releases/download/v0.16.1-2/mydumper_0.16.1-2.jammy_amd64.deb
   sudo dpkg -i mydumper_0.16.1-2.jammy_amd64.deb
   ```¨
4. Installation überprüfen
   ```
   mydumper --version
   ```

### MyDumper (Backup erstellen)
Beispiel Befehl um ein Backup zu erstellen
```
mydumper --threads 6 --host localhost --user root --password Root1234! --database pokemon --compress --rows="10000000" --verbose 3 --long-query-guard 999999 --no-locks --compress-protocol --outputdir /home/vagrant/Desktop/SQL/MyDumper --logfile /home/vagrant/Desktop/SQL/MyDumper/backup.log --clear
```


### MyLoader (Backup wiederherstellen)
Beispiel Befehl um ein Backup wiederherzustellen
```
myloader --threads 6 \
--host localhost \
--user root \
--password Root1234! \
--database pokemon \
--directory /home/vagrant/Desktop/SQL/MyDumper \
--queries-per-transaction 50000 \
--verbose 3 \
--compress-protocol \
--logfile /home/vagrant/Desktop/SQL/MyDumper/restore.log
```
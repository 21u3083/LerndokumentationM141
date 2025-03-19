# Migration

## Vorbereitung
1. IP ändern
   ```
   sudo /etc/netplan/nano 01-netcfg.yaml
   ```
   Konfiguration
   ```
   network:
  version: 2
  ethernets:
    eth0:
      dhcp4: false
      addresses: [10.0.2.25/24]
      gateway4: 10.0.2.1
      nameservers:
        addresses: [10.0.2.1]
   ```
2. Netzwerkplan anwenden
    ```
    sudo netplan apply
    ```


### SSH
1. Schauen, ob die andere VM gepingt werden kann
   ```
   -- IP Abrufen
   ifconfig
   ```
2. SSH verbinden
   ```
   ssh 10.0.2.25
   ```
   

### Dump durchführen
1. Named Pipe erstellen
   ```
   mkfifo namedPipe
   ```
2. MySQLDump erstellen
   ```
   mysqldump --single-transaction -u root -pRoot1234! pokemon | gzip -9 > namedPipe
   ```
3. Dump auf SSH übertragen
   ```
   ssh vagrant@10.0.2.25 'gunzip | mysql -u root -pRoot1234! pokemon_ssh' < namedPipe;
   ```
   -u: user \
   -p: Passwort (gleich mitgeben) \
   -pokemon_ssh: Ziel DB auf der der SQLDump ausgeführt werden soll.

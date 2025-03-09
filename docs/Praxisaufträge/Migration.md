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
   ssh 10.0.2.20
   ```


### Dump durchführen
```
sudo mysqldump pokemon -u root -p --fields-terminated-by ',' --fields-enclosed-by '"' --fields-escaped-by '\' --no-create-info --tab /var/lib/mysql-files/ | ssh 10.0.2.20 "mysql -u root -p --database pokemon"
```

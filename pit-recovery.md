# Teststellung 

## Szenario 

  1. Nachts um xy Uhr wird die tägliche Sicherung gemacht, mit folgenden Optionen 
     mysqldump --all-databases --delete-master-logs --master-data=2 -B > /usr/src/all-databases.sql
     
  2. Um 4.15 Uhr nachts passiert ein Unglück und eine komplette Datenbank wird gelöscht  


## Steps 

### Vorbereitung 
  1. Volles backup durchführen 
     mysqldump --all-databases --delete-master-logs --master-data=2 -B > /usr/src/all-databases.sql

### Live-Betrieb 
  1. 3 Datenbanken anlegen (z.b. testa, testb, testc) 
  2. Eine Datenbank löschen (testa) 

### Recover-Modus (Vorbereitung) 
  1. Recover.sql vorbereiten
     1. Aus letzter Sicherung bei change master herausfinden, ab wann das binlog relevant ist
        binlog-name + position 
     1. cd /var/lib/mysql 
     2. mysqlbinlog server-bin.00005  ## letzte binlog anzeigen 
     3. Stelle finden, wo fehler aufgetreten ist 
     4. hochgehen bis zur Position auf dem der Befehl ausgeführt wird (pos) 
     5. Position notieren 
     
### Recover-Modus (recover.sql) erstellen 

  1. Aus allen binlogs + position recover.sql erstellen 
  2. mysqlbinlog --stop-position=<postion_aus_vorbereitung> server-bin.00002 server-bin.00003 server-bin-00004 > /usr/src/recover.sql
  
### Einspielen der Daten 
``` 
  0. systemctl stop mariadb
     cd /var/lib/
     mv mysql mysql.bkup
     mysql_install_db --user=mysql --datadir=mysql
     systemctl start mariadb
  1. Nächtliches Backup einspielen 
     mysql < /usr/src/nightly.sql 
  2. zur Übung: sichtprüfung in mysql: show databases 
  3. mysql < /usr/src/recover.sql 
  4. zur Übung: sichtprüfung in mysql: show databases # sind alle Datenbanken wieder da. 
```

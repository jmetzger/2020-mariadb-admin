# MariaDB Komplettkurs 


   1. [Point-In-Time Recovery](pit-recovery.md)
   1. [Flashback](flashback.md)
   1. [Binary Log](binarylog.md)
   1. [Enrypting mysqldump](encryption-openssl-mysqldump.md)
   1. [Journalctl](journalctl.md)
   1. [Debugging with Logs](logs-debug.md)
   1. [PMM Percona Monitoring Management](percona-monitoring-management.md)
   1. [Collation - case sensitive](collation-case-sensitive.md) 
   1. [Encryption-data-at-rest](encryption.md) 
   1. [Galera](galera.md)
   1. [Galera Questions](galera-questions.md) 
   1. [Monitoring (What to monitor?)](monitoring.md)
  

## Architektur des MariaDB-Servers 

  * Referenz: http://schulung.t3isp.de/documents/pdfs/mysql/mysql-performance.pdf

### Storage Engine 

```
# Partition 
Bietet die Möglichkeit Daten in Scheiben zu schneiden. 
z.b. nach Jahr oder Nachname - Anfangsbuchstabe
```

### Ändern der Storage Engine 

```
# DB-weit wird nur die Default-Engine definiert 
# Aktuell seit Version ?? is das InnoDB 

# vorher InnoDB 
alter table mitarbeiter engine = MyISAM;

# Aber, es ist eine teure Operation 
# Weil 3 Schritte:
# 1. Original-Tabelle wird gesperrt (tabellenweit) - kein Schreiben möglich 
# 2. Es wird eine Kopie der Tabelle (Datenstruktur) inkl. Umstrutkturierung gemacht als Schattenkopie 
# 3. Jeder einzelne Datensatz wird rüberkopiert
# 4. Neue Tabelle gegen Original-Tabelle umgetauscht (umbenennung der Tabellen) 

# Das bitte im Wartungsfenster machen !!! 

# Sperrt, nicht sondern macht Trigger 
# Percona-Toolkit (Percona : mysqlperformanceblog.de -> gibt's https://www.percona.com/blog/
# https://www.percona.com/doc/percona-toolkit/3.0/pt-online-schema-change.html


```
 ## Installation 
 
 ```
 # Schritt repo besorgen 
 https://downloads.mariadb.org/mariadb/repositories/#mirror=cyanlink
 
 ```
 
 ### MySQL absichern 
 
 ```
 mysql_secure_installation 
 (Alle Fragen mit Y beantworten, ausser evtl. root-pw ändern) 
 # test-datenbanken werden rausgeschmissen.
 # anonyme benutzer 
 # absichern, dass root nur lokal zugreifen kann 
 ```
 
 ## mysql - client 
 
 ### Wichtige SQL-Befehle ###
 
 ```
 show databases; 
 use mysql; # in die Datenbank wechseln 
 show tables; 
 # wie ist die tabelle aufgebaut 
 describe user; 
 # mysql - client 
 select * from user \G # Spaltenweise anzeigen statt Zeilenweise 
 
 # Datenbank anlegeen
 create schema training;
 create database training; # same same  

 # Tabelle anlegen
 create table teilnehmer ( id int, vorname varchar(60), name varchar (60), primary key (id))

 # Datensatz einfügen 
 insert into teilnehmer ( id, vorname, name) values (4, 'Jochen',"Testero@test");
 
 # Tabellen - Struktur ändern (auto_increment) 
 ALTER TABLE teilnehmer  CHANGE id id int AUTO_INCREMENT;
 
 # neue struktur 
 describe teilnehmer;
+---------+-------------+------+-----+---------+----------------+
| Field   | Type        | Null | Key | Default | Extra          |
+---------+-------------+------+-----+---------+----------------+
| id      | int(11)     | NO   | PRI | NULL    | auto_increment |
| vorname | varchar(60) | YES  |     | NULL    |                |
| name    | varchar(60) | YES  |     | NULL    |                |
+---------+-------------+------+-----+---------+----------------+
3 rows in set (0.001 sec)
 
 # Einfügen ohne auto_incremement 
 MariaDB [training]> insert into teilnehmer (vorname, name) values ('Jochen',"El Mariachi");

 
 ```

## Innodb - Struktur (welche Files ?) 

```
/var/lib/mysql 
ibdata1 - allgemeine Tablespace (für Systemdaten) 
z.B. training/teilnehmer.ibd (Tablespace pro Tabelle für innodb-engine) 
ib_logfile0 # innodb log file 
ibtmp1 # tablespace für temporäre systemdaten 
ib_buffer_pool # File zum Zwischenspeichern das buffer_pool beim Runterfahren 
```

## Server System Variables 

```
mysql>show variables; 

# google 
mariadb server system variables 

```

## selinux abschalten 

```
# läuft selinux 
sestatus 

# Während der aktuellen Laufzeit auf permissive setzen (als root) 
setenforce 0 

# /etc/selinux/config 
SELIUNX=permissive 
# greift nach dem nächsten boot 
```

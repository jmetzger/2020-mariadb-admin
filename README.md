# MariaDB Komplettkurs 

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
 ```

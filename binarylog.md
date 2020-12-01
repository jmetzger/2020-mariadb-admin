# Binary Log

```
# /etc/my.cnf.d/server.cnf 
[mysqld]
log-bin 

# 
systemctl restart mariadb 

# mysql - bin log kurzfristig in session ausschalten
# ! Achtung ! Sehr gefährlich 
set sql_log_bin = 0 
create schema test1 
set sql_log_bin = 1 

# cd /var/lib/mysql
# binärlog ausgeben 
mysqlbinlog hostname-bin.00001 
```

## Nur bestimmte Datenbanken auslesen 

```
Geht nur, wenn 
use datenbank
insert into tabelle 

# nicht bei  
insert into datenbank.tabelle # das wird dann nicht rausgefiltert 

#
mysqlbinlog -vv --database=training mdb7-bin.000002

```



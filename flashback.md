# Flashback 

## Server konfiguriert 

```
/etc/my.cnf.d/server.cnf 
[mysqld] 
# binlog_format = row / binärlog 
flashback  

# systemctl restart mariadb 
```

## Daten gelöscht versehentlich 

```
mysql>use training
mysql>delete from teilnehmer
# Nachfragen, wo das binär-log steht 
mysql>show master logs 
```

## Im filesystem 

```
cd /var/lib/mysql 
# drauf achten, dass die Delete logs auftauchen 
# und ab welcher Position wir auslesen 
mysqlbinlog -vv -d training -T teilnehmer server-bin.00005
```

```
# bis hoch zur letzten Transaktion gegangen 
# at 7322218
#201202 10:32:37 server id 1  end_log_pos 7322260 CRC32 0x07da61cd 	GTID 0-1-347 trans
/*!100001 SET @@session.gtid_seq_no=347*//*!*/;
START TRANSACTION

## als Konsequenz 
## zu Dokumentation 
mysqlbinlog -vv -d training -T teilnehmer --start-position=7322218 server-bin.00005 > /usr/src/review.sql 

## als nächstes Flashback
mysqlbinlog -vv -d training -T teilnehmer --start-position=7322218 server-bin.00005 --flashback > /usr/src/flashback.sql 

```

```
# auszug 
# umgekehrte Statements aus delete wird insert 

'/*!*/;
### INSERT INTO `training`.`teilnehmer`
### SET
###   @1=5 /* INT meta=0 nullable=0 is_null=0 */
###   @2='Hans' /* VARSTRING(60) meta=60 nullable=1 is_null=0 */
###   @3='Mustermann2' /* VARSTRING(60) meta=60 nullable=1 is_null=0 */
### INSERT INTO `training`.`teilnehmer`
### SET
###   @1=4 /* INT meta=0 n
```

## einspielen 

```
mysql < /usr/src/flashback.sql 

```

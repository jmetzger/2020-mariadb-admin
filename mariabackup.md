# mariabackup 

  * mariabackup is a fork of xtrabackup, that supports windows os and encryption (xtrabackup does not !!!) 


## Schritt 1: Backup 

```
/root/.my.cnf 
[mariabackup]
user=root 
```

```
mkdir /backups 
mariabackup --backup --target=/backups/201202-1411
```

## Schritt 2: Prepare 

  * Änderungen anwenden, die noch im innodb_log_file sind 
```
mariabackup --prepare --target=/backups/201202-1411 

```

## Viele Tage später: Schritt 3: Havarie / Zurückspielen eines Backups 

```
# mariadb server stoppen  
systemctl stop mariadb 
mariabackup --copy-back --target=/backups/201202-1411 
chown -R mysql:mysql /var/lib/mysql 

# if it is does not start, check permissions ! 
systemctl start mariabackup 

```

## Ref: 

  * https://mariadb.com/kb/en/mariabackup-options/#-copy-back
  * https://mariadb.com/kb/en/full-backup-and-restore-with-mariabackup/
  * https://www.percona.com/doc/percona-xtrabackup/2.4/index.html

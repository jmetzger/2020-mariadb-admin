# mariabackup 

## Schritt 1: Backup 

```
/root/.my.cnf 
[mariabackup]
user=root 
```

mkdir /backups 
mariabackup --backup --target=/backups/201202-1411
```

## Schritt 2: Prepare 

  * Änderungen anwenden, die noch im innodb_log_file sind 
```
mariabackup --prepare --target=/backups/201202-1411 

```


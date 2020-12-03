# Performance Optimization 

## innodb_buffer_pool_size 

```
70-80% of total ram on dedicated MariaDB-Server
# what is really 
# look into :
MariaDB [(none)]> pager grep "Free buffers"
PAGER set to 'grep "Free buffers"'
MariaDB [(none)]> show engine innodb status \G 
Free buffers       59
1 row in set (0.000 sec)

MariaDB [(none)]> pager 
Default pager wasn't set, using stdout.
MariaDB [(none)]> 

# in der Konfiguration 
/etc/my.cnf.d/server.cnf
[mysqld]
innodb_buffer_pool_size = 128 G

# während der Laufzeit // Achtung ... nicht persistent 
set global innob_buffer_pool_size = 138000000000 # zahlt stimmt nicht ganz 

```

## innodb_flush_log_at_trx_commit 

```
https://mariadb.com/kb/en/innodb-system-variables/#innodb_flush_log_at_trx_commit
# best performance, while using up to 1 seconds of data on power outage
innodb_flush_log_at_trx_commit = 2 
```

## innob_flush_method 

```
innodb_flush_method = O_DIRECT
# DO not use O_DIRECT_no_fsync on centos, because XFS 
```

## innob_log_file_size 

  * https://www.percona.com/blog/2008/11/21/how-to-calculate-a-good-innodb-log-file-size/
 
## innodb_flush_neighbors 

  * bei ssd auf 0 setzen (default 1) 
  * d.h. benachbarte buffer-pages werden nicht mit auf Platte geflusht 

## skip-name-resolve 

```
# Bitte nur aktivieren, wenn ihr ausschliesslich ip - adressen bei den Usern habt 
/etc/my.cnf.d/server.cnf 
[mysqld]
skip-name-resolve 

systemctl restart mariadb 

```


## Ref

  * http://schulung.t3isp.de/documents/pdfs/mysql/mysql-performance.pdf

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

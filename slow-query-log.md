# slow query log 

  * https://mariadb.com/kb/en/slow-query-log-overview/
  
## Slow query log aktivieren 

```
# in der session 
mysql>set long_query_time = 0.000001
mysql>set global long_query_time = 0.000001
mysql>set slow_query_log = on 
mysql>set global slow_query_log = on 

```

## installieren von percona-toolkit 

```
https://www.percona.com/downloads/percona-release/
letzte Release auswählen und auf server mit wget download 
yum install wget 
cd /usr/src
wget https://downloads.percona.com/downloads/percona-release/percona-release-1.0-25/redhat/percona-release-1.0-25.noarch.rpm
rpm -ivh percona-release-1.0-25.noarch.rpm
```

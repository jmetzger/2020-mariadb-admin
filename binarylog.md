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

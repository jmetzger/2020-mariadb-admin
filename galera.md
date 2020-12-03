# Galera (Setup mariadb 10.5 mit Centos 7) 

  * http://schulung.t3isp.de/documents/pdfs/mariadb/mariadb-galera-cluster.pdf
  
## Konfigurationsdatei 

```
# /etc/my.cnf.d/zz_galera.cnf

[mysqld]
binlog_format=ROW
default-storage-engine=innodb
innodb_autoinc_lock_mode=2
bind-address=0.0.0.0
# Set to 1 sec instead of per transaction
# for better performance // Attention: You might loose data on power
innodb_flush_log_at_trx_commit=0
# Galera Provider Configuration
wsrep_on=ON
# centos7 (x86_64)
wsrep_provider=/usr/lib64/galera-4/libgalera_smm.so
# Galera Cluster Configuration
wsrep_cluster_name="cluster_jmetzger"
wsrep_cluster_address="gcomm://157.230.31.2,157.230.104.6,167.99.133.234"
# Galera Synchronization Configuration
wsrep_sst_method=rsync
```

## Starten des 1. Nodes 

```
# Zur Sicherheit, falls dieser läuft
systemctl stop mariadb 
galera_new_cluster 
```

## Dokumentation 

  * https://galeracluster.com/library/documentation/galera-status-variables.html
  

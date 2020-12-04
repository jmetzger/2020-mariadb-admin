# slow query log 

  * https://mariadb.com/kb/en/slow-query-log-overview/
  
## Slow query log aktivieren 

```
# in der session 
mysql>set long_query_time = 0.000001
mysql>set global long_query_time = 0.000001
mysql>set slow_query_log = on 
mysql>set global slow_query_log = on 

# increase verbosity
set global log_slow_verbosity='query_plan,innodb,explain';
set log_slow_verbosity='query_plan,innodb,explain';
```

## Where is ist ? 

```
show global variables like '%slow_query_log_file%';
+---------------------+---------------+
| Variable_name       | Value         |
+---------------------+---------------+
| slow_query_log_file | mdb7-slow.log |
+---------------------+---------------+
1 row in set (0.001 sec)


```


## installieren von percona-toolkit 

```
https://www.percona.com/downloads/percona-release/
letzte Release auswählen und auf server mit wget download 
yum install wget 
cd /usr/src
wget https://downloads.percona.com/downloads/percona-release/percona-release-1.0-25/redhat/percona-release-1.0-25.noarch.rpm
rpm -ivh percona-release-1.0-25.noarch.rpm

yum install percona-toolkit 
```

## auswerten 

```
cd /var/lib/mysql 
# get hitlist 
pt-query-digest server-slow.log 
```

## Evaluate in mysql

```
# Is indexed properly 

```
## Little sample, if you have sakila 

```
# table will get created without index
MariaDB [sakila]> create table actor2 as select * from actor;

MariaDB [sakila]> explain select * from actor where actor_id=2;
+------+-------------+-------+-------+---------------+---------+---------+-------+------+-------+
| id   | select_type | table | type  | possible_keys | key     | key_len | ref   | rows | Extra |
+------+-------------+-------+-------+---------------+---------+---------+-------+------+-------+
|    1 | SIMPLE      | actor | const | PRIMARY       | PRIMARY | 2       | const | 1    |       |
+------+-------------+-------+-------+---------------+---------+---------+-------+------+-------+
1 row in set (0.000 sec)

MariaDB [sakila]> explain select * from actor2 where actor_id=2;
+------+-------------+--------+------+---------------+------+---------+------+------+-------------+
| id   | select_type | table  | type | possible_keys | key  | key_len | ref  | rows | Extra       |
+------+-------------+--------+------+---------------+------+---------+------+------+-------------+
|    1 | SIMPLE      | actor2 | ALL  | NULL          | NULL | NULL    | NULL | 200  | Using where |
+------+-------------+--------+------+---------------+------+---------+------+------+-------------+

MariaDB [sakila]> show create table actor;
+-------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table | Create Table                                                                                                                                                                                                                                                                                                                                                                              |
+-------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| actor | CREATE TABLE `actor` (
  `actor_id` smallint(5) unsigned NOT NULL AUTO_INCREMENT,
  `first_name` varchar(45) NOT NULL,
  `last_name` varchar(45) NOT NULL,
  `last_update` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  PRIMARY KEY (`actor_id`),
  KEY `idx_actor_last_name` (`last_name`)
) ENGINE=InnoDB AUTO_INCREMENT=201 DEFAULT CHARSET=utf8mb4 |
+-------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.000 sec)

MariaDB [sakila]> show create table actor2;
+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table  | Create Table                                                                                                                                                                                                                                                                                                                                                                                              |
+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| actor2 | CREATE TABLE `actor2` (
  `actor_id` smallint(5) unsigned NOT NULL DEFAULT 0,
  `first_name` varchar(45) CHARACTER SET utf8mb4 NOT NULL,
  `last_name` varchar(45) CHARACTER SET utf8mb4 NOT NULL,
  `last_update` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  KEY `idx_actor_first_name_last_name` (`first_name`,`last_name`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1 |
+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.000 sec)

MariaDB [sakila]> 
```

* type ALL means -> full table scan (bad !!) 


## Ref / Docs (good read) 

  * https://www.amazon.de/Effective-Optimizing-Statements-Oracle-English-ebook/dp/B006402OBI/ref=sr_1_3?__mk_de_DE=%C3%85M%C3%85%C5%BD%C3%95%C3%91&dchild=1&keywords=oracle+mysql+indexes&qid=1607088075&sr=8-3

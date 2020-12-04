# Encryption (data-at-rest)  

## Docs
  * https://mariadb.com/resources/blog/table-and-tablespace-encryption-on-mariadb-10-1/
  * https://mariadb.com/kb/en/data-at-rest-encryption-overview/

## Create key 

```
# Befehl
openssl enc -aes-256-ctr -k mylong2016secret@keyfor35fun -P -md sha1
# Ausgabe 
salt=DADA11ACFC68F5E6
key=282FA20F44454E2D09165E8ED1D9E82CD543CF8E7CBF236E06C94C76C531758A
iv =1F880B12999A48EAFCD3FD0ACBE5475D
```

```
# write key with prefix 1; to file keyfile  
tree  mysql/
mysql/
├── encryption
│   └── keyfile

cat mysql/encryption/keyfile 
1;282FA20F44454E2D09165E8ED1D9E82CD543CF8E7CBF236E06C94C76C531758A
```

## Add settings to mariadb and restart 

```
/etc/my.cnf.d/server.cnf 
[mysqld]
#...
plugin-load-add=file_key_management
file_key_management_filename=/etc/mysql/encryption/keyfile
file_key_management_encryption_algorithm=AES_CTR

# 
systemctl restart mariadb 

```

## Was it loaded succesfully ?

```
# must appear here
show plugins 
# if not loaded will be not_installed status in here 
select information_schema.all_plugins where plugin_name like '%key%';
# you can also check the logs
journalctl -u mariadb | grep -i error | grep 'file_key'

```

## test encryption 
```
create database training;
use training;
CREATE TABLE table1
(col1 INT NOT NULL PRIMARY KEY, secret CHAR(200))
ENGINE=InnoDB ENCRYPTED=YES;

SELECT * FROM information_schema.innodb_tablespaces_encryption;
+-------+-----------------+-------------------+--------------------+-----------------+---------------------+--------------------------+------------------------------+----------------+----------------------+
| SPACE | NAME            | ENCRYPTION_SCHEME | KEYSERVER_REQUESTS | MIN_KEY_VERSION | CURRENT_KEY_VERSION | KEY_ROTATION_PAGE_NUMBER | KEY_ROTATION_MAX_PAGE_NUMBER | CURRENT_KEY_ID | ROTATING_OR_FLUSHING |
+-------+-----------------+-------------------+--------------------+-----------------+---------------------+--------------------------+------------------------------+----------------+----------------------+
|    78 | training/table1 |                 1 |                  0 |               1 |                   1 |                     NULL |                         NULL |              1 |                    0 |
+-------+-----------------+-------------------+--------------------+-----------------+---------------------+--------------------------+------------------------------+----------------+----------------------+
1 row in set (0.001 sec)

```

## Next step: Encrypt more 

```
# /etc/my.cnf.d/server.cnf 
# what to encrypt
innodb_encrypt_log
innodb_encrypt_tables=ON
innodb_encryption_threads=4
# for monitoring // additional information in information_schema.innodb-tablespaces-encryption  
innodb-tablespaces-encryption

# restart
systemctl restart mariadb 

```



## Order is important 

```
# !!! if you do not do it in this order it will not work 
# !!! Plugin cannot be loaded

# As described in https://mariadb.com/resources/blog/table-and-tablespace-encryption-on-mariadb-10-1/
# Step 1 
# Create the encryption keys and add to keyfile 

# Step 2:
# Change the settings in /etc/my.cnf.d/server.cnf 
[mariadb]
plugin-load-add=file_key_management
file_key_management_filename=/mnt/usb/secret.txt
file_key_management_encryption_algorithm=AES_CTR
```

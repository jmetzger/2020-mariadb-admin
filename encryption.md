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

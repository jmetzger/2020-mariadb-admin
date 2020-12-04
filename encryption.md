# Encryption (data-at-rest)  

## Docs
  * https://mariadb.com/resources/blog/table-and-tablespace-encryption-on-mariadb-10-1/
  * https://mariadb.com/kb/en/data-at-rest-encryption-overview/

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

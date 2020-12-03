# ssl - mariadb 

## Create CA and Server-Key 


```

# On Server - create ca and certificates 
sudo mkdir -p /etc/mysql/ssl
sudo cd /etc/mysql/ssl

# create ca.  
sudo openssl genrsa 4096 > ca-key.pem

# create ca-certificate 
# Common Name: MariaDB Admin 
sudo openssl req -new -x509 -nodes -days 365000 -key ca-key.pem -out ca-cert.pem

# create server-cert 
# Common Name: MariaDB Server
# Password: --- leave empty ----
sudo openssl req -newkey rsa:2048 -days 365000 -nodes -keyout server-key.pem -out server-req.pem

# Next process the rsa - key 
sudo openssl rsa -in server-key.pem -out server-key.pem

# Now sign the key 
sudo openssl x509 -req -in server-req.pem -days 365000 -CA ca-cert.pem -CAkey ca-key.pem -set_serial 01 -out server-cert.pem

```

## Create Client-Key 

  * You must have created a CA before that 
  
```
# Create Client certificate 
sudo openssl req -newkey rsa:2048 -days 365000 -nodes -keyout client-key.pem -out client-req.pem

# process RSA - Key 
sudo openssl rsa -in client-key.pem -out client-key.pem

# sign certficate with CA 
sudo openssl x509 -req -in client-req.pem -days 365000 -CA ca-cert.pem -CAkey ca-key.pem -set_serial 01 -out client-cert.pem
```

## Verify certificates 

```
openssl verify -CAfile ca-cert.pem server-cert.pem client-cert.pem


```

## Configure Server 
```
# create file 
# /etc/my.cnf.d/z_ssl.cnf 
[mysqld]
ssl-ca=/etc/mysql/ssl/ca-cert.pem
ssl-cert=/etc/mysql/ssl/server-cert.pem
ssl-key=/etc/mysql/ssl/server-key.pem
## Set up TLS version here. For example TLS version 1.2 and 1.3 ##
tls_version = TLSv1.2,TLSv1.3

# Set ownership 
chown -vR mysql:mysql /etc/mysql/ssl/

```

### Restart and check for errors 
```
systemctl restart mariadb
journalctl -u mariadb 

```

### Setup on clients 

```
# from 
# copy /etc/mysql/ssl to client
cd /etc/mysql
tar cvfz ssl.tar.gz ssl
scp ssl.tar.gz 11trainingdo@ip:/tmp 
```













## Ref 

  * https://www.cyberciti.biz/faq/how-to-setup-mariadb-ssl-and-secure-connections-from-clients/
  
  ```

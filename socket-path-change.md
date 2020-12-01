# socket path change 
  * where to find it 

```
# change in server 
# /etc/my.cnf.d/server.cnf 
[mysqld]
socket=mysql.sock  # it will search in datadir because not starting with "/" 

# chnage for clients 
# /etc/my.cnf.d/mysql-clients.cnf 
# add the following section 
# client is a category for all mysql - client 
[client]
socket=/var/lib/mysql-data/mysql.sock  # full path is needed here 
```

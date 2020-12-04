# Load Testing Galera

## Only database

```
./sysbench --test=tests/db/oltp.lua --mysql-table-engine=innodb --oltp-test-mode=complex --oltp-read-only=off --oltp-table-size=100000 --max-requests=100000000 --num-threads=128 --max-time=60 --mysql-socket=/mnt/dfs/db/mysql.sock --mysql-user=root run
```

## With Webapp wordpress 

  * http://unix-linux-server.blogspot.com/2017/01/wordpress-how-to-setup-mariadb-galera.html
  
## Loadtesting webapp 

  * Jmeter: https://jmeter.apache.org/usermanual/get-started.html
  * blazemeter (uses Jmeter - scripts) https://www.blazemeter.com/

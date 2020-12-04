# Monitoring

## What to monitor 

### System 

  * Last auf dem System (top) 
  * Festplatte (z.B. 85% voll ?) df /var/lib/mysql
  * Swap (Wenn geswappt wird ist Hopfen und Malz verloren) 
  
### Erreichbarkeit 

  * Server per ping erreichen (mysqladmin ping -h ziel-ip) 
  * Einlogbar ? (myadmin ping -h ziel-ip -u control_user 
 
### Platte aka IO-Subsystem 

| --       | --          | -- |
| ------------- |:-------------:| -----:|
| Read/Write requests	      | IOPS (Input/Output operations per second) | -- |
| Average IO wait	| Time that queue operations have to wait for disk access |   -- |
| Average Read/Write time | Time it takes to finish disk access operations (latency) |  -- |
| Read/Write bandwidth | Data transfer from and towards your disk | -- |

### Gneral mysql metrics 

| Metric	| Comments	| Suggested Alert |
| ------------- |:-------------:| -----:|
| Uptime	| Seconds since the server was started. We can use this to detect respawns.	 | When uptime is < 180. |
| Threads_connected	| Number of clients currently connected. If none or too high, something is wrong.	| None |
| Max_used_connections |	Max number of connections at a time since server started. (max_used_connections / max_connections) indicates if you could run out soon of connection slots.|	When connections usage is > 85%. |
| Aborted_connects |	Number of failed connection attempts. When growing over a period of time either some credentials are wrong or we are being attacked.	| When aborted connects/min > 3. |

### InnoDB 

| Metric | Coments | Suggested Alert | 
| Innodb_row_lock_waits	| Number of times InnoDB had to wait before locking a row.	| None |
| Innodb_buffer_pool_wait_free	| Number of times InnoDB had to wait for memory pages to be flushed. If too high, innodb_buffer_pool_size is too small for current write load.	| None | 



## Ref 

  * https://blog.serverdensity.com/how-to-monitor-mysql/

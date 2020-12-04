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

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

  * Read/Write requests	IOPS (Input/Output operations per second)	None
IO Queue length	Tracks how many operations are waiting for disk access. If a query hits the cache, it doesn’t create any disk operation. If a query doesn’t hit the cache (i.e. a miss), it will create multiple disk operations.	None
Average IO wait	Time that queue operations have to wait for disk access.	None
Average Read/Write time	Time it takes to finish disk access operations (latency).	None
Read/Write bandwidth	Data transfer from and towards your disk.	None



## Ref 

  * https://blog.serverdensity.com/how-to-monitor-mysql/

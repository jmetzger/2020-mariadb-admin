# User killen 

## Prozessliste anzeigen und kill 

```
show processlist 
-- thread it 
kill 12 
-- kill as soon as possible but not immediately  
kill hard 12 
-- soft - wait for critical commands to finish 
kill soft 12 
```

## Get all threads for a specific user 

```
use information_schema;
select id from processlist where user='root' and host='localhost';
```

## Ref

  * https://mariadb.com/kb/en/kill/
  
## Externe Tools 

Threads nach bestimmten Kriterien killen 
https://www.percona.com/doc/percona-toolkit/LATEST/pt-kill.html

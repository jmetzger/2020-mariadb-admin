# Processlist 

```
show processlist 

# Detaillierter 
select id, user, host, db, command, time, state, info, progress from information_schema.processlist;
select + from information_schema.processlist;
# bzw. 
show full processlist 
```

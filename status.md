# Status 

```
show status # session status
show global status

# wieviele select abfragen in der aktuellen session
show status like '%Com_select%';
# wieviel select abfragen, seit der Server läuft 
show global status like '%Com_select%'

# session status zurücksetzen
flush status 
```

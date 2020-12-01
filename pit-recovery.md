# Teststellung 

## Szenario 

  1. Nachts um xy Uhr wird die tägliche Sicherung gemacht, mit folgenden Optionen 
     mysqldump --all-databases --delete-master-logs --master-data=2 -B > /usr/src/all-databases.sql
     
  2. Um 4.15 Uhr nachts passiert ein Unglück und eine komplette Datenbank wird gelöscht  

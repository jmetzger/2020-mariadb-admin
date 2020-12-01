# Journalctl 

## Journal persistent setzten 

```
cat /etc/systemd/journald.conf | grep '#Storage' 
# muss auf Auto -> Default 
# Alle Zeilen mit # am Anfang sind Default 

cd /var/log/
mkdir journal 

systemctl restart systemd-journal-flush.service
systemctl restart systemd-journald.service 

# now you should a new folder with digits in 
# /var/log/journal/124ed........ 
```


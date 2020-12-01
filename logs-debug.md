# Debugging 

```
# Steps: 
# 1. systemctl status mariadb 
# ## wenn nichts gefunden 
# ## -> service im journal anzeigen 
# 2. journalctl -u mariadb 
# ## wenn nichts gefunden 
# 3. /var/log/mysql oder /var/log/mariadb 
# ## wenn keine dateien dort 
# 4. /var/lib/mysql -> error_log oder name_des_servers.log
# ## last resort 
# 5. /var/log/messages 
```

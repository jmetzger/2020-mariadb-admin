# Galera startet nicht 

# Was ist los ? 

```
Keines der Nodes startet 

Debugging
# Jeden der Nodes stoppen mit 
systemctl stop mariadb 
# Auf jedem der Nodes socket löschen: 
rm /var/lib/mysql/mysql.socket
# Auf dem Server nachschauen in /var/lib/mysql/grastate.dat 
# ob einer der Server den neuesten Stand hat. 
# bedeutet: seq_no != -1 
Wenn ja, diesen .. dieser muss als erstes gestartet werden.
Wenn nein: es ist egal, welchen wir als erstes starten

Nach Entscheidung, welcher zuerst 
# Im Node prüfen, ob in /var/lib/mysql/grastate.dat 
# safe_to_bootstrap: 0 <- steht.
# Wenn ja:
# Ändern in 
safe_to_bootstrap: 1  # Datei manuell ändern 
galera_new_cluster (auf erstem Node) 

Auf node 2 und node 3:
systemctl start mariadb 

# Auf allen Nodes kontrollieren, ob die Cluster size o.k. 
echo "show status like 'wsrep_cluster%'" | mysql 

```

## War Reperatur erfolgreich ? 

```

wsrep_last_committed entspricht seq_no in /var/lib/mysql/grastate.data
.. und wird dort eingetragen, wenn der Server gestoppt 

seq_no ist immer -1, wenn der Server läuft. 

MariaDB [(none)]> show status like 'wsrep_last_committed';
+----------------------+-------+
| Variable_name        | Value |
+----------------------+-------+
| wsrep_last_committed | 19    |
+----------------------+-------+
1 row in set (0.001 sec)

MariaDB [(none)]> show status like 'wsrep_%state_uuid';
+--------------------------+--------------------------------------+
| Variable_name            | Value                                |
+--------------------------+--------------------------------------+
| wsrep_local_state_uuid   | 02e7239d-3557-11eb-85a9-c75571931751 |
| wsrep_cluster_state_uuid | 02e7239d-3557-11eb-85a9-c75571931751 |
+--------------------------+--------------------------------------+
2 rows in set (0.003 sec)

```

## Ist status o.k. ? Haben alle nodes die Daten

```
show status like 'wsrep_local_state_comment' #      | Synced
show status like 'wsrep_cluster_size' # sind alle Mitglieder im Cluster 
```

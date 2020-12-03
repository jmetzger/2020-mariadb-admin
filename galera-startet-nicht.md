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

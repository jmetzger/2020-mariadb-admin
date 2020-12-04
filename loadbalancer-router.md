# Load-Balancer / Router 

## HA-Proxy / 

  * Nachteile: Das Teil ist dumm. (Versteht kein SQL) 
  * geeignigt für einaches Schreiben auf den einen / Lesen auf den anderen
  * oder: RoundRobin 
  
## MySQL-Router / MySQL-Proxy 

### Galera Load-Balancer 

  * https://galeracluster.com/library/documentation/glb.html

### ProxySQL 

  * https://severalnines.com/database-blog/how-run-and-configure-proxysql-20-mysql-galera-cluster-docker


#### ProxySQL - Vorteile

  * OpenSource 
  * HochLast ausgelegt 

#### ProxySQL - Nachteile 

  * Etwas fricklig einzurichten (alles über db) 
  * User, die verwendet werden für den Zugang müssen eingerichtet   

### MaxScale 

  * in bestimmten Versionen GPL - frei
  * bis 2 Nodes auch frei. 
  * Lizenzkosten: BSL https://mariadb.com/de/bsl-faq-mariadb/

#### MaxScale - Vorteile 

  * Sehr schöne überwachung 
  * Springt er automatisch auf neuen Master (es wird einer ausgewählt auf den geschrieben wird) 
  * Fragt auch den Status ob Node erreichbar. 
  * Einfach zu konfigurieren / alles in einem Konfigurations - File 

```
# versteht sql 
# ReadWrite-Split Router 
# oder: Verteilt auf all 

```
#### MaxScale - Nachteile 

  * Nachteile: Transaktionen über mehrere Statements gehen immer auf den Master 

#### MaxScale - Achtung 

  * Bitte in MySQL/MariaDB keine Nutzer mit Wildcard eintragen, wenn ihr MaxScale verwendet 
  * user@'%' <- das nicht !!! 





# Load-Balancer / Router 

## HA-Proxy / 

  * Nachteile: Das Teil ist dumm. (Versteht kein SQL) 
  
## MySQL-Router / MySQL-Proxy 

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

Nachteile: Transaktionen über mehrere Statements gehen immer auf den Master 

```

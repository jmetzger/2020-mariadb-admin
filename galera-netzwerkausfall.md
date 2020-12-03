# Netzwerkausfall simulieren 

```
# Achtung, es sollten keine anderen Firewalls laufen 
iptables -A INPUT -p tcp --dport 3306 -j DENY
iptables -A INPUT -p tcp --dport 3306 -j DROP
   41  iptables -A INPUT -p tcp --dport 4567 -j DROP
   42  iptables -A INPUT -p tcp --dport 4444 -j DROP
   43  iptables -A INPUT -p tcp --dport 4568 -j DROP
   44  iptables -A OUTPUT -p tcp --dport 3306 -j DENY
   45  iptables -A OUTPUT -p tcp --dport 3306 -j DROP
   46  iptables -A OUTPUT -p tcp --dport 4444 -j DROP
   47  iptables -A OUTPUT -p tcp --dport 4567 -j DROP
   48  iptables -A OUTPUT -p tcp --dport 4568 -j DROP
# cluster status ist dann Non-Primary
```

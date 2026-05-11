# OSPF Routing
## Purpose
Become familiar with setting up Open Shortest Path First (OSPF) routing protocol on a LAN.

## Useful Commands
### Create an OSPF instance
```
router ospf <PID>
```

### Add a network to be advertised
```
network <IP> <WILDCARD MASK> area <AREA>
```

### Troubleshooting / Verifying
```
show ip ospf neighbors
show ip ospf interface
```



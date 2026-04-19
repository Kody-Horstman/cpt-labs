# Static Routing Homework
2026-04-17
Kody Horstman

## Purpose
Become familiar with configuring and maintaining a statically routed network.

## Objectives
1. Configure a statically routed local area network.
2. Configure a gateway of last resort.
3. Fix network after the network changes.

## Specs
- ISP connection
    - ISP router (44.128.128.1 /30)
    - RTR-3 (44.128.128.2 /30)
    - Random "External" IP addr to ping against (lo0 x.x.x.x /32)
- 5 Departments 
    - Each department has 1 switch and 1 PC
    - All PCs must be able to ping each other and the "External" IP addr

## Design
All internal networks will stem from the IP space 10.0.0.0/8
- 10.0.0.0/16
    - 10.0.0.0/30  : Rtr-1 <-> Rtr-2
    - 10.0.0.4/30  : Rtr-2 <-> Rtr-3
    - 10.0.0.8/30  : Rtr-3 <-> Rtr-4
    - 10.0.0.12/30 : Rtr-4 <-> Rtr-5
- 10.1.0.0/16  : Dept 1
    - 10.1.0.1  : Rtr-1
    - 10.1.0.2  : Pc-1
- 10.2.0.0/16  : 
- 10.3.0.0/16  : Dept 2
    - 10.3.0.1  : Rtr-2
    - 10.3.0.2  : Pc-2
- 10.4.0.0/16  : 
- 10.5.0.0/16  : Dept 3
    - 10.5.0.1  : Rtr-3
    - 10.5.0.2  : Pc-3
- 10.6.0.0/16  : 
- 10.7.0.0/16  : Dept 4
    - 10.7.0.1  : Rtr-4
    - 10.7.0.2  : Pc-4
- 10.8.0.0/16  : 
- 10.9.0.0/16  : Dept 5
    - 10.9.0.1  : Rtr-5
    - 10.9.0.2  : Pc-5

## Stage 1: initial setup
### Rtr-0 (ISP)
```
en
conf t

hostname Rtr-0

int g0/0/0
ip addr 44.128.128.1 255.255.255.252
no shut

int lo0
ip addr 69.69.69.69 255.255.255.255

exit 

ip route 10.0.0.0 255.0.0.0 44.128.128.2

end 
wr mem
```

### Rtr-1
```
enable
config terminal

hostname Rtr-1

int g0/0/0
ip addr 10.0.0.1 255.255.255.252
no shut

int vlan 1
ip addr 10.1.0.1 255.255.0.0
no shut

exit

ip route 10.0.0.4 255.255.255.252 10.0.0.2
ip route 10.0.0.8 255.255.255.252 10.0.0.2
ip route 10.0.0.12 255.255.255.252 10.0.0.2
ip route 10.3.0.0 255.255.0.0 10.0.0.2
ip route 10.5.0.0 255.255.0.0 10.0.0.2
ip route 10.7.0.0 255.255.0.0 10.0.0.2
ip route 10.9.0.0 255.255.0.0 10.0.0.2
ip route 44.128.128.0 255.255.255.252 10.0.0.2
ip route 0.0.0.0 0.0.0.0 10.0.0.2

end
wr mem
```

### Pc-1
```
ipconfig 10.1.0.2 255.255.0.0 10.1.0.1
```

### Rtr-2
```
enable
config terminal

hostname Rtr-2

int g0/0/0
ip addr 10.0.0.2 255.255.255.252
no shut

int g0/0/1
ip addr 10.0.0.5 255.255.255.252
no shut

int vlan 1
ip addr 10.3.0.1 255.255.0.0
no shut

exit 

ip route 10.1.0.0 255.255.0.0 10.0.0.1
ip route 10.5.0.0 255.255.0.0 10.0.0.6
ip route 10.0.0.8 255.255.255.252 10.0.0.6
ip route 10.7.0.0 255.255.0.0 10.0.0.6
ip route 10.0.0.12 255.255.255.252 10.0.0.6
ip route 10.9.0.0 255.255.0.0 10.0.0.6
ip route 0.0.0.0 0.0.0.0 10.0.0.6

end
wr mem
```

### Pc-2
```
ipconfig 10.3.0.2 255.255.0.0 10.3.0.1
```

### Rtr-3
```
enable
config terminal

hostname Rtr-3

int g0/0/0
ip addr 10.0.0.6 255.255.255.252
no shut

int g0/0/1
ip addr 10.0.0.9 255.255.255.252
no shut

int g0/0/2
ip addr 44.128.128.2 255.255.255.252
no shut

int vlan 1
ip addr 10.5.0.1 255.255.0.0
no shut

exit 

ip route 10.1.0.0 255.255.0.0 10.0.0.5
ip route 10.0.0.0 255.255.255.252 10.0.0.5
ip route 10.3.0.0 255.255.0.0 10.0.0.5
ip route 10.7.0.0 255.255.0.0 10.0.0.10
ip route 10.0.0.12 255.255.255.252 10.0.0.10
ip route 10.9.0.0 255.255.0.0 10.0.0.10
ip route 0.0.0.0 0.0.0.0 44.128.128.1

end
wr mem
```

### Pc-3
```
ipconfig 10.5.0.2 255.255.0.0 10.5.0.1
```

### Rtr-4
```
enable
config terminal

hostname Rtr-4

int g0/0/0
ip addr 10.0.0.10 255.255.255.252
no shut

int g0/0/1
ip addr 10.0.0.13 255.255.255.252
no shut

int vlan 1
ip addr 10.7.0.1 255.255.0.0
no shut

exit 

ip route 10.1.0.0 255.255.0.0 10.0.0.9
ip route 10.0.0.0 255.255.255.252 10.0.0.9
ip route 10.3.0.0 255.255.0.0 10.0.0.9
ip route 10.0.0.4 255.255.255.252 10.0.0.9
ip route 10.5.0.0 255.255.0.0 10.0.0.9
ip route 44.128.128.0 255.255.255.252 10.0.0.9
ip route 0.0.0.0 0.0.0.0 10.0.0.9
ip route 10.9.0.0 255.255.0.0 10.0.0.14

end
wr mem
```

### Pc-4
```
ipconfig 10.7.0.2 255.255.0.0 10.7.0.1
```

### Rtr-5
```
enable
config terminal

hostname Rtr-5

int g0/0/0
ip addr 10.0.0.14 255.255.255.252
no shut

int vlan 1
ip addr 10.9.0.1 255.255.0.0
no shut

exit 

ip route 10.1.0.0 255.255.0.0 10.0.0.13
ip route 10.0.0.0 255.255.255.252 10.0.0.13
ip route 10.3.0.0 255.255.0.0 10.0.0.13
ip route 10.0.0.4 255.255.255.252 10.0.0.13
ip route 10.5.0.0 255.255.0.0 10.0.0.13
ip route 10.0.0.8 255.255.255.252 10.0.0.13
ip route 10.7.0.0 255.255.0.0 10.0.0.13
ip route 44.128.128.0 255.255.255.252 10.0.0.13
ip route 0.0.0.0 0.0.0.0 10.0.0.13

end
wr mem
```

### Pc-5
```
ipconfig 10.9.0.2 255.255.0.0 10.9.0.1
```
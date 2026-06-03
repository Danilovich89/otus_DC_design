### Четвертое домашнее задание  - VXLAN EVPN L2
Underlay на eBGP уже собран, прололжаем. Делаем overlay VXLAN EVPN L2.

#### Адресация CE
|Device|Interface|IP Address|Subnet Mask|Description|Vlan
|---|---|---|---|---|---|
CE1|Ethernet12|192.168.10.1|255.255.255.0|to_Leaf1_Eth12 |vlan10
CE2|Ethernet12|192.168.10.2|255.255.255.0|to_Leaf2_Eth12 |vlan20
CE3|Ethernet12|192.168.10.3|255.255.255.0|to_Leaf3_Eth12 |vlan10
CE4|Ethernet12|192.168.10.4|255.255.255.0|to_Leaf4_Eth12 |vlan20
CE1|Ethernet12|192.168.20.1|255.255.255.0|to_Leaf1_Eth12 |vlan10
CE2|Ethernet12|192.168.20.2|255.255.255.0|to_Leaf2_Eth12 |vlan20
CE3|Ethernet12|192.168.20.3|255.255.255.0|to_Leaf3_Eth12 |vlan10
CE4|Ethernet12|192.168.20.4|255.255.255.0|to_Leaf4_Eth12 |vlan20

#### Установленные соседства:
##### Spine-1:
```
Spine-1#show bgp evpn summary
BGP summary information for VRF default
Router identifier 111.111.111.111, local AS number 65100
Neighbor Status Codes: m - Under maintenance
  Neighbor V AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  1.1.1.1  4 65001          77939     77895    0    0 18:25:33 Estab   4      4
  2.2.2.2  4 65002          77838     77855    0    0 18:25:03 Estab   4      4
  3.3.3.3  4 65003          77785     77788    0    0 18:24:47 Estab   3      3
  4.4.4.4  4 65004          77794     77817    0    0 18:24:35 Estab   4      4
Spine-1#
```
##### Spine-2:
```
Spine-2#show bgp evpn summary
BGP summary information for VRF default
Router identifier 222.222.222.222, local AS number 65100
Neighbor Status Codes: m - Under maintenance
  Neighbor V AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  1.1.1.1  4 65001          77840     77892    0    0 18:25:18 Estab   4      4
  2.2.2.2  4 65002          77867     77861    0    0 18:25:09 Estab   4      4
  3.3.3.3  4 65003          77697     77666    0    0 18:23:03 Estab   3      3
  4.4.4.4  4 65004          77780     77783    0    0 18:24:30 Estab   4      4
```

#### Маршруты:

##### Leaf1:
```
Leaf1#show bgp evpn route-type mac-ip
BGP routing table information for VRF default
Router identifier 1.1.1.1, local AS number 65001
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 65100:2 mac-ip 10010 0c60.544c.0000
                                 2.2.2.2               -       100     0       65100 65002 i
 *  ec    RD: 65100:2 mac-ip 10010 0c60.544c.0000
                                 2.2.2.2               -       100     0       65100 65002 i
```

##### Leaf2:
```
Leaf2#show bgp evpn route-type mac-ip
BGP routing table information for VRF default
Router identifier 2.2.2.2, local AS number 65002
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >      RD: 65100:2 mac-ip 10010 0c60.544c.0000
                                 -                     -       -       0       i
 * >Ec    RD: 65100:4 mac-ip 10010 0ce0.e140.0000
                                 4.4.4.4               -       100     0       65100 65004 i
 *  ec    RD: 65100:4 mac-ip 10010 0ce0.e140.0000
                                 4.4.4.4               -       100     0       65100 65004 i
 * >Ec    RD: 65100:1 mac-ip 10020 0cf6.8779.0000
                                 1.1.1.1               -       100     0       65100 65001 i
 *  ec    RD: 65100:1 mac-ip 10020 0cf6.8779.0000
                                 1.1.1.1               -       100     0       65100 65001 i
```
##### Leaf3:
```
Leaf3#show bgp evpn route-type mac-ip
BGP routing table information for VRF default
Router identifier 3.3.3.3, local AS number 65003
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 65100:2 mac-ip 10010 0c60.544c.0000
                                 2.2.2.2               -       100     0       65100 65002 i
 *  ec    RD: 65100:2 mac-ip 10010 0c60.544c.0000
                                 2.2.2.2               -       100     0       65100 65002 i
 * >Ec    RD: 65100:4 mac-ip 10010 0ce0.e140.0000
                                 4.4.4.4               -       100     0       65100 65004 i
 *  ec    RD: 65100:4 mac-ip 10010 0ce0.e140.0000
                                 4.4.4.4               -       100     0       65100 65004 i
 * >Ec    RD: 65100:1 mac-ip 10020 0cf6.8779.0000
                                 1.1.1.1               -       100     0       65100 65001 i
 *  ec    RD: 65100:1 mac-ip 10020 0cf6.8779.0000
                                 1.1.1.1               -       100     0       65100 65001 i
```
##### Leaf4:
```
Leaf4#show bgp evpn route-type mac-ip
BGP routing table information for VRF default
Router identifier 4.4.4.4, local AS number 65004
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 65100:2 mac-ip 10010 0c60.544c.0000
                                 2.2.2.2               -       100     0       65100 65002 i
 *  ec    RD: 65100:2 mac-ip 10010 0c60.544c.0000
                                 2.2.2.2               -       100     0       65100 65002 i
 * >      RD: 65100:4 mac-ip 10010 0ce0.e140.0000
                                 -                     -       -       0       i
 * >Ec    RD: 65100:1 mac-ip 10020 0cf6.8779.0000
                                 1.1.1.1               -       100     0       65100 65001 i
 *  ec    RD: 65100:1 mac-ip 10020 0cf6.8779.0000
                                 1.1.1.1               -       100     0       65100 65001 i
```

#### Ping:

##### С CE1 на CE4 vlan10:
```
ubuntu@ubuntu-cloud:~$ ping 192.168.10.4 -I v10.ens3
PING 192.168.10.4 (192.168.10.4) from 192.168.10.1 v10.ens3: 56(84) bytes of data.
64 bytes from 192.168.10.4: icmp_seq=1 ttl=64 time=30.9 ms
64 bytes from 192.168.10.4: icmp_seq=2 ttl=64 time=34.0 ms
64 bytes from 192.168.10.4: icmp_seq=3 ttl=64 time=28.0 ms
^C
--- 192.168.10.4 ping statistics ---
```

##### С CE3 на CE2 vlan20:
```

ubuntu@ubuntu-cloud:~$ ping 192.168.20.2 -I v20.ens3
PING 192.168.20.2 (192.168.20.2) from 192.168.20.3 v20.ens3: 56(84) bytes of data.
64 bytes from 192.168.20.2: icmp_seq=1 ttl=64 time=115 ms
64 bytes from 192.168.20.2: icmp_seq=2 ttl=64 time=37.3 ms
64 bytes from 192.168.20.2: icmp_seq=3 ttl=64 time=31.7 ms
64 bytes from 192.168.20.2: icmp_seq=4 ttl=64 time=31.0 ms
64 bytes from 192.168.20.2: icmp_seq=5 ttl=64 time=33.1 ms
^C
--- 192.168.20.2 ping statistics ---
```

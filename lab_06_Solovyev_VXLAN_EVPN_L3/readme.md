### Шестое домашнее задание  - VXLAN EVPN L3
Underlay на eBGP уже собран, прололжаем. Разбираем VXLAN EVPN L2, чтобы не вносил путаницу. Делаем overlay VXLAN EVPN L3.

#### Адресация CE
|Device|Interface|IP Address|Subnet Mask|Description|Vlan
|---|---|---|---|---|---|
CE1|Ethernet12|192.168.10.1|255.255.255.0|to_Leaf1_Eth12 |vlan10
CE2|Ethernet12|192.168.20.1|255.255.255.0|to_Leaf2_Eth12 |vlan20
CE3|Ethernet12|192.168.30.1|255.255.255.0|to_Leaf3_Eth12 |vlan30
CE4|Ethernet12|192.168.40.1|255.255.255.0|to_Leaf4_Eth12 |vlan40


#### Конфиги:
Отдельно в приложенных файлах в папке "конфиги". Конфиги Spine без изменений в силу того, что underlay без изменений. На Leaf underlay-часть конфига так же без изменений

#### Маршруты:

##### Leaf1:
```
Leaf1#show bgp evpn route-type ip-prefix ipv4
BGP routing table information for VRF default
Router identifier 1.1.1.1, local AS number 65001
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >      RD: 1.1.1.1:1 ip-prefix 192.168.10.0/24
                                 -                     -       -       0       i
 * >Ec    RD: 2.2.2.2:1 ip-prefix 192.168.20.0/24
                                 2.2.2.2               -       100     0       65100 65002 i
 *  ec    RD: 2.2.2.2:1 ip-prefix 192.168.20.0/24
                                 2.2.2.2               -       100     0       65100 65002 i
 * >Ec    RD: 3.3.3.3:1 ip-prefix 192.168.30.0/24
                                 3.3.3.3               -       100     0       65100 65003 i
 *  ec    RD: 3.3.3.3:1 ip-prefix 192.168.30.0/24
                                 3.3.3.3               -       100     0       65100 65003 i
 * >Ec    RD: 4.4.4.4:1 ip-prefix 192.168.40.0/24
                                 4.4.4.4               -       100     0       65100 65004 i
 *  ec    RD: 4.4.4.4:1 ip-prefix 192.168.40.0/24
                                 4.4.4.4               -       100     0       65100 65004 i

```

##### Leaf2:
```
Leaf2#show bgp evpn route-type ip-prefix ipv4
BGP routing table information for VRF default
Router identifier 2.2.2.2, local AS number 65002
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 1.1.1.1:1 ip-prefix 192.168.10.0/24
                                 1.1.1.1               -       100     0       65100 65001 i
 *  ec    RD: 1.1.1.1:1 ip-prefix 192.168.10.0/24
                                 1.1.1.1               -       100     0       65100 65001 i
 * >      RD: 2.2.2.2:1 ip-prefix 192.168.20.0/24
                                 -                     -       -       0       i
 * >Ec    RD: 3.3.3.3:1 ip-prefix 192.168.30.0/24
                                 3.3.3.3               -       100     0       65100 65003 i
 *  ec    RD: 3.3.3.3:1 ip-prefix 192.168.30.0/24
                                 3.3.3.3               -       100     0       65100 65003 i
 * >Ec    RD: 4.4.4.4:1 ip-prefix 192.168.40.0/24
                                 4.4.4.4               -       100     0       65100 65004 i
 *  ec    RD: 4.4.4.4:1 ip-prefix 192.168.40.0/24
                                 4.4.4.4               -       100     0       65100 65004 i

```
##### Leaf3:
```
Leaf3#show bgp evpn route-type ip-prefix ipv4
BGP routing table information for VRF default
Router identifier 3.3.3.3, local AS number 65003
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 1.1.1.1:1 ip-prefix 192.168.10.0/24
                                 1.1.1.1               -       100     0       65100 65001 i
 *  ec    RD: 1.1.1.1:1 ip-prefix 192.168.10.0/24
                                 1.1.1.1               -       100     0       65100 65001 i
 * >Ec    RD: 2.2.2.2:1 ip-prefix 192.168.20.0/24
                                 2.2.2.2               -       100     0       65100 65002 i
 *  ec    RD: 2.2.2.2:1 ip-prefix 192.168.20.0/24
                                 2.2.2.2               -       100     0       65100 65002 i
 * >      RD: 3.3.3.3:1 ip-prefix 192.168.30.0/24
                                 -                     -       -       0       i
 * >Ec    RD: 4.4.4.4:1 ip-prefix 192.168.40.0/24
                                 4.4.4.4               -       100     0       65100 65004 i
 *  ec    RD: 4.4.4.4:1 ip-prefix 192.168.40.0/24
                                 4.4.4.4               -       100     0       65100 65004 i

```
##### Leaf4:
```
Leaf4#show bgp evpn route-type ip-prefix ipv4
BGP routing table information for VRF default
Router identifier 4.4.4.4, local AS number 65004
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 1.1.1.1:1 ip-prefix 192.168.10.0/24
                                 1.1.1.1               -       100     0       65100 65001 i
 *  ec    RD: 1.1.1.1:1 ip-prefix 192.168.10.0/24
                                 1.1.1.1               -       100     0       65100 65001 i
 * >Ec    RD: 2.2.2.2:1 ip-prefix 192.168.20.0/24
                                 2.2.2.2               -       100     0       65100 65002 i
 *  ec    RD: 2.2.2.2:1 ip-prefix 192.168.20.0/24
                                 2.2.2.2               -       100     0       65100 65002 i
 * >Ec    RD: 3.3.3.3:1 ip-prefix 192.168.30.0/24
                                 3.3.3.3               -       100     0       65100 65003 i
 *  ec    RD: 3.3.3.3:1 ip-prefix 192.168.30.0/24
                                 3.3.3.3               -       100     0       65100 65003 i
 * >      RD: 4.4.4.4:1 ip-prefix 192.168.40.0/24
                                 -                     -       -       0       i
Leaf4#

```

#### Ping:

##### С CE1 на CE4:
```
ubuntu@ubuntu-cloud:~$ ping 192.168.40.1 -I v10.ens3
PING 192.168.40.1 (192.168.40.1) from 192.168.10.1 v10.ens3: 56(84) bytes of data.
64 bytes from 192.168.40.1: icmp_seq=1 ttl=62 time=15.0 ms
64 bytes from 192.168.40.1: icmp_seq=2 ttl=62 time=12.7 ms
64 bytes from 192.168.40.1: icmp_seq=3 ttl=62 time=13.0 ms
^C

```

##### С CE3 на CE2:
```
ubuntu@ubuntu-cloud:~$ ping 192.168.20.1 -I v30.ens3
PING 192.168.20.1 (192.168.20.1) from 192.168.30.1 v30.ens3: 56(84) bytes of data.
64 bytes from 192.168.20.1: icmp_seq=1 ttl=62 time=16.9 ms
64 bytes from 192.168.20.1: icmp_seq=2 ttl=62 time=13.9 ms
64 bytes from 192.168.20.1: icmp_seq=3 ttl=62 time=12.8 ms
64 bytes from 192.168.20.1: icmp_seq=4 ttl=62 time=11.9 ms
^C

--- 192.168.20.2 ping statistics ---
```

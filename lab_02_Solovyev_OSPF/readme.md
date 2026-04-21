### Второе домашнее задание  - OSPF
Здесь была собрана виртуальная лаба в GNS3 на Arista. Далее продолжаем в ней.

#### Конфиги

##### Spine-1:
```
Spine-1#show running-config section ospf
interface Ethernet1
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Ethernet2
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Ethernet3
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Ethernet4
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
router ospf 1
   router-id 111.111.111.111
   passive-interface Loopback0
   max-lsa 12000
```

##### Spine-2:
```
Spine-2#show running-config section ospf
interface Ethernet1
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Ethernet2
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Ethernet3
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Ethernet4
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
router ospf 1
   router-id 222.222.222.222
   passive-interface Loopback0
   max-lsa 12000
```

##### Leaf-1:
```
Leaf1#show run sec ospf
interface Ethernet1
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Ethernet2
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
router ospf 1
   router-id 1.1.1.1
   passive-interface Loopback0
   max-lsa 12000
```

##### Leaf-2:
```
Leaf2#show running-config section ospf
interface Ethernet1
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Ethernet2
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
router ospf 1
   router-id 2.2.2.2
   passive-interface Loopback0
   max-lsa 12000
```

##### Leaf-3:
```
Leaf3#show run sec ospf
interface Ethernet1
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Ethernet2
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
router ospf 1
   router-id 3.3.3.3
   passive-interface Loopback0
   max-lsa 12000
```

##### Leaf-4:
```
Leaf4#show running-config section ospf
interface Ethernet1
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Ethernet2
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
router ospf 1
   router-id 4.4.4.4
   passive-interface Loopback0
   max-lsa 12000
```

#### Установленные соседства:
##### Spine-1:
```
Spine-1#show ip ospf neighbor
Neighbor ID     Instance VRF      Pri State                  Dead Time   Address         Interface
1.1.1.1         1        default  0   FULL                   00:00:30    111.1.1.2       Ethernet1
2.2.2.2         1        default  0   FULL                   00:00:33    111.2.2.2       Ethernet2
3.3.3.3         1        default  0   FULL                   00:00:33    111.3.3.2       Ethernet3
4.4.4.4         1        default  0   FULL                   00:00:29    111.4.4.2       Ethernet4
```

##### Spine-2:
```
Spine-2#show ip ospf nei
Neighbor ID     Instance VRF      Pri State                  Dead Time   Address         Interface
1.1.1.1         1        default  0   FULL                   00:00:37    222.1.1.2       Ethernet1
2.2.2.2         1        default  0   FULL                   00:00:34    222.2.2.2       Ethernet2
3.3.3.3         1        default  0   FULL                   00:00:32    222.3.3.2       Ethernet3
4.4.4.4         1        default  0   FULL                   00:00:30    222.4.4.2       Ethernet4

```
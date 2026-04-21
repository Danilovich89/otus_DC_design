### Третье домашнее задание - ISIS

#### Конфиги

##### Spine1:
```
Spine-1#show run sec isi
interface Ethernet1
   isis enable 1
   isis network point-to-point
interface Ethernet2
   isis enable 1
   isis network point-to-point
interface Ethernet3
   isis enable 1
   isis network point-to-point
interface Ethernet4
   isis enable 1
   isis network point-to-point
router isis 1
   net 49.0001.1111.1111.1111.00
   router-id ipv4 111.111.111.111
   !
   address-family ipv4 unicast
```


##### Spine2:
```
Spine-2#show run sec isi
interface Ethernet1
   isis enable 1
   isis network point-to-point
interface Ethernet2
   isis enable 1
   isis network point-to-point
interface Ethernet3
   isis enable 1
   isis network point-to-point
interface Ethernet4
   isis enable 1
   isis network point-to-point
router isis 1
   net 49.0001.2222.2222.2222.00
   router-id ipv4 222.222.222.222
   !
   address-family ipv4 unicast
```

##### Leaf1:
```
Leaf-1#show run sec isi
interface Ethernet1
   isis enable 1
   isis network point-to-point
interface Ethernet2
   isis enable 1
   isis network point-to-point
router isis 1
   net 49.0001.0010.0100.1001.00
   router-id ipv4 1.1.1.1
   !
   address-family ipv4 unicast
```

##### Leaf2:
```
Leaf-2#show run sec isi
interface Ethernet1
   isis enable 1
   isis network point-to-point
interface Ethernet2
   isis enable 1
   isis network point-to-point
router isis 1
   net 49.0001.0020.0200.2002.00
   router-id ipv4 2.2.2.2
   !
   address-family ipv4 unicast
```

##### Leaf3:
```
Leaf-3#show run sec isi
interface Ethernet1
   isis enable 1
   isis network point-to-point
interface Ethernet2
   isis enable 1
   isis network point-to-point
router isis 1
   net 49.0001.0030.0300.3003.00
   router-id ipv4 3.3.3.3
   !
   address-family ipv4 unicast
```

##### Leaf4:
```
Leaf-4#show run sec isi
interface Ethernet1
   isis enable 1
   isis network point-to-point
interface Ethernet2
   isis enable 1
   isis network point-to-point
router isis 1
   net 49.0001.0040.0400.4004.00
   router-id ipv4 4.4.4.4
   !
   address-family ipv4 unicast
```

#### Соседства:

##### Spine1:
```
Spine-1#show isis neighbors

Instance  VRF      System Id        Type Interface          SNPA              State Hold time   Circuit Id
1         default  Leaf1            L1L2 Ethernet1          P2P               UP    23          13
1         default  Leaf1            L1L2 Ethernet2          P2P               UP    22          13
1         default  Leaf3            L1L2 Ethernet3          P2P               UP    28          13
1         default  Leaf4            L1L2 Ethernet4          P2P               UP    26          13
```


##### Spine2:
```
Spine-2#show isis nei
Instance  VRF      System Id        Type Interface          SNPA              State Hold time   Circuit Id
1         default  Leaf1            L1L2 Ethernet1          P2P               UP    22          14
1         default  Leaf1            L1L2 Ethernet2          P2P               UP    28          14
1         default  Leaf3            L1L2 Ethernet3          P2P               UP    24          14
1         default  Leaf4            L1L2 Ethernet4          P2P               UP    22          14
```
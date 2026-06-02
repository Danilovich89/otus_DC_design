### Четвертое домашнее задание  - eBGP underlay
Здесь была собрана виртуальная лаба в GNS3 на Arista. Далее продолжаем в ней.

#### Конфиги

##### Spine-1:
```
Spine-1(config-router-bgp)#show running-config sec bgp
router bgp 65100
   router-id 111.111.111.111
   maximum-paths 4 ecmp 4
   bgp listen range 111.0.0.0/8 peer-group LEAFS peer-filter PF_AS_RANGE
   bgp listen range 222.0.0.0/8 peer-group LEAFS peer-filter PF_AS_RANGE
   neighbor LEAFS peer group
   neighbor LEAFS bfd
   neighbor LEAFS timers 1 3
   redistribute connected
   !
   address-family ipv4
      neighbor LEAFS activate
      redistribute connected route-map RM_RED_L0
```

##### Spine-2:
```
Spine-2(config-router-bgp)#show running-config sec bgp
router bgp 65100
   router-id 222.222.222.222
   maximum-paths 4 ecmp 4
   bgp listen range 111.0.0.0/8 peer-group LEAFS peer-filter PF_AS_RANGE
   bgp listen range 222.0.0.0/8 peer-group LEAFS peer-filter PF_AS_RANGE
   neighbor LEAFS peer group
   neighbor LEAFS bfd
   neighbor LEAFS timers 1 3
   redistribute connected
   !
   address-family ipv4
      neighbor LEAFS activate
      redistribute connected route-map RM_RED_L0
```

##### Leaf-1:
```
Leaf1(config-router-bgp)#show running-config sec bgp
router bgp 65001
   router-id 1.1.1.1
   maximum-paths 4 ecmp 4
   bgp listen range 111.0.0.0/8 peer-group SPINES peer-filter PF_AS
   bgp listen range 222.0.0.0/8 peer-group SPINES peer-filter PF_AS
   neighbor SPINES peer group
   neighbor SPINES remote-as 65100
   neighbor SPINES bfd
   neighbor SPINES timers 1 3
   neighbor 111.1.1.1 peer group SPINES
   neighbor 222.1.1.1 peer group SPINES
   redistribute connected
   !
   address-family ipv4
      neighbor SPINES activate
      redistribute connected route-map RM_RED_L0
```

##### Leaf-2:
```
Leaf2(config-router-bgp)#show running-config sec bgp
router bgp 65002
   router-id 2.2.2.2
   maximum-paths 4 ecmp 4
   bgp listen range 111.0.0.0/8 peer-group SPINES peer-filter PF_AS
   bgp listen range 222.0.0.0/8 peer-group SPINES peer-filter PF_AS
   neighbor SPINES peer group
   neighbor SPINES remote-as 65100
   neighbor SPINES bfd
   neighbor SPINES timers 1 3
   neighbor 111.2.2.1 peer group SPINES
   neighbor 222.2.2.1 peer group SPINES
   redistribute connected
   !
   address-family ipv4
      neighbor SPINES activate
      redistribute connected route-map RM_RED_L0

```

##### Leaf-3:
```
Leaf3(config-router-bgp)#show running-config sec bgp
router bgp 65003
   router-id 3.3.3.3
   maximum-paths 4 ecmp 4
   bgp listen range 111.0.0.0/8 peer-group SPINES peer-filter PF_AS
   bgp listen range 222.0.0.0/8 peer-group SPINES peer-filter PF_AS
   neighbor SPINES peer group
   neighbor SPINES remote-as 65100
   neighbor SPINES bfd
   neighbor SPINES timers 1 3
   neighbor 111.3.3.1 peer group SPINES
   neighbor 222.3.3.1 peer group SPINES
   redistribute connected
   !
   address-family ipv4
      neighbor SPINES activate
      redistribute connected route-map RM_RED_L0

```

##### Leaf-4:
```
Leaf4(config-router-bgp)#show running-config sec bgp
router bgp 65004
   router-id 4.4.4.4
   maximum-paths 4 ecmp 4
   bgp listen range 111.0.0.0/8 peer-group SPINES peer-filter PF_AS
   bgp listen range 222.0.0.0/8 peer-group SPINES peer-filter PF_AS
   neighbor SPINES peer group
   neighbor SPINES remote-as 65100
   neighbor SPINES bfd
   neighbor SPINES timers 1 3
   neighbor 111.4.4.1 peer group SPINES
   neighbor 222.4.4.1 peer group SPINES
   redistribute connected
   !
   address-family ipv4
      neighbor SPINES activate
      redistribute connected route-map RM_RED_L0

```

#### Установленные соседства:
##### Spine-1:
```

Spine-1(config-router-bgp)#show ip bgp sum
BGP summary information for VRF default
Router identifier 111.111.111.111, local AS number 65100
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  111.1.1.2        4  65001            515       516    0    0 00:08:29 Estab   4      4
  111.2.2.2        4  65002            334       336    0    0 00:05:28 Estab   4      4
  111.3.3.2        4  65003            294       297    0    0 00:04:49 Estab   4      4
  111.4.4.2        4  65004            206       208    0    0 00:03:20 Estab   4      4


```

##### Spine-2:
```

Spine-2(config-peer-filter-PS_AS_RANGE)#show ip bgp sum
BGP summary information for VRF default
Router identifier 222.222.222.222, local AS number 65100
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  222.1.1.2        4  65001            528       527    0    0 00:08:39 Estab   4      4
  222.2.2.2        4  65002            350       348    0    0 00:05:40 Estab   4      4
  222.3.3.2        4  65003            310       309    0    0 00:05:01 Estab   4      4
  222.4.4.2        4  65004            222       220    0    0 00:03:32 Estab   4      4

```


#### Маршруты:

##### Spine1:
```
Spine-1(config-router-bgp)#show ip route

VRF: default
Codes: C - connected, S - static, K - kernel,
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 B E      1.1.1.1/32 [200/0] via 111.1.1.2, Ethernet1
 B E      1.1.1.11/32 [200/0] via 111.1.1.2, Ethernet1
 B E      2.2.2.2/32 [200/0] via 111.2.2.2, Ethernet2
 B E      2.2.2.22/32 [200/0] via 111.2.2.2, Ethernet2
 B E      3.3.3.3/32 [200/0] via 111.3.3.2, Ethernet3
 B E      3.3.3.33/32 [200/0] via 111.3.3.2, Ethernet3
 B E      4.4.4.4/32 [200/0] via 111.4.4.2, Ethernet4
 B E      4.4.4.44/32 [200/0] via 111.4.4.2, Ethernet4
 C        111.1.1.0/30 is directly connected, Ethernet1
 C        111.2.2.0/30 is directly connected, Ethernet2
 C        111.3.3.0/30 is directly connected, Ethernet3
 C        111.4.4.0/30 is directly connected, Ethernet4
 C        111.111.111.111/32 is directly connected, Loopback0
 B E      222.1.1.0/30 [200/0] via 111.1.1.2, Ethernet1
 B E      222.2.2.0/30 [200/0] via 111.2.2.2, Ethernet2
 B E      222.3.3.0/30 [200/0] via 111.3.3.2, Ethernet3
 B E      222.4.4.0/30 [200/0] via 111.4.4.2, Ethernet4
```

##### Spine2:
```

Spine-2(config-peer-filter-PS_AS_RANGE)#show ip bgp sum
BGP summary information for VRF default
Router identifier 222.222.222.222, local AS number 65100
Neighbor Status Codes: m - Under maintenance
  Neighbor         V  AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  222.1.1.2        4  65001            528       527    0    0 00:08:39 Estab   4      4
  222.2.2.2        4  65002            350       348    0    0 00:05:40 Estab   4      4
  222.3.3.2        4  65003            310       309    0    0 00:05:01 Estab   4      4
  222.4.4.2        4  65004            222       220    0    0 00:03:32 Estab   4      4
Spine-2(config-peer-filter-PS_AS_RANGE)#show ip route

VRF: default
Codes: C - connected, S - static, K - kernel,
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 B E      1.1.1.1/32 [200/0] via 222.1.1.2, Ethernet1
 B E      1.1.1.11/32 [200/0] via 222.1.1.2, Ethernet1
 B E      2.2.2.2/32 [200/0] via 222.2.2.2, Ethernet2
 B E      2.2.2.22/32 [200/0] via 222.2.2.2, Ethernet2
 B E      3.3.3.3/32 [200/0] via 222.3.3.2, Ethernet3
 B E      3.3.3.33/32 [200/0] via 222.3.3.2, Ethernet3
 B E      4.4.4.4/32 [200/0] via 222.4.4.2, Ethernet4
 B E      4.4.4.44/32 [200/0] via 222.4.4.2, Ethernet4
 B E      111.1.1.0/30 [200/0] via 222.1.1.2, Ethernet1
 B E      111.2.2.0/30 [200/0] via 222.2.2.2, Ethernet2
 B E      111.3.3.0/30 [200/0] via 222.3.3.2, Ethernet3
 B E      111.4.4.0/30 [200/0] via 222.4.4.2, Ethernet4
 C        222.1.1.0/30 is directly connected, Ethernet1
 C        222.2.2.0/30 is directly connected, Ethernet2
 C        222.3.3.0/30 is directly connected, Ethernet3
 C        222.4.4.0/30 is directly connected, Ethernet4
 C        222.222.222.222/32 is directly connected, Loopback0
```



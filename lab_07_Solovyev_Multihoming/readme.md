### Шестое домашнее задание  - VXLAN EVPN Multihoming (Anycast GW)
Разбираем singlehome-подключения клиентов CE1-Leaf1, CE2-Leaf2 и собираем multihoming CE1-(Leaf1+Leaf2), CE2-(Leaf1+Leaf2). Подключения CE3-Leaf3 и CE4-Leaf4 оставляем без изменений

#### Адресация CE
|Device|Interface|IP Address|Subnet Mask|Description|Vlan
|---|---|---|---|---|---|
CE1|bond0(ens3+ens4)|192.168.10.1|255.255.255.0|to_Leaf1+Leaf2/Eth11 |vlan10
CE2|bond0(ens3+ens4)|192.168.20.1|255.255.255.0|to_Leaf1+Leaf2/Eth12 |vlan20
CE3|ens3|192.168.30.1|255.255.255.0|to_Leaf3_Eth12 |vlan30
CE4|ens3|192.168.40.1|255.255.255.0|to_Leaf4_Eth12 |vlan40


#### Конфиги:
Отдельно в приложенных файлах в папке "конфиги". Конфиги Spine и Leaf3-Leaf4 без изменений.

#### Маршруты:

##### Leaf1 auto-discovery:
```
Leaf1#show bgp evpn route-type auto-discovery
BGP routing table information for VRF default
Router identifier 1.1.1.1, local AS number 65001
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >      RD: 1.1.1.1:10 auto-discovery 0 0000:0000:0000:0000:1212
                                 -                     -       -       0       i
 * >      RD: 1.1.1.1:20 auto-discovery 0 0000:0000:0000:0000:1212
                                 -                     -       -       0       i
 * >Ec    RD: 2.2.2.2:10 auto-discovery 0 0000:0000:0000:0000:1212
                                 2.2.2.2               -       100     0       65100 65002 i
 *  ec    RD: 2.2.2.2:10 auto-discovery 0 0000:0000:0000:0000:1212
                                 2.2.2.2               -       100     0       65100 65002 i
 * >Ec    RD: 2.2.2.2:20 auto-discovery 0 0000:0000:0000:0000:1212
                                 2.2.2.2               -       100     0       65100 65002 i
 *  ec    RD: 2.2.2.2:20 auto-discovery 0 0000:0000:0000:0000:1212
                                 2.2.2.2               -       100     0       65100 65002 i
 * >      RD: 1.1.1.1:1 auto-discovery 0000:0000:0000:0000:1212
                                 -                     -       -       0       i
 * >Ec    RD: 2.2.2.2:1 auto-discovery 0000:0000:0000:0000:1212
                                 2.2.2.2               -       100     0       65100 65002 i
 *  ec    RD: 2.2.2.2:1 auto-discovery 0000:0000:0000:0000:1212
                                 2.2.2.2               -       100     0       65100 65002 i
 * >      RD: 1.1.1.1:10 auto-discovery 0 0000:0000:0000:0000:1313
                                 -                     -       -       0       i
 * >      RD: 1.1.1.1:20 auto-discovery 0 0000:0000:0000:0000:1313
                                 -                     -       -       0       i
 * >Ec    RD: 2.2.2.2:10 auto-discovery 0 0000:0000:0000:0000:1313
                                 2.2.2.2               -       100     0       65100 65002 i
 *  ec    RD: 2.2.2.2:10 auto-discovery 0 0000:0000:0000:0000:1313
                                 2.2.2.2               -       100     0       65100 65002 i
 * >Ec    RD: 2.2.2.2:20 auto-discovery 0 0000:0000:0000:0000:1313
                                 2.2.2.2               -       100     0       65100 65002 i
 *  ec    RD: 2.2.2.2:20 auto-discovery 0 0000:0000:0000:0000:1313
                                 2.2.2.2               -       100     0       65100 65002 i
 * >      RD: 1.1.1.1:1 auto-discovery 0000:0000:0000:0000:1313
                                 -                     -       -       0       i
 * >Ec    RD: 2.2.2.2:1 auto-discovery 0000:0000:0000:0000:1313
                                 2.2.2.2               -       100     0       65100 65002 i
 *  ec    RD: 2.2.2.2:1 auto-discovery 0000:0000:0000:0000:1313
                                 2.2.2.2               -       100     0       65100 65002 i
                         4.4.4.4               -       100     0       65100 65004 i

```

##### Leaf2 auto-discovery:
```
Leaf2#show bgp evpn route-type auto-discovery
BGP routing table information for VRF default
Router identifier 2.2.2.2, local AS number 65002
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 1.1.1.1:10 auto-discovery 0 0000:0000:0000:0000:1212
                                 1.1.1.1               -       100     0       65100 65001 i
 *  ec    RD: 1.1.1.1:10 auto-discovery 0 0000:0000:0000:0000:1212
                                 1.1.1.1               -       100     0       65100 65001 i
 * >Ec    RD: 1.1.1.1:20 auto-discovery 0 0000:0000:0000:0000:1212
                                 1.1.1.1               -       100     0       65100 65001 i
 *  ec    RD: 1.1.1.1:20 auto-discovery 0 0000:0000:0000:0000:1212
                                 1.1.1.1               -       100     0       65100 65001 i
 * >      RD: 2.2.2.2:10 auto-discovery 0 0000:0000:0000:0000:1212
                                 -                     -       -       0       i
 * >      RD: 2.2.2.2:20 auto-discovery 0 0000:0000:0000:0000:1212
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.1.1:1 auto-discovery 0000:0000:0000:0000:1212
                                 1.1.1.1               -       100     0       65100 65001 i
 *  ec    RD: 1.1.1.1:1 auto-discovery 0000:0000:0000:0000:1212
                                 1.1.1.1               -       100     0       65100 65001 i
 * >      RD: 2.2.2.2:1 auto-discovery 0000:0000:0000:0000:1212
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.1.1:10 auto-discovery 0 0000:0000:0000:0000:1313
                                 1.1.1.1               -       100     0       65100 65001 i
 *  ec    RD: 1.1.1.1:10 auto-discovery 0 0000:0000:0000:0000:1313
                                 1.1.1.1               -       100     0       65100 65001 i
 * >Ec    RD: 1.1.1.1:20 auto-discovery 0 0000:0000:0000:0000:1313
                                 1.1.1.1               -       100     0       65100 65001 i
 *  ec    RD: 1.1.1.1:20 auto-discovery 0 0000:0000:0000:0000:1313
                                 1.1.1.1               -       100     0       65100 65001 i
 * >      RD: 2.2.2.2:10 auto-discovery 0 0000:0000:0000:0000:1313
                                 -                     -       -       0       i
 * >      RD: 2.2.2.2:20 auto-discovery 0 0000:0000:0000:0000:1313
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.1.1:1 auto-discovery 0000:0000:0000:0000:1313
                                 1.1.1.1               -       100     0       65100 65001 i
 *  ec    RD: 1.1.1.1:1 auto-discovery 0000:0000:0000:0000:1313
                                 1.1.1.1               -       100     0       65100 65001 i
 * >      RD: 2.2.2.2:1 auto-discovery 0000:0000:0000:0000:1313
                                 -                     -       -       0       i

```

##### Leaf1 ethernet-segment:
```
Leaf1#show bgp evpn route-type ethernet-segment
BGP routing table information for VRF default
Router identifier 1.1.1.1, local AS number 65001
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >      RD: 1.1.1.1:1 ethernet-segment 0000:0000:0000:0000:1212 1.1.1.1
                                 -                     -       -       0       i
 * >Ec    RD: 2.2.2.2:1 ethernet-segment 0000:0000:0000:0000:1212 2.2.2.2
                                 2.2.2.2               -       100     0       65100 65002 i
 *  ec    RD: 2.2.2.2:1 ethernet-segment 0000:0000:0000:0000:1212 2.2.2.2
                                 2.2.2.2               -       100     0       65100 65002 i
 * >      RD: 1.1.1.1:1 ethernet-segment 0000:0000:0000:0000:1313 1.1.1.1
                                 -                     -       -       0       i
 * >Ec    RD: 2.2.2.2:1 ethernet-segment 0000:0000:0000:0000:1313 2.2.2.2
                                 2.2.2.2               -       100     0       65100 65002 i
 *  ec    RD: 2.2.2.2:1 ethernet-segment 0000:0000:0000:0000:1313 2.2.2.2
                                 2.2.2.2               -       100     0       65100 65002 i
```

##### Leaf2 ethernet-segment:
```
Leaf2#show bgp evpn route-type ethernet-segment
BGP routing table information for VRF default
Router identifier 2.2.2.2, local AS number 65002
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 1.1.1.1:1 ethernet-segment 0000:0000:0000:0000:1212 1.1.1.1
                                 1.1.1.1               -       100     0       65100 65001 i
 *  ec    RD: 1.1.1.1:1 ethernet-segment 0000:0000:0000:0000:1212 1.1.1.1
                                 1.1.1.1               -       100     0       65100 65001 i
 * >      RD: 2.2.2.2:1 ethernet-segment 0000:0000:0000:0000:1212 2.2.2.2
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.1.1:1 ethernet-segment 0000:0000:0000:0000:1313 1.1.1.1
                                 1.1.1.1               -       100     0       65100 65001 i
 *  ec    RD: 1.1.1.1:1 ethernet-segment 0000:0000:0000:0000:1313 1.1.1.1
                                 1.1.1.1               -       100     0       65100 65001 i
 * >      RD: 2.2.2.2:1 ethernet-segment 0000:0000:0000:0000:1313 2.2.2.2
                                 -                     -       -       0       i
```



#### Проверка Ping:

##### С CE1 на CE3:
1) Запускаем пинг с интервалом 0.5 сек
```
ubuntu@ubuntu-cloud:~$ ping 192.168.30.1 -i 0.5
PING 192.168.30.1 (192.168.30.1) 56(84) bytes of data.
64 bytes from 192.168.30.1: icmp_seq=1 ttl=62 time=11.8 ms
64 bytes from 192.168.30.1: icmp_seq=2 ttl=62 time=11.8 ms
64 bytes from 192.168.30.1: icmp_seq=3 ttl=62 time=12.0 ms
64 bytes from 192.168.30.1: icmp_seq=4 ttl=62 time=13.6 ms
64 bytes from 192.168.30.1: icmp_seq=5 ttl=62 time=12.0 ms
64 bytes from 192.168.30.1: icmp_seq=6 ttl=62 time=12.1 ms
64 bytes from 192.168.30.1: icmp_seq=7 ttl=62 time=12.6 ms
64 bytes from 192.168.30.1: icmp_seq=8 ttl=62 time=12.1 ms
64 bytes from 192.168.30.1: icmp_seq=9 ttl=62 time=13.2 ms
64 bytes from 192.168.30.1: icmp_seq=10 ttl=62 time=12.4 ms
64 bytes from 192.168.30.1: icmp_seq=11 ttl=62 time=14.2 ms
64 bytes from 192.168.30.1: icmp_seq=12 ttl=62 time=13.2 ms
64 bytes from 192.168.30.1: icmp_seq=13 ttl=62 time=12.4 ms
64 bytes from 192.168.30.1: icmp_seq=14 ttl=62 time=14.0 ms
64 bytes from 192.168.30.1: icmp_seq=15 ttl=62 time=14.1 ms
64 bytes from 192.168.30.1: icmp_seq=16 ttl=62 time=12.9 ms
64 bytes from 192.168.30.1: icmp_seq=17 ttl=62 time=12.3 ms
64 bytes from 192.168.30.1: icmp_seq=18 ttl=62 time=12.1 ms
64 bytes from 192.168.30.1: icmp_seq=19 ttl=62 time=12.8 ms
64 bytes from 192.168.30.1: icmp_seq=20 ttl=62 time=12.3 ms
64 bytes from 192.168.30.1: icmp_seq=21 ttl=62 time=11.7 ms
64 bytes from 192.168.30.1: icmp_seq=22 ttl=62 time=11.5 ms
64 bytes from 192.168.30.1: icmp_seq=23 ttl=62 time=11.9 ms
64 bytes from 192.168.30.1: icmp_seq=24 ttl=62 time=11.9 ms
64 bytes from 192.168.30.1: icmp_seq=25 ttl=62 time=12.7 ms
```
2) Здесь отключаем линк, через который идёт траффик (то, что он идёт именно через Leaf1, а не через Leaf2 был установлено предварительно по счётчикам на интерфейсах)

```
Leaf1#configure t
Leaf1(config)#interface Ethernet 11
Leaf1(config-if-Et11)#shut
```

Наблюдаем за потерями:
```
64 bytes from 192.168.30.1: icmp_seq=26 ttl=62 time=12.3 ms
64 bytes from 192.168.30.1: icmp_seq=31 ttl=62 time=17.6 ms
```

Т.е. пропущено 5 пакетов с интервало 0.5, время потерь траффика на отключении линка - порядка 2.5 секунд.
Наблюдалем далее
```
64 bytes from 192.168.30.1: icmp_seq=32 ttl=62 time=21.3 ms
64 bytes from 192.168.30.1: icmp_seq=33 ttl=62 time=16.4 ms
64 bytes from 192.168.30.1: icmp_seq=34 ttl=62 time=18.0 ms
64 bytes from 192.168.30.1: icmp_seq=35 ttl=62 time=17.4 ms
64 bytes from 192.168.30.1: icmp_seq=36 ttl=62 time=15.3 ms
64 bytes from 192.168.30.1: icmp_seq=37 ttl=62 time=17.9 ms
64 bytes from 192.168.30.1: icmp_seq=38 ttl=62 time=16.5 ms
64 bytes from 192.168.30.1: icmp_seq=39 ttl=62 time=15.8 ms
64 bytes from 192.168.30.1: icmp_seq=40 ttl=62 time=15.3 ms
64 bytes from 192.168.30.1: icmp_seq=41 ttl=62 time=16.4 ms
64 bytes from 192.168.30.1: icmp_seq=42 ttl=62 time=16.7 ms
```
3) В этот момент включаем линк обратно
```
Leaf1(config-if-Et11)#no shutdown
Leaf1(config-if-Et11)#
```

4) Наблюдаем
```
64 bytes from 192.168.30.1: icmp_seq=43 ttl=62 time=16.4 ms
64 bytes from 192.168.30.1: icmp_seq=83 ttl=62 time=22.1 ms
64 bytes from 192.168.30.1: icmp_seq=84 ttl=62 time=13.1 ms
64 bytes from 192.168.30.1: icmp_seq=85 ttl=62 time=13.6 ms
64 bytes from 192.168.30.1: icmp_seq=86 ttl=62 time=14.5 ms
64 bytes from 192.168.30.1: icmp_seq=87 ttl=62 time=11.4 ms
```

Время восстановления=(83-43)*0.5 = 20 сек.
Довольно много, скорее всего, вызвано тем, что на Leaf1/Leaf2 PortChannelы созданы с параметрами lacp по умолчанию, т.е. LACP-сигнализация от Leaf к CE ходит каждые 30 секунд и интерфейс PortChannel поднимается значительное время. Это значит, что время восстановления можно значительно снизить, если настроить LACP в fast-режим. На текущий момент не нашёл как это сделать на VeOS.





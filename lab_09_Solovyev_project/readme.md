### Проектная работа
Собираем L3 VXLAN/EVPN Multisite

#### Адресация Site-1

##### Адресация фабрики Site-1

###### Spine-ы Site-1:

|Device|Interface|IP Address|Subnet Mask|Description| AS
|---|---|---|---|---|---|
Spine1-s1|Loopback0|1.111.111.111|255.255.255.255|router_id_Spine1| 65100
Spine1-s1|Ethernet1|10.1.1.1|255.255.255.252|to_Leaf1 | 65100
Spine1-s1|Ethernet2|10.1.2.1|255.255.255.252|to_Leaf2 | 65100
Spine1-s1|Ethernet3|10.1.3.1|255.255.255.252|to_Leaf3 | 65100
Spine1-s1|Ethernet4|10.1.4.1|255.255.255.252|to_leaf4 | 65100
Spine1-s1|Ethernet5|10.1.5.1|255.255.255.252|to_BGW1 | 65100
Spine1-s1|Ethernet6|10.1.6.1|255.255.255.252|to_BGW2 | 65100
Spine2-s1|Loopback0|1.222.222.222|255.255.255.255|router_id_Spine2|65100
Spine2-s1|Ethernet1|10.2.1.1|255.255.255.252|to_Leaf1 | 65100
Spine2-s1|Ethernet2|10.2.2.1|255.255.255.252|to_Leaf2 | 65100
Spine2-s1|Ethernet3|10.2.3.1|255.255.255.252|to_Leaf3 | 65100
Spine2-s1|Ethernet4|10.2.4.1|255.255.255.252|to_leaf4 | 65100
Spine2-s1|Ethernet5|10.2.5.1|255.255.255.252|to_BGW1 | 65100
Spine2-s1|Ethernet6|10.2.6.1|255.255.255.252|to_BGW2 | 65100

###### Leaf-ы Site-1 : 

|Device|Interface|IP Address|Subnet Mask|Description| AS
|---|---|---|---|---|---|
Leaf1-s1|Loopback0|1.1.1.1|255.255.255.255|router_id_Leaf1|65001
Leaf1-s1|Ethernet1|10.1.1.2|255.255.255.252|to_Spine1|65001
Leaf1-s1|Ethernet2|10.02.1.2|255.255.255.252|to_Spine2|65001
Leaf2-s1|Loopback0|1.2.2.2|255.255.255.255|router_id_Leaf2|65002
Leaf2-s1|Ethernet1|10.1.2.2|255.255.255.252|to_Spine1|65002
Leaf2-s1|Ethernet2|10.2.2.2|255.255.255.252|to_Spine2|65002
Leaf3-s1|Loopback0|1.3.3.3|255.255.255.255|router_id_Leaf3|65003
Leaf3-s1|Ethernet1|10.1.3.2|255.255.255.252|to_Spine1|65003
Leaf3-s1|Ethernet2|10.2.3.2|255.255.255.252|to_Spine2|65003
Leaf4-s1|Loopback0|1.4.4.4|255.255.255.255|router_id_Leaf4|65004
Leaf4-s1|Ethernet1|10.1.4.2|255.255.255.252|to_Spine1|65004
Leaf4-s1|Ethernet2|10.2.4.2|255.255.255.252|to_Spine2|65004

###### BGW-ы Site-1 : 

|Device|Interface|IP Address|Subnet Mask|Description| AS
|---|---|---|---|---|---|
BGW1-s1|Loopback0|1.5.5.5|255.255.255.255|router_id_BGW1|65005
BGW1-s1|Ethernet1|10.1.5.2|255.255.255.252|to_Spine1|65005
BGW1-s1|Ethernet2|10.2.5.2|255.255.255.252|to_Spine2|65005
BGW1-s1|Ethernet3|172.16.0.1|255.255.255.252|to_Route-Server|65005
BGW2-s1|Loopback0|1.6.6.6|255.255.255.255|router_id_BGW1|65005
BGW2-s1|Ethernet1|10.1.6.2|255.255.255.252|to_Spine1|65005
BGW2-s1|Ethernet2|10.2.6.2|255.255.255.252|to_Spine2|65005
BGW2-s1|Ethernet3|172.16.0.5|255.255.255.252|to_Route-Server|65005


##### Downlinks Site-1
|Device|Interface|IP Address|Subnet Mask|Description| Mode
|---|---|---|---|---|---|
Leaf1-s1|Vlan10|192.168.10.254|255.255.255.0|to_CE1-s1| Anycast GW
Leaf1-s1|Vlan20|192.168.20.254|255.255.255.0|to_CE2-s1| Anycast GW
Leaf2-s1|Vlan10|192.168.10.254|255.255.255.0|to_CE1-s1| Anycast GW
Leaf2-s1|Vlan20|192.168.20.254|255.255.255.0|to_CE2-s1| Anycast GW
Leaf3-s1|Vlan30|192.168.30.100|255.255.255.0|to_CE3-s1| Single-homed
Leaf4-s1|vlan40|192.168.40.100|255.255.255.0|to_CE4-s1| Single-homed


##### Адресация CE Site-1
|Device|Interface|IP Address|Subnet Mask|Description|Vlan
|---|---|---|---|---|---|
CE1-s1|bond0(ens3+ens4)|192.168.10.1|255.255.255.0|to_Leaf1+Leaf2/Eth11 |vlan10
CE2-s1|bond0(ens3+ens4)|192.168.20.1|255.255.255.0|to_Leaf1+Leaf2/Eth12 |vlan20
CE3-s1|ens3|192.168.30.1|255.255.255.0|to_Leaf3_Eth12 |vlan30
CE4-s1|ens3|192.168.40.1|255.255.255.0|to_Leaf4_Eth12 |vlan40

#### Адресация Site-2

##### Адресация фабрики Site-2

###### Spine-ы Site-2:

|Device|Interface|IP Address|Subnet Mask|Description| AS
|---|---|---|---|---|---|
Spine1-s2|Loopback0|2.111.111.111|255.255.255.255|router_id_Spine1| 65200
Spine1-s2|Ethernet1|20.1.1.1|255.255.255.252|to_Leaf1 | 65200
Spine1-s2|Ethernet2|20.1.2.1|255.255.255.252|to_Leaf2 | 65200
Spine1-s2|Ethernet3|20.1.3.1|255.255.255.252|to_Leaf3 | 65200
Spine1-s2|Ethernet4|20.1.4.1|255.255.255.252|to_leaf4 | 65200
Spine1-s2|Ethernet5|20.1.5.1|255.255.255.252|to_BGW1 | 65200
Spine1-s2|Ethernet6|20.1.6.1|255.255.255.252|to_BGW2 | 65200
Spine2-s2|Loopback0|2.222.222.222|255.255.255.255|router_id_Spine2|65200
Spine2-s2|Ethernet1|20.2.1.1|255.255.255.252|to_Leaf1 | 65200
Spine2-s2|Ethernet2|20.2.2.1|255.255.255.252|to_Leaf2 | 65200
Spine2-s2|Ethernet3|20.2.3.1|255.255.255.252|to_Leaf3 | 65200
Spine2-s2|Ethernet4|20.2.4.1|255.255.255.252|to_leaf4 | 65200
Spine2-s2|Ethernet5|20.2.5.1|255.255.255.252|to_Leaf5 | 65200
Spine2-s2|Ethernet6|20.2.6.1|255.255.255.252|to_leaf6 | 65200

###### Leaf-ы Site-2 : 

|Device|Interface|IP Address|Subnet Mask|Description| AS
|---|---|---|---|---|---|
Leaf1-s1|Loopback0|2.1.1.1|255.255.255.255|router_id_Leaf1|65101
Leaf1-s1|Ethernet1|20.1.1.2|255.255.255.252|to_Spine1|65101
Leaf1-s1|Ethernet2|20.2.1.2|255.255.255.252|to_Spine2|65101
Leaf2-s1|Loopback0|2.2.2.2|255.255.255.255|router_id_Leaf2|65102
Leaf2-s1|Ethernet1|20.1.2.2|255.255.255.252|to_Spine1|65102
Leaf2-s1|Ethernet2|20.2.2.2|255.255.255.252|to_Spine2|65102
Leaf3-s1|Loopback0|2.3.3.3|255.255.255.255|router_id_Leaf3|65103
Leaf3-s1|Ethernet1|20.1.3.2|255.255.255.252|to_Spine1|65103
Leaf3-s1|Ethernet2|20.2.3.2|255.255.255.252|to_Spine2|65103
Leaf4-s1|Loopback0|2.4.4.4|255.255.255.255|router_id_Leaf4|65104
Leaf4-s1|Ethernet1|20.1.4.2|255.255.255.252|to_Spine1|65104
Leaf4-s1|Ethernet2|20.2.4.2|255.255.255.252|to_Spine2|65104


###### BGW-ы Site-2 : 

|Device|Interface|IP Address|Subnet Mask|Description| AS
|---|---|---|---|---|---|
BGW1-s2|Loopback0|2.5.5.5|255.255.255.255|router_id_BGW1|65105
BGW1-s2|Ethernet1|20.1.5.2|255.255.255.252|to_Spine1|65105
BGW1-s2|Ethernet2|20.2.5.2|255.255.255.252|to_Spine2|65105
BGW1-s2|Ethernet3|172.17.0.1|255.255.255.252|to_Route-Server|65105
BGW2-s2|Loopback0|2.6.6.6|255.255.255.255|router_id_BGW2|61006
BGW2-s2|Ethernet1|20.1.6.2|255.255.255.252|to_Spine1|65106
BGW2-s2|Ethernet2|20.2.6.2|255.255.255.252|to_Spine2|65106
BGW2-s2|Ethernet3|172.17.0.5|255.255.255.252|to_Route-Server|65106



##### Downlinks Site-2
|Device|Interface|IP Address|Subnet Mask|Description| Mode
|---|---|---|---|---|---|
Leaf1-s2|Vlan210|192.168.210.254|255.255.255.0|to_CE1-s2| Anycast GW
Leaf1-s2|Vlan220|192.168.220.254|255.255.255.0|to_CE2-s2| Anycast GW
Leaf2-s2|Vlan210|192.168.210.254|255.255.255.0|to_CE1-s2| Anycast GW
Leaf2-s2|Vlan220|192.168.220.254|255.255.255.0|to_CE2-s2| Anycast GW
Leaf3-s2|Vlan230|192.168.230.100|255.255.255.0|to_CE3-s2| Single-homed
Leaf4-s2|Vlan240|192.168.240.100|255.255.255.0|to_CE4-s2| Single-homed

##### Адресация CE Site-2

|Device|Interface|IP Address|Subnet Mask|Description|Vlan
|---|---|---|---|---|---|
CE1-s2|bond0(ens3+ens4)|192.168.210.1|255.255.255.0|to_Leaf1+Leaf2/Eth11 |vlan210
CE2-s2|bond0(ens3+ens4)|192.168.220.1|255.255.255.0|to_Leaf1+Leaf2/Eth12 |vlan220
CE3-s2|ens3|192.168.230.1|255.255.255.0|to_Leaf3_Eth12 |vlan230
CE4-s2|ens3|192.168.240.1|255.255.255.0|to_Leaf4_Eth12 |vlan240

##### Router server
|Device|Interface|IP Address|Subnet Mask|Description| AS
|---|---|---|---|---|---|
RS|Ethernet1|172.16.0.2|255.255.255.252|to_BGW1-s1|65300
RS|Ethernet2|172.16.0.6|255.255.255.252|to_BGW2-s1|65300
RS|Ethernet3|172.17.0.2|255.255.255.252|to_BGW1-s2|65300
RS|Ethernet4|172.17.0.6|255.255.255.252|to_BGW2-s2|65300




#### Конфиги:
Отдельно в приложенных файлах в папке "конфиги". Конфиги Spine и Leaf3-Leaf4 без изменений.

#### Маршруты:

##### Leaf1 auto-discovery:
```

```

##### Leaf2 auto-discovery:
```
                        -                     -       -       0       i

```

##### Leaf1 ethernet-segment:
```

```

##### Leaf2 ethernet-segment:
```

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





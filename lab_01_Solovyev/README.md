### Первое домашнее задание - адресный план


Данный адресный план перенесён из моей физической лаборатории на Булатах для EVPN-MH / Anycast-GW один в один. Для использования в продуктиве не предназначен в силу объективного недостатка - отсутствия масштабируемости адресного пространства, не предусмотренного дизайном лаборатории. 

Но для учебных целей с небольшим количеством устройств в фабрике такой адресации вполне достаточно + она довольно просто запоминается.


Обозначения Ethernet-портов (eth) условны и подлежат корректировки после сборки топологии в виртуальной лаборатории. Фабрика в виртуальной лаборатории GNS3 будет собрана на Arista VeOS.


Spine-ы:

|Device|Interface|IP Address|Subnet Mask|Description|Speed
|---|---|---|---|---|---|
Spine1|Loopback0|111.111.111.111|255.255.255.255|router_id_Spine1|-
Spine1|Ethernet1|111.1.1.1|255.255.255.252|to_Leaf1 | 100G
Spine1|Ethernet2|111.2.2.1|255.255.255.252|to_Leaf2 | 100G
Spine1|Ethernet3|111.3.3.1|255.255.255.252|to_Leaf3 | 100G
Spine1|Ethernet4|111.4.4.1|255.255.255.252|to_leaf4 | 100G
Spine2|Loopback0|222.222.222.222|255.255.255.255|router_id_Spine2|-
Spine2|Ethernet1|222.1.1.1|255.255.255.252|to_Leaf1 | 100G
Spine2|Ethernet2|222.2.2.1|255.255.255.252|to_Leaf2 | 100G
Spine2|Ethernet3|222.3.3.1|255.255.255.252|to_Leaf3 | 100G
Spine2|Ethernet4|222.4.4.1|255.255.255.252|to_leaf4 | 100G

Leaf-ы: 

|Device|Interface|IP Address|Subnet Mask|Description|Speed
|---|---|---|---|---|---|
Leaf1|Loopback0|1.1.1.1|255.255.255.255|router_id_Leaf1|-
Leaf1|Loopback1|1.1.1.11|255.255.255.255|vtep_source_Leaf1|-
Leaf1|Ethernet1|111.1.1.2|255.255.255.252|to_Spine1|100G
Leaf1|Ethernet2|222.1.1.2|255.255.255.252|to_Spine2|100G
Leaf2|Loopback0|2.2.2.2|255.255.255.255|router_id_Leaf2|-
Leaf2|Loopback1|2.2.2.22|255.255.255.255|vtep_source_Leaf2|-
Leaf2|Ethernet1|111.2.2.2|255.255.255.252|to_Spine1|100G
Leaf2|Ethernet2|222.2.2.2|255.255.255.252|to_Spine2|100G
Leaf3|Loopback0|3.3.3.3|255.255.255.255|router_id_Leaf3|-
Leaf3|Loopback1|3.3.3.33|255.255.255.255|vtep_source_Leaf3|-
Leaf3|Ethernet1|111.3.3.2|255.255.255.252|to_Spine1|100G
Leaf3|Ethernet2|222.3.3.2|255.255.255.252|to_Spine2|100G
Leaf4|Loopback0|4.4.4.4|255.255.255.255|router_id_Leaf4|-
Leaf4|Loopback1|4.4.4.44|255.255.255.255|vtep_source_Leaf4|-
Leaf4|Ethernet1|111.4.4.2|255.255.255.252|to_Spine1|100G
Leaf4|Ethernet2|222.4.4.2|255.255.255.252|to_Spine2|100G


### Третье домашнее задание - ISIS


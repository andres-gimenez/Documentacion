#Router

## Enrutamiento estatico

Para enrutar dentro de un router, hay que especificar la IP de la red, la mascara y el destino.

``` cisco ios
Router>enable
Router#configure terminal
Reouter(config)# ip route 13.0.0.0 255.0.0.0 12.0.0.2
```

Para poder ver las rutas que tiene 

``` cisco ios
Router#show ip route 
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

S    192.168.0.0/24 [1/0] via 192.168.11.1
C    192.168.11.0/24 is directly connected, FastEthernet1/0
C    192.168.21.0/24 is directly connected, Serial2/0
S    192.168.200.0/24 [1/0] via 192.168.21.1
```

## Enturamiento dinamico

Se utiliza el comando Router rip para realizar enrutamiento dinamico y
especificamos todas las redes que queremos unir.

``` cisco ios
Router(config)#router rip
Router(config-router)#version 2
Router(config-router)#no auto-summary
Router(config-router)#network 192.168.1.0
Router(config-router)#network 162.16.0.0
Router(config-router)#network 192.168.2.0
Router(config-router)#exit
Router(config)#
```
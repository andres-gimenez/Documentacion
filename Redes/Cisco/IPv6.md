# Configurar un router IPv6

Para configurar un router con IPV6

``` cisco
Router>en
Router#config te
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#ipv6 unicast-routing
Router(config)#interface FastEthernet0/0
Router(config-if)#ipv6 address 2001:BD4:ABCD:1111::1/64
Router(config-if)#ipv6 FE80::1 link-local
Router(config-if)#no shutdown
Router(config-if)#interface FastEthernet0/1
Router(config-if)#ipv6 address 2001:BD4:ABCD:1112::1/64
Router(config-if)#ipv6 FE80::1 link-local
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#exit
Router#
```

## Ver configuración

Para ver la configuración del router

``` cisco
Router>en
Router#config te
Router#show ipv6 interface brief
FastEthernet0/0            [up/up]
    FE80::207:ECFF:FEA7:6701
    2001:BD4:ABCD:1111::1
FastEthernet0/1            [up/up]
    FE80::207:ECFF:FEA7:6702
    2001:BD4:ABCD:1112::1
Vlan1                      [administratively down/down]
    unassigned
```

Para ver la configuración de un interface

``` cisco
Router#show ipv6 interface fa0/1
FastEthernet0/1 is up, line protocol is up
  IPv6 is enabled, link-local address is FE80::207:ECFF:FEA7:6702
  No Virtual link-local address(es):
  Global unicast address(es):
    2001:BD4:ABCD:1112::1, subnet is 2001:BD4:ABCD:1112::/64
  Joined group address(es):
    FF02::1
    FF02::2
    FF02::1:FF00:1
    FF02::1:FFA7:6702
  MTU is 1500 bytes
  ICMP error messages limited to one every 100 milliseconds
  ICMP redirects are enabled
  ICMP unreachables are sent
  ND DAD is enabled, number of DAD attempts: 1
  ND reachable time is 30000 milliseconds
  ND advertised reachable time is 0 (unspecified)
  ND advertised retransmit interval is 0 (unspecified)
  ND router advertisements are sent every 200 seconds
  ND router advertisements live for 1800 seconds
  ND advertised default router preference is Medium
  Hosts use stateless autoconfig for addresses.
```

Para ver la configuración de enrutamiento

``` cisco
Router#show ipv6 route
IPv6 Routing Table - 5 entries
Codes: C - Connected, L - Local, S - Static, R - RIP, B - BGP
       U - Per-user Static route, M - MIPv6
       I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea, IS - ISIS summary
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF intra, OI - OSPF inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2
       D - EIGRP, EX - EIGRP external
C   2001:BD4:ABCD:1111::/64 [0/0]
     via ::, FastEthernet0/0
L   2001:BD4:ABCD:1111::1/128 [0/0]
     via ::, FastEthernet0/0
C   2001:BD4:ABCD:1112::/64 [0/0]
     via ::, FastEthernet0/1
L   2001:BD4:ABCD:1112::1/128 [0/0]
     via ::, FastEthernet0/1
L   FF00::/8 [0/0]
     via ::, Null0
```

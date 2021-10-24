# Configurar un swich capa 3

## Crear las vlan

Comandos para crear un vlan.

```
Switch#vlan database
Switch(vlan)#vlan 10 name vlan10
Switch(vlan)#vlan 20 name vlan20
Switch(vlan)#exit
```

Secuencia completa.

```
Switch>
Switch>en
Switch#
Switch#vlan database
% Warning: It is recommended to configure VLAN from config mode,
  as VLAN database mode is being deprecated. Please consult user
  documentation for configuring VTP/VLAN in config mode.

Switch(vlan)#vlan 10 name vlan10
VLAN 10 added:
    Name: vlan10
Switch(vlan)#vlan 20 name vlan20
VLAN 20 added:
    Name: vlan20
Switch(vlan)#
Switch(vlan)#exit
APPLY completed.
Exiting....
Switch#vlan database
% Warning: It is recommended to configure VLAN from config mode,
  as VLAN database mode is being deprecated. Please consult user
  documentation for configuring VTP/VLAN in config mode.

Switch(vlan)#
Switch#
```

Comando para mostra configuración de vlan

```
Switch>
Switch>en
Switch#show vlan
```

## Configurar interface (vlan)

Asignar un vlan a un interface en modo access.

```
Switch>
Switch>en
Switch#config t
Switch(config)#int fa0/1
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 10
Switch(config-if)#exit
Switch(config)#exit
```

Asignar un vlan a un interface en modo trunk.

```
Switch#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#int fa0/1
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk allowed vlan all
Switch(config-if)#exit
Switch(config)#exit
Switch#
```

## Configurar shwich capa 3

Avilitamos que el ruter funcione como capa 3

```
Switch>en
Switch#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#ip routing
```

Configuramos cada vlan en sus respectivas interfaces.

```
Switch>en
Switch#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#ip routing
Switch(config)#int fa0/1
Switch(config-if)#switchport trunk encapsulation dot1q
Switch(config-if)#switchport mode trunk
Switch(config-if)#
```

Configuramos puerto que se conecta al router y le damos una IP.

```
Switch>en
Switch#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#interface FastEthernet0/4
Switch(config-if)#exit
Switch(config)#int fa0/5
Switch(config-if)#no switchport
Switch(config-if)#ip address 192.168.100.242 255.255.255.240
Switch(config-if)#duplex auto
Switch(config-if)#speed auto
Switch(config-if)#no shutdown
Switch(config-if)#exit
Switch(config)#exit
```

Configuramos el router

```
Router(config)#int fa0/0 
Router(config-if)#ip address 192.168.100.241 255.255.255.240
Router(config-if)#duplex auto
Router(config-if)#speed auto
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#exit
Router#
```

Seteamos las IP a las vlan (en los switch normales).

```
Router(config)#int vlan10 
Router(config-if)#ip address 192.168.100.1 255.255.255.192
Router(config)#int vlan20 
Router(config-if)#ip address 192.168.100.65 255.255.255.192
Router(config)#int vlan30 
Router(config-if)#ip address 192.168.100.193 255.255.255.240
Router(config)#int vlan40 
Router(config-if)#ip address 192.168.100.225 255.255.255.240
Router(config-if)#exit
Router(config)#exit
Router#
```
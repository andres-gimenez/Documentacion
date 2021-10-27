# Cisco IOS CLI


Entrar en configuración

```
Router>enable
Router#configure terminal
Router(config)#
```

Cambiar nombre de host

```
Router(config)#hostname R1
R1(config)#
```

Guardar configuración.

```
R1>enable
R1#copy running-config startup-config
Destination filename [startup-config]? startup-config
Building configuration...
[OK]
```

Borrar configuración

```
Router#erase startup-config
Erasing the nvram filesystem will remove all configuration files! Continue? [confirm]
[OK]
```

Reiniciar

```
Router#reload
Proceed with reload? [confirm]ySystem Bootstrap, Version 12.1(3r)T2, RELEASE SOFTWARE (fc1)
Copyright (c) 2000 by cisco Systems, Inc.
Initializing memory for ECC
..
PT1000 processor with 524288 Kbytes of main memory
Main memory is configured to 64 bit mode with ECC enabled

Readonly ROMMON initialized

Self decompressing the image :
########################################################################## [OK]

```

Mostrar información

```
R1#show running-config
Building configuration...

Current configuration : 742 bytes
!
version 15.1
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname R1
!
!
```

Asignar IP a la interface

```
Router>enable
Router#configure terminal
Router(config)#interface ethernet 0/0
Router(config-if)#no shutdown
Router(config-if)#ip address 20.0.0.1 255.0.0.0
Router(config-if)#duplex auto
Router(config-if)#speed auto
Router(config-if)#exit
Router(config)#exit
Router#
```
# Cisco IOS CLI

## Comandos basicos

#### Entrar en configuración

```
Router>enable
Router#configure terminal
Router(config)#
```

#### Cambiar nombre de host

```
Router(config)#hostname R1
R1(config)#
```

#### Guardar configuración.

Para guardar la configuración sin reiniciar el dispositivo, 
se debera copiar el fichero *running-config* a *startup-config*.

```
R1>enable
R1#copy running-config startup-config
Destination filename [startup-config]? startup-config
Building configuration...
[OK]
```

Se puede hacer de una forma abreviada de la siguiente forma.

```
R2>enable
R2#copy run start
Destination filename [startup-config]? 
Building configuration...
[OK]
```

#### Borrar configuración

Para borrar toda la configuración de un dispositivo y 
dejarlo como su estado de fabrica utilizaremos.

```
Router#erase startup-config
Erasing the nvram filesystem will remove all configuration files! Continue? [confirm]
[OK]
```

#### Reiniciar

Se puede reinicar el dispositivo con el comando **reload** y
guardar los cambios de la sesión.

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

### Mostrar información

El comando **show** nos muestra el estado de configuración del dispositivo.


#### Configuración actual

Para mostar la configuración que está ejecutando el dispositivo 
debemos mostrar la configuración en el fichero *running-config*.

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


#### Configuración guardada

Para mostar la configuración con la que se iniciara el dispositivo 
debemos mostrar la configuración en el fichero *startup-config*.

```
R2#show startup-config
Using 589 bytes
!
version 12.2
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname R2
!
!
!
```

## Interfaces


### Asignar una IP a un interface

Para asignar IP a una interface.

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



## Banner

Se puedo poner un mensaje para cuando se inicie el dispositivo.

```
Router>enable
Router#configure terminal
Router(config)#banner motd #No esta autorizado a entrar en este dispositivo.
Router(config)#banner login #No esta autorizado a entrar en este dispositivo.
Router#
```
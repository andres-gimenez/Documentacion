# Configuración acceso

Configuración de acceso a un dispositivo.

## Configuración acceso via consola

Para entrar a un dispositivo via consola y administra el acceso.

``` cisco ios
Switch>enable 
Switch#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#line console 0
Switch(config-line)#password cisco
Switch(config-line)#login
Switch(config-line)#end
Switch#
```

## Configuración de acceso via telnet

Creamos un usuario con acceso via terminal.

``` cisco ios
Switch>enable
Switch#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#line vty 0 15
Switch(config-line)#no login
Switch(config-line)#login local
Switch(config-line)#username <<usuario>> password <<1234>>
Switch(config)#username usuario privilege 15
Switch(config)#
```

Asignamos una IP para la interface

``` cisco ios
Switch>enable
Switch#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#interface vlan1
Switch(config-if)#ip addres 192.168.0.3 255.255.255.0
Switch(config-if)#no shutdown
```

## Banner

Se puedo poner un mensaje para cuando se inicie el dispositivo.

``` cisco ios
Router>enable
Router#configure terminal
Router(config)#banner motd #No esta autorizado a entrar en este dispositivo.
Router(config)#banner login #No esta autorizado a entrar en este dispositivo.
Router#
```

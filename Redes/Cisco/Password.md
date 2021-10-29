# Permisos del dispositivo

Los dispositivos de red, por motivos de seguridad, han de ir protegiso por una constreña de entrada.
Para extablecer dicha contraseña, escribiremos:

```
Router>enable
Router#configure terminal
Router(config)#line console 0
Router(config-line)#password cisco
Router(config-line)#login
Router(config-line)#exit
```

## Habilitar consola virtual

Para poder entrar a traves de una consola virtual via telnet. 
Primero asignamos un password a la consola virtual.

```
Router>enable
Router#configure terminal
Router(config)#line vty 0 4
Router(config-line)#password cisco
Router(config-line)#login
Router(config-line)#exit
```

Asociamos un interface a la consola virtual, a la que previamente le hemos asignado una IP a la intervaz.

```
Router>enable
Router#configure terminal
Router(config)#line vty 0 4
Router(config-line)#password cisco
Router(config-line)#login
Router(config-line)#exit
```

La password se puede ver con un comando show running-config, para que no se pueda ver se utiliza el comando secret.

```
Router>enable
Router#configure terminal
Router(config)#no enable password
Router(config)#enable secret cisco
Router(config)#exit
Router#
```

```
Router>enable
Router#configure terminal
Router(config)#service password-encryption
Router(config)#exit
Router#
```


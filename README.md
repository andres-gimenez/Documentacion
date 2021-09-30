---
title: Apuntes Linux
author: Andrés Giménez
description: Notas sobre linux.
no-loc: [Title, Document]
---

# Apuntes Linux

## Netplan (> Ubuntu17)
- Desaparece: ifconfig, traceroute, route, ...
- Nuevos: ip address show, ip route show,

- YAML
- Antes: /etc/network/interfaces
- Ahora: /etc/netplan

## IP Fija

> En el directorio /etc/netplan

Fichero 00-installer-config.yaml
``` yaml
network:
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: true
  version: 2
```

* Servicios (renderer): Network-Manager (Gestión grafica)
* Systemd-netwrokd Se configura con networkd de por comandos.

### Ejemplo de IP Fija

``` yaml
network:
  renderer: networkd
  ethernets:
    enp0s3:
      addresses: [192.168.1.2/24]
      gateway4: 192.168.1.1
      nameservers:
        search: [mydominio.com]
        addresses: [8.8.8.8, 8.8.4.4]
  version: 2
```
Para ver el nombre del adaptador:
> ip address show

Actualizar configuración, revisando la sintaxis:
> netplan apply
 
Actualizar sin aplicarlos los cambios.
>netplan generate

Para listar información 
>systemd-resolve --status

[Linux](Linux/Linux.md)
# Configuracion IP en Linux

## Netplan (> Ubuntu17)

- Desaparece: ifconfig, traceroute, route, ...
- Nuevos: ip address show, ip route show,
- YAML
- Antes: /etc/network/interfaces
- Ahora: /etc/netplan

## IP Fija

> En el directorio /etc/netplan

Fichero 00-installer-config.yaml

```yaml
network:
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: true
  version: 2
```

- Servicios (renderer): Network-Manager (Gestión grafica)
- Systemd-netwrokd Se configura con networkd de por comandos.

### Ejemplo de IP Fija

```yaml
network:
  renderer: networkd
  ethernets:
    enp0s3:
      addresses: [192.168.1.2/24]
      gateway4: 192.168.1.1
      nameservers:
        search: [dominio.intranet]
        addresses: [8.8.8.8, 8.8.4.4]
  version: 2
```

#### Para ver el nombre del adaptador

```shell
ip address show
```

#### Actualizar sin aplicarlos los cambios

```shell
netplan generate
```

#### Actualizar configuración, revisando la sintaxis

```shell
netplan apply
```

#### Para listar información

```shell
systemd-resolve --status
```

## Hardware

La configuración de los interfaces de red la podemos ver en la carpeta
*/sys/class/net*

Verificar que las interfaces de red funcionan correctamente

```shell
thtool lo |grep "Link detected"
ethtool eth0 |grep "Link detected"
ethtool eth1 |grep "Link detected"
```

# Montar Servidor DHCP en Ubuntu Server

### Configuración de red

Se ha de configurar un [IP fija en el servidor](./ConfiguracionIP.md)

### Instalar software de servidor DHCP

```linux
sudo apt install isc-dhcp-server -y
```

### Configurar

#### Abrir el fichero **dhcpd.conf** con tu editor favorito

Hay que modificar el fichero /etc/dhcp/dhcpd.conf

```linux
sudo nano dhcpd.conf
```

#### Modificar opciones nombre de dominio y nombre del servidor

``` conf
option domain-name "dominio.intranet";
option domain-name-servers ns1.dominio.intranet, ns2.dominio.intranet;
```

``` conf
option domain-name "dominiointranet";
option domain-name-servers 8.8.8.8, 8.8.4.4;
```

#### Agregamos la configuración de subred

``` conf
subnet 192.168.10.0 netmask 255.255.255.0 {
  range 192.168.10.100 192.168.10.200;
  option routers 192.168.10.1;
}
```

#### Aplicamos los cambios

``` linux
sudo systemctl restart isc-dhcp-server.service
```

#### Comandos dhcp server

Arrancar, parar o reiniciar el servicio

```linux
sudo service isc-dhcp-server [restart|start|stop|status]
```

```linux
sudo systemctl [restart|start|stop|status] isc-dhcp-server.service
```

#### Crear direcciones IP Fijas

```linux
host esxi02 {
  hardware ethernet 00:0c:29:c0:a0:19;
  fixed-address 10.1.1.12;
}
```

#### Comprobar lista de IP asignadas

```linux
dhcp-lease-list
```

## Comandos clientes

### Windows

#### Liberar configuración de red IP en Windows

```Command
ipconfig /release
```

#### Renueva configuración de red IP en Windows

```Command
ipconfig /renew
```

### Linux

#### Liberar configuración de red IP en Linux

```Command
sudo dhclient -r
```

#### Renueva configuración de red IP en Linux

```Command
sudo dhclient 
```

### MacOS

#### Listar interfaces de red

```Command
networksetup -listallhardwareports
```

#### Renueva configuración de red

Indicando la interface de red, ejemcutamos:

```Command
sudo ipconfig set en1 DHCP
```

#### Visualizar estructura de red

```Command
ipconfig getpacket en1
```

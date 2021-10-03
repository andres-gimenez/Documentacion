# Comandos de red para Ubuntu

## Comandos basicos

### Ping
Utiliza el protocolo ICMP para comprobar que un equipo está activo en la red. 
Tambien nos puede permitir resolver un nombre DNS.

```linux
ping [ip][nombre de dominio]
```

### Muestra configuración de red

```linux
ip a
```

### Configurar cliente DHCP
Liberar IP obtenida pod DHCP
```linux
sudo dhclient -r

sudo dhclient -r eth0
```
Refescar configuración DHCP
```linux
sudo dhclient

sudo dhclient eth0
```

### Chequear conexión
#### Comprobar DNS
```linux
ping google.com
```

## Configuración Wifi
> En el directorio /etc/netplan

Fichero *00-installer-config*.yaml

```yaml
network:
  renderer: networkd
  version: 2
  ethernets:
    enp0s3:
      dhcp4: true
  wifis:
    wlan0:
      dhcp4: true
      optional: true
      access-points:
        "SSID_name":
          password: "1234"
```


cute el siguiente comando de control del sistema en el shell de su terminal para iniciar la herramienta Wi-Fi Protected Access en su máquina Ubuntu.‎
```linux
sudo systemctl start wpa_supplicant
```
#### Para verificar las redes Wifi conocidas
```linux
nmcli con show
```
#### Para cambiar de red Wifi
```linux
nmcli con down ssid/uuid
```

## DNS


### NSLOOKUP
Existe tanto en Windows como Linux. 
Nos permite resolver nombres DNS, indicando la dirección IP a la que apunta y el servidor que nos ha respondido.


```command
nslookup [nombre dominio]
```

Para consultar los registors de recursos (RR) de un tipo concreto usamo **set type=[tipo]**

```command
nslookup 
> Set Type=NS
> upv.es
```

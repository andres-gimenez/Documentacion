# Montar Servidor DNS en Ubuntu Server

### Configuración de red

Se ha de configurar un [IP fija en el servidor](./ConfiguracionIP.md)

### Instalar software de servidor DHCP

```shell
sudo apt-get install bind9 bind9utils bind9-doc dnsutils
```

### Editamos el fichero de configuración

```shell
sudo nano /etc/bind/named.conf.options
```

<!-- Configuramos BIND en modo IPv4.
```json
OPTIONS="-4 -u bind"
``` -->

Configuramos la seción forwarder, con los servidores DNS

```
forwarders {
    8.8.8.8;
    8.8.4.4;
};
```

### Reiniciamos el servidor para aplicar los cambios.

```shell
sudo systemctl restart bind9
```

Para probar el tiempo de consulta podemos utilizar los comandos

```shell dig 
dig google.com
```

```shell nslookup
nslookup google.com
```

Consultar un tipo especifico de registro

```shell
C:> nslookup
Servidor predeterminado:  UnKnown
Address:  192.168.0.28
> set type=TXT
> google.es
Servidor:  UnKnown
Address:  192.168.0.28

Respuesta no autoritativa:
google.es       text =

        "v=spf1 -all"
```

Ahora bien, atención a los siguientes pasos. Vamos a crear un archivo donde guardaremos nuestros registros DNS:

Editamos el fichoero /etc/bind/named.conf.local para añadir una zona.

```shell
sudo nano /etc/bind/named.conf.local
```

Y insertamos el siguiente texto.

```
zone "example.com" {
type master;
file "/etc/bind/db.example.com";
};
```

Para comprobar que el fichero de configuración tiene el formato correcto

```shell
named-checkconf /etc/bind/named.conf.local
```

Creamos el fichero de zona, para ello hacemos una copia de db.local.

```shell
sudo cp /etc/bind/db.local /etc/bind/db.example.com
```

Editamos el fichero de zona

```shell
sudo nano /etc/bind/db.example.com
```

```
$TTL    604800
@       IN      SOA     ns1.example.com root.ns1.example.com (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL

; name servers - A records
@                         IN      NS     ns1.example.com.    
@                         IN      A      85.13.36.52
@                         IN      AAAA   2017:25AB::25
ns1                       IN      A      85.36.36.36
; A records
host1.example.com.        IN      A      10.128.100.101
host2.example.com.        IN      A      10.128.200.102
host3.example.com.        IN      A      10.128.200.103
host4                     IN      CNAME  
host1                     IN      TXT    Hola mundo
```

Para comprobar que el fichero tiene el formato correcto

```shell
named-checkzone example.com /etc/bind/db.example.com
```

Reiniciamos el servicio

```shell
sudo service bind9 restart
```

Para ver su estado del servicio

```shell
sudo service bind9 status
```

## Comandos clientes

### Windows

#### Liberal cache DNS en Windows

```shell
ipconfig /flushdns
```

### Linux

#### Liberal cache DNS en Linux

```Shell
sudo systemd-resolve --flush-caches
```

### MacOS

#### Liberal cache DNS en MacOS

```Shell
sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder
```

```Shell
sudo killall -HUP mDNSResponder;sudo killall mDNSResponderHelper;sudo dscacheutil -flushcache;say MacOS DNS cache has been cleared
```

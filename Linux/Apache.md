# Instalación del servidor web Apache

Apache está disponible en los repositorios de software predeterminados de Ubuntu, por lo que se instalará mediante herramientas de administración de paquetes convencionales:

Empiece por actualizar el índice de paquetes local para reflejar los cambios ascendentes más recientes.

``` bash
sudo apt-get update
```

Después Instale Apache.

``` bash
sudo apt-get install apache2 -y
```

Se debería iniciar de forma automática; el estado se puede comprobar mediante systemctl.

Bash

``` bash
sudo systemctl status apache2 --no-pager
```

El comando systemctl devuelve algo parecido a la salida siguiente.

Resultados

```
apache2.service - The Apache HTTP Server
   Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
  Drop-In: /lib/systemd/system/apache2.service.d
           └─apache2-systemd.conf
   Active: active (running) since Mon 2018-09-03 21:00:03 UTC; 1min 34s ago
 Main PID: 11156 (apache2)
    Tasks: 55 (limit: 4915)
   CGroup: /system.slice/apache2.service
           ├─11156 /usr/sbin/apache2 -k start
           ├─11158 /usr/sbin/apache2 -k start
           └─11159 /usr/sbin/apache2 -k start


test-web-eus-vm1 systemd[1]: Starting The Apache HTTP Server...
test-web-eus-vm1 apachectl[11129]: AH00558: apache2: Could not reliably determine the server's fully qua
test-web-eus-vm1 systemd[1]: Started The Apache HTTP Server.
```

Por último, podemos intentar recuperar la página predeterminada a través de la IP pública. Pero, aunque el servidor web se ejecute en la máquina virtual, no obtendrá ninguna conexión o respuesta válidas. ¿Sabe por qué?

Es necesario realizar un paso más para poder interactuar con el servidor web. La red virtual está bloqueando la solicitud entrante. Esto se puede cambiar mediante la configuración. A continuación veremos cómo permitir la solicitud de entrada.
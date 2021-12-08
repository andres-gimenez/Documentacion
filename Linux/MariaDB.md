# Instalación de Maria DB

MariaDB es un sistema de gestión de bases de datos derivado de MySQL con licencia GPL.

Para instalar Maria DB en Linux

Empiece por actualizar el índice de paquetes local para reflejar los cambios ascendentes más recientes.

``` bash
$ sudo apt-get update
```

Después Instale Apache.

``` bash
$ sudo apt install mariadb-server mariadb-client
```

Comprobamos el estado de MariaDB:

``` bash
sudo systemctl status mariadb
```

Habilitamos el inicio despues del arranque:

``` bash
sudo systemctl is-enabled mariadb
```

Configuramos la instalación de MariaDB con el siguiente comando:

``` bash
sudo mysql_secure_installation
```

Ingresamos la contraseña deseada para el usuario root y luego respondemos lo siguiente:
* Set a root password? [Y/n] y
* Remove anonymous users? [Y/n] y
* Disallow root login remotely? [Y/n] y
* Remove test database and access to it? [Y/n] y
* Reload privilege tables now? [Y/n] y
 
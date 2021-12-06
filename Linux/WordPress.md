# Instalar Word press en Ubuntu server.

Manual de instalación de WordPress sobre Ubuntu server 20.04LTS. 
WordPress esta basado en PHP y MySQL.

Para instalr PHP utilamos Apache o NGinx

## Instalar Apache

Instalamos servidor Apache.

## Descargamos WordPress

Descargamos la ultima versión oficial de WordPress

``` shell
sudo wget -c http://wordpress.org/latest.tar.gz
```

Descomprimimos el fichero descargado

``` shell
sudo tar -xzvf latest.tar.gz
```

Movemos el contenido descargado a la raíz del documento web, la cual está en /var/www/html/

``` shell
sudo cp -R wordpress /var/www/html/solvetic.lan
```

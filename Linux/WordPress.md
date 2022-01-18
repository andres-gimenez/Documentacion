# Instalar Word press en Ubuntu server

Manual de instalación de WordPress sobre Ubuntu server 20.04LTS.
WordPress esta basado en PHP y MySQL.

Para instalr PHP utilamos Apache o NGinx

## Instalar Apache

Instalamos servidor [Apache](Apache.md).

## Instalar MariaDB

Instalamos servidor [MariaDB](MariaDB.md).

## Instalamos los modulos necesarios de PHP para Apache y MySQL

``` shell
sudo apt install php libapache2-mod-php php-mysql
```

<!-- Se pueden instalar todos los componentes de php

``` shell
sudo apt install php-dom php-simplexml php-ssh2 php-xml php-xmlreader php-curl php-exif php-ftp php-gd php-iconv php-imagick php-json php-mbstring php-posix php-sockets php-tokenizer
``` -->

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
sudo cp -R wordpress/ /var/www/midominio/
```

## Configuramos MariaDB

Para configurar la base de datos MariaDB.

Iniciamos sesión en MariaDB con el siguiente comando y debemos ingresar la contraseña que hemos definido anteriormente:
Luego creamos la base de datos, el usuario y la contraseña a usar en el inicio de sesión de WordPress:

``` shell
sudo mysql -u root -p
CREATE DATABASE databasename;
CREATE USER 'usuario'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON databasename.* TO 'usuario'@'localhost';
CREATE USER 'usuario'@'192.0.2.255' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON databasename.* TO 'usuario'@'192.0.2.255';
FLUSH PRIVILEGES;
EXIT;
```

Para poder ver que se ha creado correctamente los usuario podemos usar:

``` shell
mysql -u usuario -p
status;
exit
```

Para ver lista de base de datos

```sql
show databases;
```

## Configuramos WorkPress

Vamos a la a la raíz de documentos del sitio web y allí vamos a crear un archivo wp-config.php tomando como origen el archivo de configuración de la siguiente manera:

``` shell
cd /var/www/mi_dominio
sudo cp wp-config-sample.php wp-config.php
```

Editamos el nuevo fichero de configuración

``` shell
sudo nano wp-config.php
```

Devemos modificar los campos:

* DB_NAME
* DB_USER
* DB_PASSWORD

Para obtener valores seguros del generador de claves secretas de WordPress, escriba lo siguiente:

``` shell
curl -s https://api.wordpress.org/secret-key/1.1/salt/
```

## Probamos que funciona

Desde el navegador, cargamos la WordPress y lo configuramos.

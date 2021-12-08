<!-- https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04-es -->

# Servidor web Nginx

## Instalación de Nginx

Empiece por actualizar el índice de paquetes local para reflejar los cambios ascendentes más recientes.

``` bash
$ sudo apt-get update
```

Instalamos el paquete de nginx

``` bash
sudo apt-get install nginx
```

## Servicio Nginx

Comandos para iniciar y parar Nginx

``` bash
$ sudo service nginx start | stop | restart
```

Para comprobar una configuración o recargar la configuración tras hacer cambios

``` bash
$ sudo service nginx configtest | reload
```

Para ver el estado del servicio

``` bash
$ sudo service status
```

``` bash
$ systemctl status nginx
```

## Configuración de Nginx

La configuración del servicio se encuentra en */etc/nginx/nginx.conf*

Dentro de este fichero vamos a poder configurar las funciones generales del servidor web, entre otros:

* El usuario que ejecutará el servidor.
* Número de procesos del servidor (según el número de núcleos de la CPU).
* El proceso maestro del servidor (pid)
* La ruta donde se guardarán los ficheros de log.
* Usuarios máximos conectados al servidor.
* Configuración http (tipos de ficheros, envío de datos, compresión Gzip, ruta de los servidores web, configuración del servidor de correo, etc).

``` text
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 768;
        # multi_accept on;
}

http {

        ##
        # Basic Settings
        ##

        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_timeout 65;
        types_hash_max_size 2048;
        # server_tokens off;

        # server_names_hash_bucket_size 64;
        # server_name_in_redirect off;

        include /etc/nginx/mime.types;
        default_type application/octet-stream;

        ##
        # SSL Settings
        ##

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLE
        ssl_prefer_server_ciphers on;

        ##
        # Logging Settings
        ##

        access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;

        ##
        # Gzip Settings
        ##

        gzip on;

        # gzip_vary on;
        # gzip_proxied any;
        # gzip_comp_level 6;
        # gzip_buffers 16 8k;
        # gzip_http_version 1.1;
        # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

        ##
        # Virtual Host Configs
        ##

        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
}


#mail {
#       # See sample authentication script at:
#       # http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
#
#       # auth_http localhost/auth.php;
#       # pop3_capabilities "TOP" "USER";
#       # imap_capabilities "IMAP4rev1" "UIDPLUS";
#
#       server {
#               listen     localhost:110;
#               protocol   pop3;
#               proxy      on;
#       }
#
#       server {
#               listen     localhost:143;
#               protocol   imap;
#               proxy      on;
#       }
#}
```

## Ajuste del cortafuegos

Es necesario ajustar el firewall para permitir el acceso externo a los puertos web predetermnados.

Durante la instalación, Apache se registra con UFW para proporcionar algunos perfiles de aplicación que pueden utilizarse para habilitar o deshabilitar el acceso a Apache a través del firewall.

Para activar el cortafuegos UFW 

``` shell
$ sudo ufw enable
```

Para enumerar los perfiles de aplicación ufw escribimos lo siguiente:

``` shell
$ sudo ufw app list
```

Obtendrá una lista de los perfiles de aplicación:

``` shell
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
```

Como lo indica el resultado, hay tres perfiles disponibles para Apache:

* **Nginx Full**: este perfil abre el puerto 80 (tráfico web normal no cifrado) y el puerto 443 (tráfico TLS/SSL cifrado)
* **Nginx HTTP**: este perfil abre solo el puerto 80 (tráfico web normal no cifrado)
* **Nginx HTTPS**: este perfil abre solo el puerto 443 (tráfico TLS/SSL cifrado)

Abrimos el perfil Apache Full que nos permite hacceder a traves de http y https.

``` shell
$ sudo ufw allow 'Nginx Full'
```

Comprobamos que se ha modificado la configuración

``` shell
$ sudo ufw app status
```

El resultado proporcionará una lista del tráfico de HTTP que se permite:

``` shell
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
ApacheFull                 ALLOW       Anywhere                
OpenSSH (v6)               ALLOW       Anywhere (v6)             
Apache (v6)                ALLOW       Anywhere (v6)
```

## Configurar bloques de servidor

Los bloques de servidor son el equivalente a los host virtuales de Apache.

Creamos la caprteta para nuestro servidor
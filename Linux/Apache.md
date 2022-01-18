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

``` bash
sudo systemctl status apache2 --no-pager
```

El comando systemctl devuelve algo parecido a la salida siguiente.

``` text
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

<!-- Por último, podemos intentar recuperar la página predeterminada a través de la IP pública. Pero, aunque el servidor web se ejecute en la máquina virtual, no obtendrá ninguna conexión o respuesta válidas. ¿Sabe por qué?

Es necesario realizar un paso más para poder interactuar con el servidor web. La red virtual está bloqueando la solicitud entrante. Esto se puede cambiar mediante la configuración. A continuación veremos cómo permitir la solicitud de entrada. -->

Habilitamos el inicio despues del arranque:

``` bash
sudo systemctl is-enabled apache2
```

## Ajuste del cortafuegos

Es necesario ajustar el firewall para permitir el acceso externo a los puertos web predetermnados.

Durante la instalación, Apache se registra con UFW para proporcionar algunos perfiles de aplicación que pueden utilizarse para habilitar o deshabilitar el acceso a Apache a través del firewall.

Para activar el cortafuegos UFW

``` shell
sudo ufw enable
```

Para enumerar los perfiles de aplicación ufw escribimos lo siguiente:

``` shell
sudo ufw app list
```

Obtendrá una lista de los perfiles de aplicación:

``` shell
Available applications:
  Apache
  Apache Full
  Apache Secure
  OpenSSH
```

Como lo indica el resultado, hay tres perfiles disponibles para Apache:

* **Apache**: este perfil abre solo el puerto 80 (tráfico web normal no cifrado)
* **Apache Full**: este perfil abre el puerto 80 (tráfico web normal no cifrado) y el puerto 443 (tráfico TLS/SSL cifrado)
* **Apache Secure**: este perfil abre solo el puerto 443 (tráfico TLS/SSL cifrado)

Abrimos el perfil Apache Full que nos permite hacceder a traves de http y https.

``` shell
sudo ufw allow 'Apache'
```

Comprobamos que se ha modificado la configuración

``` shell
sudo ufw app status
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

## Comporbar el servicio

Desde un navegador probamos que podemos navegar.

## Administrar el proceso de Apache

Para detener su servidor web, escriba lo siguiente:

``` shell
sudo systemctl stop apache2
```

Para iniciar el servidor web cuando no esté activo, escriba lo siguiente:

``` shell
sudo systemctl start apache2
```

Para detener y luego iniciar el servicio de nuevo, escriba lo siguiente:

``` shell
sudo systemctl restart apache2
```

Si solo realiza cambios de configuración, Apache a menudo puede recargarse sin cerrar conexiones. Para hacerlo, utilice este comando:

``` shell
sudo systemctl reload apache2
```

Por defecto, Apache está configurado para iniciarse automáticamente cuando el servidor lo hace. Si no es lo que quiere, deshabilite este comportamiento escribiendo lo siguiente:

``` shell
sudo systemctl disable apache2
 ```

Para volver a habilitar el servicio de modo que se cargue en el inicio, escriba lo siguiente:

``` shell
sudo systemctl enable apache2
```

## Configurar hosts virtuales

Al emplear el servidor web Apache, puede utilizar hosts virtuales para encapsular detalles de configuración y alojar más de un dominio desde un único servidor. 

Configuraremos un dominio llamado **tu_domain**, pero debería *cambiarlo por su propio nombre de dominio*.

Ubuntu 20.04 tiene habilitado un bloque de servidor por defecto, que está configurado para proporcionar documentos del directorio */var/www/html*.
Si bien esto funciona bien para un solo sitio, puede ser difícil de manejar si aloja varios. 
En vez de modificar */var/www/html*, vamos a crear una estructura de directorios dentro de */var/www* para un sitio **tu_domain** y 
dejaremos */var/www/html* como directorio predeterminado que se suministrará si una solicitud de cliente no coincide con otros sitios.

Cree el directorio para your_domain de la siguiente manera:

``` shell
sudo mkdir /var/www/tu_domain
```

A continuación, asigne la propiedad del directorio con la variable de entorno $USER:

``` shell
sudo chown -R $USER:$USER /var/www/your_domain
```

Los permisos de los roots web deberían ser correctos si no modificó el valor umask, que establece permisos de archivos predeterminados.
Para asegurarse de que sus permisos sean correctos y permitir al propietario leer, escribir y ejecutar los archivos, y a la vez conceder solo permisos de lectura y ejecución a los grupos y terceros, puede ingresar el siguiente comando:

``` shell
sudo chmod -R 755 /var/www/your_domain
```

A continuación, cree una página de ejemplo *index.html* utilizando nano o su editor favorito:

``` shell
sudo nano /var/www/your_domain/index.html
```

Dentro de ellao, podemos poner un ejemplo de este tipo.

``` html
<html>
    <head>
        <title>Welcome to Your_domain!</title>
    </head>
    <body>
        <h1>Success!  The your_domain virtual host is working!</h1>
    </body>
</html>
```

Para que Apache proporcione este contenido, es necesario crear un archivo de host virtual con las directivas correctas.
En lugar de modificar el archivo de configuración predeterminado situado en */etc/apache2/sites-available/000-default.conf* directamente,
vamos a crear uno nuevo en */etc/apache2/sites-available/tu_domain.conf*:

``` shell
sudo nano /etc/apache2/sites-available/tu_domain.conf
```

Péguelo en el siguiente bloque de configuración, similar al predeterminado, pero actualizado para nuestro nuevo directorio y nombre de dominio:

``` xml
<VirtualHost mi_dominio ó IP ó *:80>
    ServerAdmin webmaster@localhost
    ServerName tu_domain
    ServerAlias www.tu_domain
    DocumentRoot /var/www/tu_domain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Habilitaremos el archivo con la herramienta *a2ensite*:

``` shell
sudo a2ensite tu_domain.conf
```

Deshabilite el sitio predeterminado definido en 000-default.conf:

``` shell
sudo a2dissite 000-default.conf
```

A continuación, realizaremos una prueba para ver que no haya errores de configuración:

``` shell
sudo apache2ctl configtest
```

Reinicie Apache para implementar sus cambios:

``` shell
sudo systemctl restart apache2
```

Para arrancar apache2 en el arranque de Linux, podemos ejecutar:

``` shell
sudo systemctl is-enabled apache2
```

## Ficheros importantes de Apache

Ahora que sabe administrar el propio servicio de Apache, debe tomarse unos minutos para familiarizarse con algunos directorios y archivos importantes.

### Contenido

* */var/www/html*: el contenido web real, que por defecto solo consta de la página predeterminada de Apache que vio antes, se proporciona desde el directorio */var/www/html*. Esto se puede cambiar modificando los archivos de configuración de Apache.

### Configuración del servidor

* **/etc/apache2**: el directorio de configuración de Apache. En él se encuentran todos los archivos de configuración de Apache.
* **/etc/apache2/apache2.conf**: el archivo principal de configuración de Apache. Esto se puede modificar para realizar cambios en la configuración general de Apache. Este archivo administra la carga de muchos de los demás archivos del directorio de configuración.
* **/etc/apache2/ports.conf**: este archivo especifica los puertos en los que Apache escuchará. Por defecto, Apache escucha en el puerto 80. De forma adicional, lo hace en el 443 cuando se habilita un módulo que proporciona capacidades SSL.
* **/etc/apache2/sites-available/**: el directorio en el que se pueden almacenar hosts por sitio. Apache no utilizará los archivos de configuración de este directorio a menos que estén vinculados al directorio sites-enabled. Normalmente, toda la configuración de bloques de servidor se realiza en este directorio y luego se habilita al vincularse al otro directorio con el comando a2ensite.
* **/etc/apache2/sites-enabled/**: el directorio donde se almacenan hosts virtuales por sitio habilitados. Normalmente, se crean vinculando los archivos de configuración del directorio sites-available con a2ensite. Apache lee los archivos de configuración y los enlaces de este directorio cuando se inicia o se vuelve a cargar para compilar una configuración completa.
* **/etc/apache2/conf-available/ y /etc/apache2/conf-enabled/**: estos directorios tienen la misma relación que los directorios sites-available y sites-enabled, pero se utilizan para almacenar fragmentos de configuración que no pertenecen a un host virtual. Los archivos del directorio conf-available pueden habilitarse con el comando a2enconf y deshabilitarse con el comando a2disconf.
* **/etc/apache2/mods-available/ y /etc/apache2/mods-enabled/**: estos directorios contienen los módulos disponibles y habilitados, respectivamente. Los archivos que terminan en .load contienen fragmentos para cargar módulos específicos, mientras que los archivos que terminan en .conf contienen la configuración para esos módulos. Los módulos pueden habilitarse y deshabilitarse con los comandos a2enmod y a2dismod.

### Registros del servidor

* /var/log/apache2/access.log: por defecto, cada solicitud enviada a su servidor web se asienta en este archivo de registro a menos que Apache esté configurado para no hacerlo.
* /var/log/apache2/error.log: por defecto, todos los errores se registran en este archivo. La directiva LogLevel de la configuración de Apache especifica el nivel de detalle de los registros de error.

## Certificados de seguridad

Podemos crear un certificado de pruebas con openssl de la siguiente forma.

``` shell
openssl genrsa -des3 -out server.key 1024
openssl req -new -x509 -days 1825 -subj "/C=ES/ST=Spain/L=/O=/CN=midominio" -key server.key -out midominio.crt
```

Para utilizar ssl en Apache hay que activar el modulo ssl. Se puede hacer con el siguiente comando.

``` shell
sudo a2enmod ssl 
```

Para configurar el site en apache debemos modificar el fichero de configuración del sitio con el siguiente texto:

``` xml
<VirtualHost mi_dominio ó IP ó *:80>
...
Redirect permanent "/" "https://mi_dominio ó IP/"
...
</VirtualHost>
<VirtualHost mi_dominio ó IP ó *:443>
DocumentRoot /var/www/html2
ServerName su.dominio.com
SSLEngine on
SSLCertificateFile /ruta/a/su_dominio.crt
SSLCertificateKeyFile /ruta/a/su_dominio.key
SSLCertificateChainFile /ruta/a/DigiCertCA.crt
</VirtualHost>
```

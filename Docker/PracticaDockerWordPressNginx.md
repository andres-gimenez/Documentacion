# Instalación de WordPress

Instalación de WordPress con Docker compose

<!-- ## Requisitos

- Docker instalado en un Ubuntu Server 2021.
- mkcert para crear certificados SSL. 

### Instalar mkcert

Para instalas `mkcert` en Ubuntu Server, primero instalamos `certutil`

``` shell
sudo apt update
sudo apt install libnss3-tools
```

Y ahora instalamos `mkcert`.

``` shell
brew install mkcert
```-->

## Recomendaciones

Se recomienda montar servicio ssh en el servidor Ubuntu y acceder a el por medio de Windows Terminal y VisualStudio Code.

## Clonar repositorio git

Clonaremos el repositorio git, con los ficheros necesarios para montar los servicios.

``` shell
git clone https://github.com/andres-gimenez/wordpress-nginx-docker-c
ompose
```

## Certificados SSL

Para crear nuestros certificados de seguridad para pruebas

``` shell
openssl req –new –newkey rsa:2048 –nodes –keyout server.key –out server.csr
```

## Configuración

### Configuración para Docker

Primero configuramos las variables de entorno, para ello utilizamos como plantilla el fichero `.env.example` y lo renombramos con `.env`

  ***Nota**: Los ficheros `.env` contienen configuración sensible, como direcciones de servidores, usuarios y contraseñas, por eso no se deben subir nunca a un repositorio de codigo como git.*

Ejemplo:

```dotenv
IP=127.0.0.1
APP_NAME=myapp
DOMAIN="myapp.local"
DB_HOST=mysql
DB_NAME=myapp
DB_ROOT_PASSWORD=password
DB_TABLE_PREFIX=wp_
```

### Configuración para WordPress

Utiliza como plantilla el fichero `./src/.env.example` para crear el fichero `./src/.env` que será creado al ejecutar `docker-composer`

Ejemplo:

```dotenv
DB_NAME='myapp'
DB_USER='root'
DB_PASSWORD='password'

# Optionally, you can use a data source name (DSN)
# When using a DSN, you can remove the DB_NAME, DB_USER, DB_PASSWORD, and DB_HOST variables
# DATABASE_URL='mysql://database_user:database_password@database_host:database_port/database_name'

# Optional variables
DB_HOST='mysql'
# DB_PREFIX='wp_'

WP_ENV='development'
WP_HOME='https://myapp.local'
WP_SITEURL="${WP_HOME}/wp"
WP_DEBUG_LOG=/path/to/debug.log

# Generate your keys here: https://roots.io/salts.html
AUTH_KEY='generateme'
SECURE_AUTH_KEY='generateme'
LOGGED_IN_KEY='generateme'
NONCE_KEY='generateme'
AUTH_SALT='generateme'
SECURE_AUTH_SALT='generateme'
LOGGED_IN_SALT='generateme'
NONCE_SALT='generateme'
```


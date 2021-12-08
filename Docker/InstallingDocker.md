# Instalar Docker

Como instalar Docker en un Ubuntu 20.04

## Paso 1 - Configurar APT para que utilice el repositorio de Docker

Actualizar packers

``` shel
$ sudo apt update
```

Instalar algunos paquetes necesarios para usar paquetes a traves de https:

``` shell
$ sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

Añadir clabe GPG del repositorio oficial de Docker

``` shell
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Añadir el repositorio de Docker al las fuentes de APT.

``` shell
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
```

Esto también actualizará nuestra base de datos de paquetes con los paquetes Docker del repositorio recién agregado.

Asegúrese de que está a punto de instalar desde el repositorio de Docker en lugar del repositorio predeterminado de Ubuntu:

``` shell
$ apt-cache policy docker-ce
```

## Paso 2 - Instalamos Docker

Ahora instalamos Docker

``` shell
$ sudo apt install docker-ce
```

Comprobamos que el domonio de docker se está ejecutando

``` shell
$ sudo systemctl status docker
```

Probamos el comando docker, sin sudo.

``` shell
$ docker
```

Si no tenemos acceso a docker, podemos agregar nuestro usuario  al grupo de docker mediante el comando:

``` shell
$ sudo usermod -aG docker ${USER}
```

Para aplicar la nueva pertenencia al grupo, cierre la sesión en el servidor o aplique el comando

``` shell
$ su - ${USER}
```

Podemos comprobar que pertenecemos al grupo de docker con el comando

``` shell
$ groups
```

Probamos que funcionan los docker descargando y ejecutando hello-world.

``` shell
$ docker run hello-world
```

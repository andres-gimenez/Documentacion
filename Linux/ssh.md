# Instalar SSH en Ubuntu server


Podemos comprobar el estado del servicio SSH en Ubuntu 20.04 con el siguiente comando:

``` shell
sudo service ssh status
```

Instalar protocolo ssh server

``` shell
sudo apt-get install openssh-server
```

Instalar protocolo ssh client

``` shell
sudo apt-get install openssh-client
```

Configuración de ssh
``` shell
sudo nano /etc/ssh/sshd_config
```

Comprobamos los puertos

``` shell
$ sudo ufw app status
```

Abrimos el perfil OpenSSH para habrir el puerto 22.

``` shell
$ sudo ufw allow 'OpenSSH'
```
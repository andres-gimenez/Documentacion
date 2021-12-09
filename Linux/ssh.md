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

Activamos el firewar

``` shell
sudo ufw enable
```

Comprobamos los puertos

``` shell
sudo ufw status
```

Abrimos el perfil OpenSSH para habrir el puerto 22.

``` shell
sudo ufw allow 'OpenSSH'
```

## Conectamos con

``` ps
ssh host -l usuario
```

## Permisos

Cambiar permisos de una carpeta.

``` shell
sudo chown -R myuser /path/to/folder
```

``` shell
sudo chmod a+rwx /etc/nginx/sites-enabled/default
```

``` shell
sudo chmod -R 0444 /etc/nginx/sites-available/default
```

``` shell
sudo chown -R $USER:$USER /var/www/your_domain
```

``` shell
sudo setfacl -R -m u:usuario:rwx /etc/nginx/sites-available/default
```

## Crear clave privada para acceso a ssh

Generar una clave

``` shell
ssh-keygen -b 2048 -t rsa -f ~/.ssh/id_rsa-remote-ssh
```

Configuración Visual Studio Code

``` config
Host nombrehost
    HostName nombre host o IP
    User andres
    ForwardAgent yes
    Port 22
    IdentityFile  ruta/id_rsa-remote-ssh
```

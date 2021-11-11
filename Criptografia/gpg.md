# Utilizr gpg para cifrar archivos en Linux

<!-- Revisar
https://www.genbeta.com/desarrollo/manual-de-gpg-cifra-y-envia-datos-de-forma-segura -->

## Cifrado simetrico con gpg

Creamo un fichero y lo encriptamos con clave simetrica.
Tras ejecutar gpg nos saldrá un cuadro que nos pedirá una contraseña.

``` shell
echo "hola mundo" > text.txt
gpg -c texto.txt
```

Para descifra el texto podemos verlo con

``` shell
gpg -d texto.txt.gpg
```

## Cifrado asimetrico con gpg

### Generación de claves

Lo primero es generar un par de claves (publica y privada).

Forma abrebiada

``` shell
gpg --gen-key
```

Forma completa

``` shell
gpg --full-generate-key
```

### Ver las claves

Para poder ver las claves generadas.

``` shell
gpg --list-secret-keys
```

### Borrar clave

``` shell
gpg --delete-secret-key ClaveID
```

### Copia y distribución de claves

Podemos enviamos la clave publica a un repositorio publico. Podemos utilizar el repositorio pgp.rediris.es o pgp.mit.edu

``` shell
gpg --send-keys --keyserver pgp.rediris.es ClaveID
```

Podemos guardar la clave publica en un fichero

``` shell
gpg --armor --output MiClavePublica --export ClaveID
```

Podemos crear una copia de seguirdad de la clave privada.

``` shell
gpg --armor --output MiClavePrivada --export-secret-key ClaveID
```

Para registrar la clave publica en la máquina destinararia usuamos

``` shell
gpg --import ClavePublica
```

### Clave de revocación

Podemos generar una clave de revocación para el caso de que perdamos nuestra clave privada

``` shell
gpg -o revocacion.asc --gen-revoke ClaveID
```

Para venviar una clave de revocación a un servidor.

``` shell
gpg --keyserver pgp.rediris.es --send-keys ClaveID
```

### Cifrar un fichero

Para encriptar un fichero con nuestra clave publica

``` shell
gpg --encrypt --recipient ClaveID texto.txt
```

### Descifrar un fichero

Para descifrar un fichero con la clave privada

``` shell
gpg --d texto.txt.gpg
```

### Firmar un archivo

Para firma un fichero con la clave privada

``` shell
gpg -u ClaveID --output firmar.txt.gpg --sign firmar.txt 
```

Para verificar un fichero con la clave publica

``` shell
gpg --verify firma.txt.gpg
```

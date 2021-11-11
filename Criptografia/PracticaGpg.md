# Practica criptografia con GPG

GNU Privacy Guard (GnyPG o GPG) es una herramienta de cifrado y firma digital, está desarrollada como software libre con licencia GPL.

## Creamos de fichero para hacer pruebas

Lo primero es disponer de un fichero para poder hacer las practicas.

``` shell
echo "hola mundo" > text.txt
```

## Cifrado simétrico

La principal característica del cifrado simétrico es que se utiliza la misma clave para cifra y descifrar.

### Cifrado de un fichero con clave simétrica

Utilizamos la opción -c ó --symmetric para encriptar con clave simétrica.

``` shell
gpg -c texto.txt
```

``` shell
gpg -symmetric texto.txt
```

Nos pedirá una frase que utiliza para cifrar nuestro texto,
recuerda que cuanto más larga sea la frase, más segura será la encriptación.
Y tendremos el fichero texto.txt.gpg con el contenido cifrado, 
el cual debemos hacer llegar al destinatario.

### Descifrar de un fichero con clave simétrica

Cuando recibimos un fichero cifrado, debemos conocer la clave con la que se encripto.
Con la opción -d ó --decrypt desencriptamos el fichero.

``` shell
gpg -d texto.txt.gpg
```

``` shell
gpg -decrypt texto.txt.gpg
```

Y obtendremos el fichero original.

## Cifrado asimétrico

La principal característica del cifrado asimétrico es que tenemos dos claves, 
una pública y otra privada. 

### Generación de claves

Lo primero es generar un par de claves (pública y privada).

Forma abreviada, nos preguntará nuestro nombre, mail y una frase. 
La frase que introducimos no es la clave, lo que hace es proteger nuestra clave.

``` shell
gpg --gen-key
```

Forma completa, nos preguntará el algoritmo a usar, 
la longitud de la clave, el periodo de validez, nuestro nombre, un correo electronico, la descripción del certificado y una frase.
y una frase. 
La frase que introducimos no es la clave, lo que hace es proteger nuestra clave.

``` shell
gpg --full-generate-key
```

### Ver las claves

Para poder ver la clave privada

``` shell
gpg --list-secret-keys
```

Para poder ver la clave pública

``` shell
gpg --list-keys
```

<!-- ### Borrar clave

Se pueden borrar las claves con --delete-secret-key y el identificador de esta.
El identificador los vemos con las opciones --list-secret-keys y --list-keys

``` shell
gpg --delete-secret-key ClaveID
``` -->

### Distribuir la clave pública

Para que nos puedan enviar un mensaje debemos enviar la clave pública.
Para ello la podemos enviar a un fichero con 

``` shell
gpg --armor --output Clavepública.crc --export ClaveID
```

### Registrar la clave pública en la máquina destinataria

Para que nos puedan enviar un fichero cifrado debemos distribuir nuestra clave pública

``` shell
gpg --import Clavepública.crc
```

### Cifrar un fichero

Para encriptar un fichero con nuestra clave pública

``` shell
gpg --encrypt --recipient ClaveID texto.txt
```

### Descifrar un fichero

Para descifrar un fichero con la clave privada

``` shell
gpg --decrypt texto.txt.gpg
```

### Firmar un fichero

Para firma un fichero con la clave privada

``` shell
gpg -u ClaveID --output firmar.txt.gpg --sign firmar.txt 
```

Para verificar un fichero con la clave pública 

``` shell
gpg --verify firma.txt.gpg
```
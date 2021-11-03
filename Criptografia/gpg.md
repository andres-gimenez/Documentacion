# Utilizr gpg para cifrar archivos en Linux

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

Lo primero es generar un par de claves (publica y privada).

Forma abrebiada

``` shell
gpg --gen-key
```

Forma completa

``` shell
gpg --full-generate-key
```
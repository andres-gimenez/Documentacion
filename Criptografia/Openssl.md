# OpenSSL

## Crear un certificado de entidad certificadora (CA).

``` shell
openssl req -x509 -sha256 -nodes -newkey rsa:4096 -keyout mi_ca_privado.key -out mi_ca_publico.crt -days 3650
```

- x509: Certificado autofirmado
- newkey: Generar nueva clave privada y nueva solicitud de certificado rsa:2048: RSA con 2048 bits
- keyout: Clave privada
- days: válidez 10 años
- out: Clave pública

## Comprobar información del certificado

Para ver la información del certificado usuamos

``` shell
openssl x509 -in mi_ca_publico.crt -text -noout
```

## Crear un certificado

Creamos una petición de certificadoo, para enviarlo a una entidad certificadora.

``` shell
openssl req -newkey rsa:2048 -subj "/DC=midominio.com/OU=com/CN=micertificado" -keyout mi_cert_privado.key -out mi_cert_publico_peticion.crt
```

- subj: Establece el asunto del certificado

## Firmar un certificado con nuestro certificado autofirmado

Creamos un fichero (por ejemplo config.txt) con las lineas:

Para indicar que es un certificado final.
- basicConstraints=critical,CA:FALSE

Para indicar la finalidad del certificado.

- extendedKeyUsage = ver_tabla

----- 
- serverAuth             SSL/TLS Web Server Authentication.
- clientAuth             SSL/TLS Web Client Authentication.
- codeSigning            Code signing.
- emailProtection        E-mail Protection (S/MIME).
- timeStamping           Trusted Timestamping
- msCodeInd              Microsoft Individual Code Signing (authenticode)
- msCodeCom              Microsoft Commercial Code Signing (authenticode)
- msCTLSign              Microsoft Trust List Signing
- msSGC                  Microsoft Server Gated Crypto
- msEFS                  Microsoft Encrypted File System
- nsSGC                  Netscape Server Gated Crypto
-----

Firmamos el certificado con 

``` shell
openssl x509 -CA mi_ca_publico.pem -CAkey mi_ca_privado.key -req -in mi_cert_publico_peticion.cet -days 3650 -sha1 -CAcreateserial -out mi_cert_publico_firmador_por_mi_ca.pem -extfile config.txt
```

- x509: Se puede usar entre otras cosas para firmar certificados
- CA: Clave pública del CA
- CAkey: Clave privada del CA
- req: Fichero con la petición
- days: Periodo de validez (10 años)
- sha1: Firma el certificado con -sha1
- CAcreateserial: Añade un número de serie a los certificados firmados con nuestra CA
- out: Certificado de salida (lo que queremos)
- extfile: Fichero con parámetros de configuración

Guardamos nuestro certificado en un fichero PKCS#12

``` shell
openssl pkcs12 -export -in mi_cert_publico_firmador_por_mi_ca.pem -inkey mi_cert_privado.pem -out pkcs_12_cert.pfx
```

Script que hace todo el proceso

``` shell
#!/bin/bash
echo ---------------------
echo CREANDO CLAVE PRIVADA
echo ---------------------
echo introduce el nombre del certificado CA:
read ca_name
echo introduce el password del CA ; read CApass
ca_name_priv=$(echo $ca_name)_priv.pem
ca_name_pub=$(echo $ca_name)_pub.pem
openssl req -x509 -newkey rsa:2048 -passout pass:"$CApass" -keyout $ca_name_priv -out $ca_name_pub -days 365
echo ---------------------
echo CREANDO CLAVE PUBLICA
echo ---------------------
echo introduce el nombre del certificado que vamos a crear:
read cert_name
echo introduce el password del certificado ; read CERTpass
cert_name_priv=$(echo $cert_name)_priv.pem
cert_name_pub=$(echo $cert_name)_pub.pem
cert_name_pet=$(echo $cert_name)_pet.pem
echo introduce el Distinguished Name del certificado \(Ej: /DC=midominio,/DC=com,/CN=certificador\):
read dn
openssl req -newkey rsa:2048 -subj "$dn" -passout pass:"$CERTpass" -keyout $cert_name_priv -out $cert_name_pet
echo -----------------------
echo FIRMANDO EL CERTIFICADO
echo -----------------------
openssl x509 -CA $ca_name_pub -passin pass:"$CApass" -CAkey $ca_name_priv -req -in $cert_name_pet -days 3650 -sha1 -CAcreateserial -out $cert_name_pub
echo ---------------------------
echo CONVIRTIENDO EL CERTIFICADO
echo ---------------------------
openssl pkcs12 -export -in $cert_name_pub -passin pass:"$CERTpass" -inkey $cert_name_priv -out $cert_name.pfx
```

## Crear un certificado digital para entornos de pruebas (localhost)

``` shell
openssl req -new -x509 -days 1825 -subj "/C=ES/ST=Spain/L=/O=/CN=localhost" -key localhost.key -out localhost.crt
```

## Cambio de formatos de cirtifiado

### Convertir un certificado de formota DER (.crt .cer .der) a PEM

``` shell
cat localhost.key localhost.crt > localhost.pem
openssl x509 -in localhost.crt -out localhost.pem
openssl x509 -inform der -in localhost.cer -out localhost.pem
```

### Convertir un certificado en formato PEM a DER

``` shell
openssl x509 -outform der -in localhost.pem -out localhost.der
```

### onvertir un certificado en formato PEM y una clave privada a PKCS#12 (.pfx .p12)

``` shell
openssl pkcs12 -export -out localhost.p12 -inkey localhost.key -in localhost.crt
```

## Examinar un certificado de un servidor Web

``` shell
openssl s_client -showcerts -connect google.com:443
```
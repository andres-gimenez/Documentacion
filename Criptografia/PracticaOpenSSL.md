# Practica criptografia con OpenSSL

OpenSSL es un proyecto de software libre basado en SSLeay, desarrollado por Eric Young y Tim Hudson.
Consiste en un robusto paquete de herramientas de administración y bibliotecas relacionadas con la criptografía, 
que suministran funciones criptográficas a otros paquetes como OpenSSH y navegadores web (para acceso seguro a sitios HTTPS).

## Crear un certificado de entidad certificadora (CA).

Primero debemos crear un certificado de entidad certificadora para poder firmar nuestros certificados.

``` shell
openssl req -x509 -sha256 -nodes -newkey rsa:4096 -keyout mi_ca_privado.key -out mi_ca_publico.crt -days 3650
```

- x509: Certificado autofirmado
- newkey: Generar nueva clave privada y nueva solicitud de certificado rsa:2048: RSA con 2048 bits
- keyout: Clave privada
- days: válidez 10 años
- out: Clave pública

## Listamos la información del certificado

Para ver la información del certificado usamos

``` shell
openssl x509 -in mi_ca_publico.crt -text -noout
```

## Instalar nuestro certificado de entidad certificadora

Para que Windows identifique nuestro certificado como entidad certificadora valida,
debemos instalarla en el almacén de "Entidades de certificación raíz de confianza" del usuario.

## Creamos un certificado para servidores Web

Para crear un certificado para nuestra página Web, primero hay que crear una petición de certificado y luego firmarlo por una entidad certificadora de confianza.

### Creación de petición de certificado

Creamos una petición de certificado digital

``` shell
openssl req -newkey rsa:2048 -subj "/DC=midominio.com/OU=com/CN=micertificado" -keyout mi_cert_privado.key -out mi_cert_publico_peticion.crt
```

- subj: Establece el asunto del certificado

### Firmar el certificado

Para firmar el certificado, lo podemos enviar a una entidad certificadora o lo podemos firmar nosotros con nuestro certificado raiz.

Creamos un fichero (por ejemplo config.txt) con las líneas:

Para indicar que es un certificado final y para autentificador de servidor.

- basicConstraints=critical,CA:FALSE
- extendedKeyUsage=serverAuth

Firmamos el certificado con 

``` shell
openssl x509 -CA mi_ca_publico.crt -CAkey mi_ca_privado.key -req -in mi_cert_publico_peticion.crt -days 3650 -sha1 -CAcreateserial -out mi_cert_publico_firmador_por_mi_ca.crt -extfile config.txt
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


### Exportamos el certificado 

Exportamos nuestro certificado a un fichero PKCS#12

``` shell
openssl pkcs12 -export -in mi_cert_publico_firmador_por_mi_ca.crt -inkey mi_cert_privado.key -out pkcs_12_cert.pfx
```

Para poder usar este certificado en el IIS debemos instalar el certificado en el servidor, en el almacén del equipo, almacén predeterminado.
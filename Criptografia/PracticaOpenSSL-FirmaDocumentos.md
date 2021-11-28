# Practica criptografia con OpenSSL (Firma documentos)

*** Borrador ***

OpenSSL es un proyecto de software libre basado en SSLeay, desarrollado por Eric Young y Tim Hudson.
Consiste en un robusto paquete de herramientas de administración y bibliotecas relacionadas con la criptografía,
que suministran funciones criptográficas a otros paquetes como OpenSSH y navegadores web (para acceso seguro a sitios HTTPS).

## Generar un certificado

``` shell
openssl req -nodes -x509 -sha256 -newkey rsa:4096 -keyout "$(whoami)s Sign Key.key" -out "$(whoami)s Sign Key.crt" -days 365 -subj "/C=NL/ST=(Nombre personal)/L=Ciudad/O=Organización/OU=Organización/CN=$(Nombre pernal)"
```

## Firmar un documento

``` shell
openssl dgst -sha256 -sign "$(whoami)s Sign Key.key" -out sign.txt.sha256 sign.txt
```

## Verificar firma

Para extrare la clave publica del certificado usamos.

``` shell
openssl x509 -in "$(whoami)s Sign Key.crt"
```

``` shell
openssl dgst -sha256 -verify  <(openssl x509 -in "$(whoami)s Sign Key.crt"  -pubkey -noout) -signature sign.txt.sha256 sign.txt
```


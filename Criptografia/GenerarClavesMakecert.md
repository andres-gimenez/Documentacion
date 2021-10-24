# Crear claves publica y privada con Macecert

## Generar certificado reiz 

Primero es crear un certificado, raiz como entidad certificadora. Este certeficado contien las claves publica y privada.

``` cmd
rem Generar certificado raiz.
makecert -n "CN=MiEntidad CA Root,O=MiEntidad,C=ES" -pe -m 360 -a sha1 -len 2048 -cy authority -r -sv MiEntidad_CA_Root.pvk MiEntidad_CA_Root.cer
```

Para Registra el certificado raíz en windows como certificado de confianza.

``` cmd
rem Registrar certificado raiz.
certutil -f -addstore root MiEntidad_CA_Root.cer
```

Crear un copia de seguridad de certificado raíz.

``` cmd
rem Crear copia de seguridad del certificado Raíz.
rem certutil -privatekey -exportpfx MiEntidad CA Root" MiEntidad_CA_Root.pfx
```

Crar un fichero pfx del certificado raíz.

``` cmd
rem Crear pfx
pvk2pfx -pvk MiEntidad_CA_Root.pvk -spc MiEntidad_CA_Root.cer -pfx MiEntidad_CA_Root.pfx 
```

## Instalar certificado raiz en clientes

Para que un certificado raíz se reconozca como de confianza hay que instalarlo en cada cliente con 

``` cmd
certutil -f -addstore root Nivaes_CA_Root.cer
```

## Generar un certificado de servidor Web

Crear un certificado para instalar en el servidor Web y la reconozca como pargina de confianza.

``` cmd
makecert -n "CN=mydominio.com,O=MiEntidad Systems,C=ES" -pe -sky exchange -m 24 -iv MiEntidad_CA_Root.pvk  -ic MiEntidad_CA_Root.cer -a sha1 -len 2048 -eku 1.3.6.1.5.5.7.3.1 -sv sslluntia.pvk sslluntia.cer
```

Crear copia de seguridad para certificado.
``` cmd
rem certutil -privatekey -exportpfx "MiEntidad CA Root" SSL_MiEntidad.pfx
pvk2pfx -pvk sslMiEntidad.pvk -spc sslMiEntidad.cer -pfx sslMiEntidad.pfx
```

## Certificados para despliegue de software 

Para realizar despliegue de software hay que firmar los ensamblados y los manifiestos con los contenidos de las aplicaciones.

### Generar un certificado de firma despliegue

Para firmar un despligue con ClickOne, se ha de generar firmas para el despliegue.

Crear un certificado para firma de despliegue

``` cmd
Rem 
makecert -n "CN=MiEntidad Systems, O=MiEntidad Systems, C=ES" -pe -sky signature -m 60 -iv MiEntidad_CA_Root.pvk  -ic MiEntidad_CA_Root.cer -a sha1 -len 2048 -eku 1.3.6.1.5.5.7.3.3 -sv DeploySign.pvk DeploySign.cer
rem makecert -n "CN=Developer MiEntidad Systems, O=MiEntidad Systems, C=ES" -pe -sky signature -m 60 -iv MiEntidad_CA_Root.pvk  -ic MiEntidad_CA_Root.cer -a sha1 -len 2048 -eku 1.3.6.1.5.5.7.3.3 -sv DeveloperDeploySign.pvk DeveloperDeploySign.cer
```

Crear copia de seguridad para certificado.

``` cmd
pvk2pfx -pvk DeploySign.pvk -spc DeploySign.cer -pfx DeploySign.pfx
rem pvk2pfx -pvk DeveloperDeploySign.pvk -spc DeveloperDeploySign.cer -pfx DeveloperDeploySign.pfx
```

### Crear certificado de firma de ensamblados 

Crear un certificado para firma de ensamblados

``` cmd
makecert -n "CN=MiEntidad Systems, O=MiEntidad Systems, C=ES" -pe -sky signature -m 60 -iv MiEntidad_CA_Root.pvk  -ic MiEntidad_CA_Root.cer -a sha1 -len 2048 -eku 1.3.6.1.5.5.7.3.3 -sv CodeSign.pvk CodeSign.cer
Rem makecert -n "CN=Developer MiEntidad Systems, O=MiEntidad Systems, C=ES" -pe -sky signature -m 60 -iv MiEntidad_CA_Root.pvk  -ic MiEntidad_CA_Root.cer -a sha1 -len 2048 -eku 1.3.6.1.5.5.7.3.3 -sv DeveloperCodeSign.pvk DeveloperCodeSign.cer
```

Crear copia de seguridad para certificado.

``` cmd
pvk2pfx -pvk CodeSign.pvk -spc CodeSign.cer -pfx CodeSign.pfx
Rem pvk2pfx -pvk DeveloperCodeSign.pvk -spc DeveloperCodeSign.cer -pfx DeveloperCodeSign.pfx
```

Instala el par de claves para que VS no se queje.

``` cmd
rem sn -i CodeSign.pfx VS_KEY_45975F5B777C9870
```

### Crear certificado de intercambio de información

Crear un certificado de servidor

``` cmd
rem makecert -n "CN=CenterServer, O=MiEntidad Systems, C=ES" -pe -sky exchange -m 60 -iv MiEntidad_CA_Root.pvk  -ic MiEntidad_CA_Root.cer -a sha1 -len 2048 -eku 1.3.6.1.5.5.7.3.1 -sv Exchange\CenterServer.pvk Exchange\CenterServer.cer
rem makecert -n "CN=CenterServer, O=MiEntidad Systems, C=ES" -pe -sky exchange -m 60 -iv MiEntidad_CA_Root.pvk  -ic MiEntidad_CA_Root.cer -a sha1 -len 2048 -eku 1.3.6.1.5.5.7.3.1 -sv Exchange\DeveloperCenterServer.pvk Exchange\DeveloperCenterServer.cer
makecert -n "CN=CenterServer, O=MiEntidad Systems, C=ES" -pe -sky exchange -m 60 -cy end -a sha1 -len 2048 -eku 1.3.6.1.5.5.7.3.1 -sv Exchange\CenterServer.pvk Exchange\CenterServer.cer
rem makecert -n "CN=CenterServer, O=MiEntidad Systems, C=ES" -pe -sky exchange -m 60 -cy end -a sha1 -len 2048 -eku 1.3.6.1.5.5.7.3.1 -sv Exchange\DeveloperCenterServer.pvk Exchange\DeveloperCenterServer.cer
```

Crear un certificado de cliente

``` cmd
rem makecert -n "CN=CenterClient, O=MiEntidad Systems, C=ES" -pe -sky exchange -m 60 -iv MiEntidad_CA_Root.pvk  -ic MiEntidad_CA_Root.cer -a sha1 -len 2048 -eku 1.3.6.1.5.5.7.3.3 -sv Exchange\CenterClient.pvk Exchange\CenterClient.cer
rem makecert -n "CN=CenterClient, O=MiEntidad Systems, C=ES" -pe -sky exchange -m 60 -iv MiEntidad_CA_Root.pvk  -ic MiEntidad_CA_Root.cer -a sha1 -len 2048 -eku 1.3.6.1.5.5.7.3.3 -sv Exchange\DeveloperCenterClient.pvk Exchange\DeveloperCenterClient.cer
makecert -n "CN=CenterClient, O=MiEntidad Systems, C=ES" -pe -sky exchange -m 60 -cy end -a sha1 -len 2048 -eku 1.3.6.1.5.5.7.3.3 -sv Exchange\CenterClient.pvk Exchange\CenterClient.cer
rem makecert -n "CN=CenterClient, O=MiEntidad Systems, C=ES" -pe -sky exchange -m 60 -cy end -a sha1 -len 2048 -eku 1.3.6.1.5.5.7.3.3 -sv Exchange\DeveloperCenterClient.pvk Exchange\DeveloperCenterClient.cer
```

Crear copia de seguridad para certificado.

``` cmd
pvk2pfx -pvk Exchange\CenterServer.pvk -spc Exchange\CenterServer.cer -pfx Exchange\CenterServer.pfx -po Petar2
pvk2pfx -pvk Exchange\CenterClient.pvk -spc Exchange\CenterClient.cer -pfx Exchange\CenterClient.pfx -po Petar2
rem pvk2pfx -pvk Exchange\DeveloperCenterServer.pvk -spc Exchange\DeveloperCenterServer.cer -pfx Exchange\DeveloperCenterServer.pfx -po Petar2
rem pvk2pfx -pvk Exchange\DeveloperCenterClient.pvk -spc Exchange\DeveloperCenterClient.cer -pfx Exchange\DeveloperCenterClient.pfx -po Petar2
```

``` ps
New-SelfSignedCertificateEx -Subject "CN=CenterServer, O=MiEntidad Systems, C=ES" -KeySpec Exchange -KeyUsage "DataEncipherment, KeyEncipherment, DigitalSignature" -Path Exchange\CenterServer.pfx -Exportable -SAN "CenterServer" -SignatureAlgorithm sha1 -AllowSMIME -Password (ConvertTo-SecureString "Petar2" -AsPlainText -Force) -NotAfter (get-date).AddYears(20)
New-SelfSignedCertificateEx -Subject "CN=CenterClient, O=MiEntidad Systems, C=ES" -KeySpec Exchange -KeyUsage "DataEncipherment, KeyEncipherment, DigitalSignature" -Path Exchange\CenterClient.pfx -Exportable -SAN "CenterClient" -SignatureAlgorithm sha1 -AllowSMIME -Password (ConvertTo-SecureString "Petar2" -AsPlainText -Force) -NotAfter (get-date).AddYears(20)

```

## Convertir certificado Pfx a Key

Para convertir formato pfx a key

Con *openssl*

``` cmd
openssl pkcs12 -in sslMiEntidad.pfx -nocerts -out sslMiEntidad.key
```

``` cmd
openssl pkcs12 -in sslMiEntidad.pfx -clcerts -nokeys -out sslMiEntidad.crt
```

``` cmd
openssl pkcs12 -in MiEntidad_CA_Root.pfx -clcerts -nokeys -out MiEntidad_CA_Root.crt
```

#Ejecutar esto, editar el fichero y seleccionar las partes.

``` cmd
openssl pkcs12 -in sslMiEntidad.pfx  -out sslMiEntidad.key -nodes
```
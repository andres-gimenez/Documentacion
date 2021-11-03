# Crear certificados

## Crear certificados autofirmados

[New-SelfSignedCertificate](https://docs.microsoft.com/en-us/powershell/module/pki/new-selfsignedcertificate)

En este ejemplo se crea un certificado S/MIME autofirmado en el almacén MY del usuario. 
El certificado utiliza el proveedor predeterminado, que es el proveedor de almacenamiento de claves de software de Microsoft.
El certificado utiliza una clave asimétrica RSA con un tamaño de clave de 2048 bits.
Este certificado tiene los nombres alternativos de sujeto de patti.fuller@contoso.com como RFC822 y pattifuller@contoso.com como nombre principal.‎

``` powershell
New-SelfSignedCertificate -Type Custom -Subject "E=patti.fuller@contoso.com,CN=Patti Fuller" -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.4","2.5.29.17={text}email=patti.fuller@contoso.com&upn=pattifuller@contoso.com") -KeyAlgorithm RSA -KeyLength 2048 -SmimeCapabilities -CertStoreLocation "Cert:\CurrentUser\My"
```

En este ejemplo se crea un certificado de autenticación de cliente autofirmado en el almacén MY del usuario. El certificado utiliza el proveedor predeterminado, que es el proveedor de almacenamiento de claves de software de Microsoft. El certificado utiliza una clave asimétrica de curva elíptica y los parámetros de curva nist256, que crea una clave de 256 bits. El nombre alternativo del sujeto es pattifuller@contoso.com.‎

``` powershell
New-SelfSignedCertificate -Type Custom -Subject "CN=Patti Fuller,OU=UserAccounts,DC=corp,DC=contoso,DC=com" -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2","2.5.29.17={text}upn=pattifuller@contoso.com") -KeyUsage DigitalSignature -KeyAlgorithm ECDSA_nistP256 -CurveExport CurveName -CertStoreLocation "Cert:\CurrentUser\My"
```

## Crear certificado autofirmado para SSL y localhost

``` powershell
New-SelfSignedCertificate -Subject "localhost" -TextExtension @("2.5.29.17={text}DNS=localhost&IPAddress=127.0.0.1&IPAddress=::1")
```

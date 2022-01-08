# Ejemplos de docker

## Aplicación de consola

### Aplicación de consola en dotnet

``` ps
docker run --rm mcr.microsoft.com/dotnet/samples
```

## Ejemplo Web

### Aplicación web dotnet

``` ps
docker run -it --rm -p 8000:80 --name aspnetcore_sample mcr.microsoft.com/dotnet/samples:aspnetapp
```

### Applicación con certificado

Generamos y instalamos un certificado.

``` ps
dotnet dev-certs https -ep C:\Apl\Certificados\aspnetapp.pfx -p crypticpassword
dotnet dev-certs https --trust
```

Descargamos la imagen y la ejecutamos ocn el certificado creado.

``` ps
docker pull mcr.microsoft.com/dotnet/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="crypticpassword" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx -v C:\Apl\Certificados:/https/ mcr.microsoft.com/dotnet/samples:aspnetapp
```

## Entrar en máquina docker

``` ps
docker exec -it <container> bash 
```

``` ps
docker exec -it <container> /bin/bash 
```

``` ps
docker exec -it <container> ls 
```

``` ps
docker run -it --entrypoint /bin/bash [docker_image]


Ejecutar un comando en un docker parado.

``` ps
docker commit <container> temp
docker run -it --rm temp <your commands>
docker rmi temp
```

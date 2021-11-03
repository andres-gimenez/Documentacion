# SQL Server en Docker

## Instalar SQL Server en Docker

[Podemos ver la instalación desde la Web de Microsoft](https://docs.microsoft.com/es-es/sql/linux/quickstart-install-connect-docker)


Para instalar SQL Server en docker basta con ejecutar

``` shell
$ sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" \
   -p 1433:1433 --name sql1 -h sql1 \
   -d mcr.microsoft.com/mssql/server:2019-latest
```

Para conectarte a SQL desde la máquina host

``` shell
$ sudo docker exec -it sql1 "bash"
```

Para conectarte al SQL Server

``` shell
$ /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "<YourNewStrong@Passw0rd>"
```

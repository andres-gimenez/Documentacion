# Sql Server

[Base de datos de ejemplo](https://github.com/microsoft/sql-server-samples/)

Abris Firewall Windows para Sql Server

``` ps
netsh firewall set portopening protocol = TCP port = 1433 name = SQLPort mode = ENABLE scope = SUBNET profile = CURRENT
```

Probar que hay conexión con

``` ps
telnet [direcionServidor] 1433
```

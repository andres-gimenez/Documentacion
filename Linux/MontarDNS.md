# Montar Servidor DNS en Ubuntu Server

## Comandos clientes

### Windows

#### Liberal cache DNS en Windows

```Command
ipconfig /flushdns
```

### Linux

#### Liberal cache DNS en Linux

```Command
sudo systemd-resolve --flush-cachres
```

### MacOS

#### Liberal cache DNS en MacOS

```Command
sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder
```

```Command
sudo killall -HUP mDNSResponder;sudo killall mDNSResponderHelper;sudo dscacheutil -flushcache;say MacOS DNS cache has been cleared
```

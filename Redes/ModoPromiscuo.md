# Mode promiscuo

## Modo promiscuo Windows

Para habilitar el modo promiscuo de Windows

Ver información de los adaptadores

``` ps
netsh bridge show adapter
```

Modificar el adaptador, para abilitar modo promiscuo

``` ps
netsh bridge set adapter 1 forcecompatmode=enable
```

Modificar el adaptador, para desabilitar modo promiscuo

``` ps
netsh bridge set adapter 1 forcecompatmode=disable
```

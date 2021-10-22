# Configurar un swich capa 3

## Crear las vlan

```
Switch#vlan database
Switch(vlan)#vlan 10 name vlan10
Switch(vlan)#vlan 20 name vlan20
Switch(vlan)#exit
```

```
Switch>
Switch>en
Switch#
Switch#vlan database
% Warning: It is recommended to configure VLAN from config mode,
  as VLAN database mode is being deprecated. Please consult user
  documentation for configuring VTP/VLAN in config mode.

Switch(vlan)#vlan 10 name vlan10
VLAN 10 added:
    Name: vlan10
Switch(vlan)#vlan 20 name vlan20
VLAN 20 added:
    Name: vlan20
Switch(vlan)#
Switch(vlan)#exit
APPLY completed.
Exiting....
Switch#vlan database
% Warning: It is recommended to configure VLAN from config mode,
  as VLAN database mode is being deprecated. Please consult user
  documentation for configuring VTP/VLAN in config mode.

Switch(vlan)#
Switch#
```
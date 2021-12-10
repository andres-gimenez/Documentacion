# Comandos Cisco show

## Show Interfaces

Despliega las estadísticas completas de todas las interfaces del router:

``` cisco
Router# show interfaces
```

## Show interface serial

Despliega la información de esta interfaz serial:

``` cisco
Router# show interface serial 0/0/0
```

## show ip interface brief

Despliega un resúmen de todas las interfaces incluyendo el estado y la dirección IP.

``` cisco
Router# show ip interface brief
```

## Show controllers nombre_interfaz

Despliega las estadísticas del hardware de una interfaz (información de si el cable es DTE o DCE):

``` cisco
Router# show controllers nombre_interfaz
```

## Show clock

Despliega la hora del router:

``` cisco
Router# show clock
```

## Show history

Despliega el historial de comandos utilizados:

``` cisco
Router# show history
```

## Show flash

Despliega información acerca de la memoria flash y los archivos IOS que se encuentran almacenados en ella:

``` cisco
Router# show flash
```

## Show version

Despliega la información acerca del router y de la imágen de IOS que esté corriendo en la memoria RAM, también muestra el valor del registro de configuración del router:

``` cisco
Router# show versión
```

## Show arp

Despliega la tabla ARP del router:

``` cisco
Router# show arp
```

## Show protocols

Despliega los protocolos de capa 3 configurados:

``` cisco
Router# show protocols
```

## Show startup-config

Despliega la configuración grabada en la NVRAM:

``` cisco
Router# show startup-config
```

## Show running-config o show run

Despliega la configuración que se encuentra actualmente corriendo en la memoria RAM:

``` cisco
Router# show running-config
```

``` cisco
Router# show run
```

## Show hosts

Despliega la lista en caché de los nombres de host y sus direcciones:

``` cisco
Router# show hosts
```

## Show users

Despliega todos los usuarios conectados al router:

``` cisco
Router# show users
```

## Show ip route

Despliega la tabla de enrutamiento del router. La tabla de enrutamiento es la lista de todas las redes que el dispositivo puede alcanzar, su métrica, y la forma en que accede a ellas.

``` cisco
Router# show ip route
```

## Show ip route summary

Despliega el resúmen de la tabla de enrutamiento del router:

``` cisco
Router# show ip route summary
```

## Show ip traffic

Despliega las estadísticas del tráfico IP en el router:

``` cisco
Router# show ip traffic
```

## Show access-list

Despliega las listas de acceso configuradas y el número de hits que cada línea ha recibido, de este modo podemos hacer un mejor debug de cualquier problema con las listas de acceso:

``` cisco
Router# show access-list
```

## Show cdp neighbors

Despliega un reporte de todos los dispositivos Cisco al que estamos conectados:

``` cisco
Router# show cdp neighbors
```

## Show cdp neightbors detail

Despliega un reporte detallado de todos los dispositivos Cisco al que estamos conectados:

``` cisco
Router# show cdp neighbors detail
```

## Show inventory

Despliega el inventario de tarjetas:

``` cisco
Router# show inventory
```

## Show processes

Despliega los procesos activos:

``` cisco
Router# show processes
```

## Show sessions

Despliega las conexiones Telnet establecidas en el router:

``` cisco
Router# show sessions
```

## Show memory

Despliega las estadísticas de memoria del router:

``` cisco
Router# show memory
```

## Show tech-support

Despliega la información completa del sistema:

``` cisco
Router# show tech-support
```

## Show ip rip database

Despliega la información del protocolo de rute RIP:

``` cisco
Router# show ip rip database
```

## Show ip eigrp topology

Despliega la rutas aprendidas con el protocolo eigrp:

``` cisco
Router# show ip eigrp topology
```

## Show vlan

Despliega la lista de vlans existentes en el switch:

``` cisco
Switch# show vlan
```

## Show vlan-membership

Despliega la lista de vlans y las interfaces asignadas a cada vlan:

``` cisco
Switch# show vlan-membership
```

## Show mac-address-table

Despliega la información de la tabla de direcciones mac:

``` cisco
Switch# show mac-address-table
```

## show spanning-tree

Despliega la información del protocolo spannig-tree:

``` cisco
Switch# show spanning-tree
```

## Show boot

Muestra el archivo de boot:

``` cisco
Switch# show boot
```

## Show controllers serial

Muestra información para el intervalo ebtre publicaciones CDP además del tiempo que es válido y la versión de esas publicaciones.

``` cisco
show cdp
```

## Show editing

Para controlar la pila de los diferentes procesos o momentos de interrupción arrojando los motivos del rearranque del sistema.

``` cisco
show stacks
```

## Show cdp interfaces

Arroja información de las interfaces donde CDP está activado por lo que este comando comando se utiliza para verificar el estado de las interfaces del switch

``` cisco
show cdp interfaces (tipo número)
```

## Show arp ethernet

Muestra las entradas en la tabla de enrutamiento.

``` cisco
show arp ethernet
```

## Show authentication

Muestra cómo se hace la autenticación de los inicios de sesión:

``` cisco
show authentication
```

## Show backplane

Arroja detalles de la memoria SEEPROM:

``` cisco
show backplane
```

## Show card

Muestra la configuración, estado y detalles de memoria SEEPROM

``` cisco
show card {card-selection | all}
```

## show diagnostic fan

Muestra pruebas de diagnóstico para los ventiladores.

``` cisco
show diagnostic fan {all | fan-selection}
```

## Show ntp

Muestra hora y fecha actual del cambio del servidor y los NTP que utiliza el conmutador del servidor para la configuración del reloj del sistema.

``` cisco
show ntp
```
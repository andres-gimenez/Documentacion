# Tracert

Tracert nos permite ver la ruta para llegar a un host remoto.

``` ps

PS C:\> tracert google.es

Traza a la dirección google.es [142.250.184.163]
sobre un máximo de 30 saltos:

  1     4 ms     1 ms     1 ms  10.100.56.1
  2     7 ms     7 ms     7 ms  100.65.102.129
  3     9 ms     7 ms     8 ms  100.65.43.134
  4     9 ms     7 ms     8 ms  172.23.144.4
  5     8 ms     8 ms     8 ms  172.23.144.97
  6    10 ms     9 ms     8 ms  41.red-193-152-57.static.ccgg.telefonica.net [193.152.57.41]
  7    12 ms    24 ms    16 ms  229.red-80-58-86.staticip.rima-tde.net [80.58.86.229]
  8    12 ms    10 ms    11 ms  29.red-81-41-205.staticip.rima-tde.net [81.41.205.29]
  9    18 ms    10 ms     9 ms  17.red-81-46-0.customer.static.ccgg.telefonica.net [81.46.0.17]
 10     *        *        *     Tiempo de espera agotado para esta solicitud.
 11    22 ms    14 ms    14 ms  176.52.253.102
 12    14 ms    11 ms    10 ms  172.253.50.55
 13    10 ms     8 ms     9 ms  142.250.213.127
 14    10 ms     9 ms     9 ms  mad07s23-in-f3.1e100.net [142.250.184.163]

 ```

Tracert desde el movil con  PingTools

 ``` txt
 traceroute to google.es (142.250.200.99), 30 hops max
Hop 1:
    * 

Hop 2:
    From 10.7.48.13, 89 ms

Hop 3:
    From 10.7.32.120, 78 ms

Hop 4:
    From 10.7.32.133, 88 ms

Hop 5:
    From 10.14.3.14, 87 ms

Hop 6:
    From 209.85.168.54, 97 ms

Hop 7:
    * 

Hop 8:
    From 142.251.51.140, 96 ms

Hop 9:
    From 74.125.242.179, 96 ms

Hop 10:
    From mad41s13-in-f3.1e100.net (142.250.200.99), 94 ms

Traceroute complete: 10 hops, time: 9979 ms
```

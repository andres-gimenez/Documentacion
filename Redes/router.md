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
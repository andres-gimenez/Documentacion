# Ejemplo de Backup ficheros en Linux

## Copia de seguridad de una máquina Linux

Creamos el siguien *script* con el nombre **backup-script** el cual empaquetará todo el contenido de las carpetas de configuración del sistema y en función del dia del mes en el que se produce el *bakcup* se renombrá el fichero resutante.

``` bash
#!/bin/bash
# script de copia completa e incremental
# modificar directorios a respaldar y destino del Backup
DIRECTORIOS="/bin /boot /etc /initrd /home /lib /opt /root /sbin /srv /usr /var"
# Directorio donde se guarda el backup
BACKUPDIR=/home/Backups
# Directorio que guarda la fecha del último backup completo
FECHADIR=/home/Backups 
DSEM=`date +%a` # Día de la semana (por ej. mié)
DMES=`date +%d` # Día del mes (por ej. 06)
DYM=`date +%d%b` # Día y mes (por ej. 06jun)
# "NUEVO" coge la fecha del backup completo de cada domingo 
# Backup mensual completo - sobrescribe el del mes anterior
if [ $DMES = "01" ]; then
tar -cf $BACKUPDIR/CTM_$DYM.tar $DIRECTORIOS
fi

# Backup semanal completo
# Actualiza fecha del backup completo
if [ $DSEM = "dom" ]; then
AHORA=`date +%d-%b`
echo $AHORA > $FECHADIR/fecha-backup-completo
tar -cf $BACKUPDIR/CTS_$DSEM.tar $DIRECTORIOS

# Backup incremental diario - sobrescribe semana anterior
# Obtiene fecha del último backup completo, opcion newer.
else
NUEVO="--newer=`cat $FECHADIR/fecha-backup-completo`"
tar $NUEVO -cf $BACKUPDIR/ID_$DSEM.tar $DIRECTORIOS
fi
```

Para determinar la fecha del ultimo *backup* completo se utiliza un fichero de texto con la fecha del último backup complejo. Para crear el fichero se puede utlizar el comando

``` bash
echo `date +%d-%b` > /home/Backups/fecha-backup-completo
```

Cuando terminamos el *script* retocamos a nuestro gusto le damos permisos de ejecución y lo copiamos a la carpeta */usr/bin/, por ejemplo

``` bash
chmod u+x backup-script
cp backup-script /usr/bin/
```

Después bastará con hacer que el *script* se ejecute cada día mediant cron, por ejemplo a las 3 de la mañana,
para que no nos moleste mientras trabajamos con el PC.

``` bash
crontab -e
escribiremos en la tabla:
0 3 * * * /usr/bin/backup-script
```

### Propuesta de mejora

Para reducir el espacio de los backup es combeniente el comprimir los ficheros donde se almacena la información.
Utilizar el comando **gzip** para comprimir el resultado del empaquetamiento con **tar**.

#### Utilización de gzip

Para comprimir un solo archivo, invoque el comando seguido del nombre de archivo

``` bash
gzip filename
```

‎Si desea conservar el archivo de entrada (original), utilice la opción ‎-k

``` bash
gzip -k filename
```

Otra opción para mantener el archivo original es usar la opción que le dice que escriba en la salida estándar y redirija la salida a un archivo

``` bash
gzip -c filename > filename.gz
```

Para aumentar el ratio de compresión se puede usar la opción -9

``` bash
gzip -9 filename
```

### Almacenamiento en lugar remoto

Para guardar los *backup* en lugar seguro es combeniente el guardar los en una máquina, preferiblemente setuada en una localización distnte,
para ello una opción sencilla es enviar el fichero a un servidor *ftp*.
Modificar el *script* para enviar el fichero a un servidor de **ftp**.

#### Para realizar un ftp desde bash

Para realizar un ftp desde un script bash, podemos insertar un fracmento de código similar a este.

``` bash
#!/bin/sh
HOST='ftp.example.com'
USER='yourid'
PASSWD='yourpw'
FILE='file.txt'

ftp -n $HOST <<END_SCRIPT
quote USER $USER
quote PASS $PASSWD
binary
put $FILE
quit
END_SCRIPT
exit 0
```

<!-- #### Para realizar un ftp por comando

Para realizar el ftp por linea de comando se puede utilizar el comando

``` console
ftp -in -u ftp://username:password@servername/path/to/ localfile
``` -->

## Restaurar el backup

Para restaurar el backup deveremos recuperar el fichero, loguearnos como *root* y, situados en el directorio raíz o punto donde queramos, restaurar el *backup* con el comando:

``` console
cd /
tar -xf ruta_del_backup_que_queremos_restaurar.tar
```

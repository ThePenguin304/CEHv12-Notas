# Técnicas de enumeración NTP y NFS

Redirección a [Información General](https://github.com/ThePenguin304/CEHv12-Notas/blob/main/Modulos/Modulo%204/%230%20Info%20general.md)

`NTP (Network Time Protocol)` Se utiliza para sincronizar la hora de los sistemas en una red. La enumeración de NTP puede proporcionar información útil para los atacantes, como la versión del servicio y posibles configuraciones vulnerables.

`NFS (Network File System)` Es un protocolo de archivos distribuido que permite a los clientes acceder a archivos en sistemas remotos a través de la red. Los atacantes pueden enumerar los sistemas NFS para obtener información sobre qué directorios están exportados y qué permisos tienen.

## NTP

UDP Puerto 123

Información que el atacante desea extraer:
- Lista de hosts conectados
- Direcciones IP de los clientes en una red, sus nombres de sistema y sistemas operativos
- Las direcciones IP internas también se pueden obtener si el servidor NTP está en la zona desmilitarizada (DMZ)

### Comandos

`ntpdate` Se utiliza para sincronizar la hora del sistema con un servidor NTP específico. A diferencia de ntpd, que mantiene la sincronización continua en segundo plano, ntpdate realiza una sincronización puntual (solo una vez en el momento de ejecución). Esto es útil cuando necesitas ajustar rápidamente la hora de una máquina o verificar si está sincronizada correctamente con el servidor NTP.

```bash
ntpdate [-46bBdqsuv] [-a key] [-e authdelay] [-k keyfile] [-o version] [-p samples] [-t timeout] [ -U user_name] server [...]
```

`ntptrace` Se utiliza para rastrear la cadena de servidores NTP en la jerarquía de tiempo. Ayuda a identificar qué servidores NTP están siendo utilizados por el servidor objetivo. Este comando sigue la cadena de servidores desde el servidor NTP primario hasta el servidor más cercano a él.

```bash
ntptrace [-n (Not print hostnames)] [-m maxhosts] [servername/IP_address]

ntptrace 192.168.1.100
target host: 192.168.1.100
  192.168.1.100: stratum 2, offset -0.001234, delay 0.03345
  192.168.1.1: stratum 1, offset -0.000125, delay 0.00345
  127.127.1.1: stratum 0, offset 0.000000, delay 0.00000
```

`ntpdc` Se usa para interactuar con un servidor NTP en modo "control", proporcionando información más detallada sobre el estado del servidor NTP. Este comando puede ser útil para obtener estadísticas y configuraciones internas del servidor NTP.

```bash
ntpdc -c sysinfo 192.168.1.100

stratum 2
leap 00
precision -18
root delay 0.040000
root dispersion 0.014000
reference ID 0x0a000001
```

`ntpq` Se utiliza para consultar el estado del servidor NTP y obtener información sobre su configuración y sincronización. Con ntpq, los usuarios pueden ver el estado de los servidores NTP a los que el servidor está sincronizado, su precisión y otros detalles importantes.

```bash
ntpq -p 192.168.1.100

     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
*192.168.1.1     127.127.1.0   10 u   33   64  377    0.012    0.003   0.001
+192.168.1.2     127.127.1.0   10 u   38   64  377    0.018    0.002   0.002
```

### Herramientas de enumeración NTP

`PRTG Network Monitor` incluye SNTP Sensor monitor, un servidor SNTP que compara la diferencia de tiempo de respuesta entre el servidor y el sistema local.

## NFS

Protocolo que permite a los sistemas conectados en una red compartir archivos y directorios de manera remota, como si estuvieran en el propio sistema local. Esto facilita la gestión y el acceso compartido a archivos y permite a los usuarios trabajar con archivos remotos sin necesidad de transferirlos manualmente.

La enumeración NFS permite a los atacantes identificar los directorios exportados, la lista de clientes conectados al servidor NFS junto con sus direcciones IP y los datos compartidos asociados a las direcciones IP.

La llamada a procedimiento remoto (RPC) se utiliza para enrutar y procesar la solicitud entre clientes y servidores.

/etc/exports contiene la lista de usuarios que se les permite exportar ficheros.

Después de recopilar esta información, los atacantes pueden falsificar sus direcciones IP para obtener acceso completo a los archivos compartidos en el servidor. Para acceder al servidor, la única credencial utilizada es la dirección IP del cliente.

`rpcinfo -p <TargetIP Addres>`

>Escaneo al "target" usando el comando `rpcinfo` al puerto 2049 y los servicios NFS corriendo en este.

Para ver la lista de ficheros y directorios compartidos se usa el siguiente comando.

`showmount -e <Target IP Addres>`

### Herramientas enumeración NFS

Los atacantes pueden usar otras herramientas para ganar acceso al servidor NFS y subir ficheros malignos para realizar ataques a futuro.

Es importante enumerar la lista de servicios RPC y la lista de los compartidos de NFS:
* `RPCScan` Revisa malas configuraciones en los servidores NFS.

`python3 rpc-scan.py <Target IP Address> --rpc`

* `SuperEnum` Contiene un script que enumera cualquier puerto abierto.

Un atacante utiliza el script `./superenum` y luego ingresa un archivo de texto llamado `“Target.txt”` que tiene una dirección IP de destino o una lista de direcciones IP para enumerar.

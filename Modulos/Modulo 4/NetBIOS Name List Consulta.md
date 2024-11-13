# NetBIOS Name List

|Name|NetBIOS Code|Type|InformationObtained|
|---------|-----|-------|-------|
|`<host name>`/`<domain>`|`<00>`| UNIQUE |Hostname o Dominio
|`<host name>`|`<03>`| UNIQUE |Servicio ejecutandose en el sistema
|`<host name>`|`<20>`| UNIQUE |Servicio ejecutandose en el servidor
|`<domain>`|`<1D>`| GROUP |Buscador master para la subnet
|`<domain>`|`<1B>`| UNIQUE |Buscador master para la subnet (PDC)
|`<domain>`|`<1E>`| GROUP |Elección del servicio de busqueda

```bash
> nbtstat -A 192.168.1.10 -r -c -s
```

>-A 192.168.1.10: Consulta la tabla de nombres de NetBIOS en el equipo con dirección IP 192.168.1.10.

>-r: Muestra las estadísticas de resolución de nombres NetBIOS.

>-c: Muestra la tabla de nombres NetBIOS en caché, que contiene los nombres ya resueltos.

>-s: Muestra las sesiones de NetBIOS activas y el estado de cada conexión.

### Resultado:

NetBIOS Remote Machine Name Table:

|Name|NetBIOS Code|Type|Status|
|---------|-----|-------|-------|
|WORKGROUP|`<00>`| GROUP |Registered|
|PC-ALICE|`<03>`| UNIQUE |Registered|
|ADMIN|`<03>`| UNIQUE |Registered|

> Muestra los nombres NetBIOS registrados en el equipo 192.168.1.10.
> WORKGROUP es el grupo de trabajo al que pertenece.
> Varios nombres (como PC-ALICE, ADMIN) están registrados con diferentes tipos, representando diferentes servicios o usuarios.

NetBIOS Names Resolution and Registration Statistics

|Name|NetBIOS Code|
|---------|-----|
|`Successful Name Resolution`|`15`|
|`Successful Name Releases`|`5`|
|`Failed Name Resolutions`|`2`|

> Resumen de estadísticas de resolución de nombres.
> Incluye nombres resueltos con éxito, liberaciones de nombres, y fallos en las resoluciones.

NetBIOS Remote Cache Name Table

|Name|NetBIOS Code|Type|IP Address|
|---------|-----|-------|-------|
|PC-JOHN|`<20>`| UNIQUE |192.168.1.11|
|ADMIN|`<20>`| UNIQUE |192.168.1.12|

> Esta tabla de caché muestra los nombres NetBIOS de otros sistemas a los que el equipo ha accedido recientemente junto con sus direcciones IP.
> Nos da pistas sobre otros dispositivos en la red, como PC-JOHN, ADMIN.

NetBIOS Session Statistics

|Remote Machine|In/Out|TotalBytes|Status|
|---------|-----|-------|-------|
|192.168.1.11|`Outbound`| 15234 |Active|
|192.168.1.12|`Inbound`| 10294 |Idle|

> Aquí se muestran las conexiones NetBIOS activas y sus estados.
> Por ejemplo, hay conexiones de entrada y salida entre 192.168.1.10 y otros dispositivos (192.168.1.11, 192.168.1.12), indicando sesiones activas y ociosas.

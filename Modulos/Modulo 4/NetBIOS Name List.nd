# NetBIOS Name List

**` Name | NetBIOS Code | Type | Information Obtained `**

` <host name> | <00> | UNIQUE | Hostname `

`<domain> | <00> | GROUP | Domain name`

`<host name> | <03> | UNIQUE | Messenger service running for the computer`

`<username> | <03> | UNIQUE | Messenger service running for the logged-in user`

`<host name> | <20> | UNIQUE | Server service running`

`<domain> | <1D> | GROUP | Master browser name for the subnet`

`<domain> | <1B> | UNIQUE | Domain master browser name, which identifies the primary domain controller (PDC) for the domain`

`<domain> | <1E> | GROUP | Browser service election`

Ejemplo:
nbtstat -A 192.168.1.10 -r -c -s
>-A 192.168.1.10: Consulta la tabla de nombres de NetBIOS en el equipo con dirección IP 192.168.1.10.

>-r: Muestra las estadísticas de resolución de nombres NetBIOS.

>-c: Muestra la tabla de nombres NetBIOS en caché, que contiene los nombres ya resueltos.

>-s: Muestra las sesiones de NetBIOS activas y el estado de cada conexión.

Resultado de la consulta  ` nbtstat -A 192.168.1.10 -r -c -s `

>
**` NetBIOS Remote Machine Name Table `**
>
`    Name               Type         Status `

`    WORKGROUP    <00>  GROUP       Registered `

`    PC-ALICE     <03>  UNIQUE      Registered `

`    ADMIN        <03>  UNIQUE      Registered `

`    ------------------------------------------------ `

**` NetBIOS Names Resolution and Registration Statistics `**

`    Successful Name Resolution:     15 `

`    Successful Name Releases  :      5 `

`    Failed Name Resolutions   :      2 `

`    ---------------------------------------------------- `

**` NetBIOS Remote Cache Name Table `**

`    Name               Type      IP Address `

`    PC-JOHN      <20>  UNIQUE    192.168.1.11 `

`    ADMIN        <20>  UNIQUE    192.168.1.12 `

`    ------------------------------------------------------- `

**` NetBIOS Session Statistics `**

`    <Remote Machine>   <In/Out>   <TotalBytes>   <Status> `

`    192.168.1.11       Outbound   15234          Active `

`    192.168.1.12       Inbound    10294          Idle `

Explicación del Resultado:
- NetBIOS Remote Machine Name Table:
> Muestra los nombres NetBIOS registrados en el equipo 192.168.1.10.
> WORKGROUP es el grupo de trabajo al que pertenece.
> Varios nombres (como PC-ALICE, ADMIN) están registrados con diferentes tipos, representando diferentes servicios o usuarios.

- NetBIOS Names Resolution and Registration Statistics:
> Resumen de estadísticas de resolución de nombres.
> Incluye nombres resueltos con éxito, liberaciones de nombres, y fallos en las resoluciones.

- NetBIOS Remote Cache Name Table:
> Esta tabla de caché muestra los nombres NetBIOS de otros sistemas a los que el equipo ha accedido recientemente junto con sus direcciones IP.
> Nos da pistas sobre otros dispositivos en la red, como PC-JOHN, ADMIN.

- NetBIOS Session Statistics:
> Aquí se muestran las conexiones NetBIOS activas y sus estados.
> Por ejemplo, hay conexiones de entrada y salida entre 192.168.1.10 y otros dispositivos (192.168.1.11, 192.168.1.12), indicando sesiones activas y ociosas.

# 2. Técnicas de enumeración NetBIOS

La enumeración de NetBIOS es una técnica utilizada en hacking ético para obtener información específica sobre equipos que están en una red local, especialmente en entornos que utilizan sistemas Windows. NetBIOS permite a los dispositivos en la red identificarse entre sí, lo cual facilita el descubrimiento de recursos compartidos, usuarios y servicios disponibles.

## 1. Enumeración NetBIOS

Un nombre NetBIOS es una cadena única de 16 caracteres ASCII que se utiliza para identificar los dispositivos de red a través de TCP/IP; se utilizan 15 caracteres para el nombre del dispositivo y el 16o carácter está reservado para el tipo de registro de servicio o nombre.

> La resolución de nombres NetBIOS no es compatible con Microsoft para el Protocolo de Internet versión 6 (IPv6)

Objectivo de la enumeración:
 - Nombres de usuario y grupos de trabajo.
 - Recursos compartidos en la red (archivos, impresoras).
 - Políticas de seguridad y configuración.
 - Direcciones IP y nombres de equipo del dominio.

> NetBIOS se considera primero para la enumeración porque extrae una gran cantidad de información confidencial sobre la red de destino, como usuarios y recursos compartidos de red. Los atacantes generalmente apuntan al servicio NetBIOS porque es fácil de explotar y ejecutar en sistemas Windows incluso cuando no está en uso.

> Un atacante que encuentre un sistema Windows con el puerto 139 abierto puede comprobar a qué recursos se puede acceder o visualizar en un sistema remoto. Sin embargo, para enumerar los nombres NetBIOS, el sistema remoto debe tener habilitado el uso compartido de archivos e impresoras.

## 2. Herramientas

### Nbtstat

Es una utilidad en Windows que permite a los administradores de red ver la tabla de nombres NetBIOS de un equipo remoto.

Comandos útiles:
- -a < NombreRemoto >
> Muestra la tabla de nombres NetBIOS de un equipo remoto, donde NombreRemoto es el nombre de equipo NetBIOS del equipo remoto
- -A < Dirección IP >
>Muestra la tabla de nombres NetBIOS de un equipo remoto, especificada por la dirección IP (en notación decimal con puntos) del equipo remoto
- -c
>Enumera el contenido de la caché de nombres NetBIOS, la tabla de nombres NetBIOS y sus direcciones IP resueltas
- -n
>Muestra los nombres registrados localmente por aplicaciones NetBIOS como el servidor y el redirector
- -r
>Muestra un recuento de todos los nombres resueltos por un servidor de difusión o WINS
- -R
>Purga la caché de nombres y vuelve a cargar todas las entradas etiquetadas con #PRE del archivo Lmhosts
- -RR
>Libera y vuelve a registrar todos los nombres con el servidor de nombres
- -s
>Enumera la tabla de sesiones NetBIOS convirtiendo las direcciones IP de destino en nombres NetBIOS de computadora
- -S
>Enumera las sesiones NetBIOS actuales y su estado con las direcciones IP
- Interval
>Vuelve a mostrar las estadísticas seleccionadas, deteniéndose en cada visualización durante la cantidad de segundos especificada en el intervalo

Ejemplo:
nbtstat -A 192.168.1.10 -r -c -s
>-A 192.168.1.10: Consulta la tabla de nombres de NetBIOS en el equipo con dirección IP 192.168.1.10.

>-r: Muestra las estadísticas de resolución de nombres NetBIOS.

>-c: Muestra la tabla de nombres NetBIOS en caché, que contiene los nombres ya resueltos.

>-s: Muestra las sesiones de NetBIOS activas y el estado de cada conexión.

Resultado de la consulta  ` nbtstat -A 192.168.1.10 -r -c -s `

>
` NetBIOS Remote Machine Name Table `
>
`    Name               Type         Status `

`    WORKGROUP    <00>  GROUP       Registered `

`    PC-ALICE     <03>  UNIQUE      Registered `

`    PC-ALICE     <20>  UNIQUE      Registered `

`    ADMIN        <03>  UNIQUE      Registered `

`    Fileshare    <1D>  UNIQUE      Registered `

`    IS~ALICE     <1B>  UNIQUE      Registered `

`    MACBOOK      <03>  UNIQUE      Registered `

`    ------------------------------------------------ `

` NetBIOS Names Resolution and Registration Statistics `

`    Successful Name Resolution:     15 `
`    Successful Name Releases  :      5 `
`    Failed Name Resolutions   :      2 `

`    ---------------------------------------------------- `

` NetBIOS Remote Cache Name Table `

`    Name               Type      IP Address `

`    PC-JOHN      <20>  UNIQUE    192.168.1.11 `

`    ADMIN        <20>  UNIQUE    192.168.1.12 `

`    ------------------------------------------------------- `

` NetBIOS Session Statistics `

`    Connection Table: `

`    <Remote Machine>   <In/Out>   <TotalBytes>   <Status> `

`    ----------------------------------------------- `

`    192.168.1.11       Outbound   15234          Active `

`    192.168.1.12       Inbound    10294          Idle `


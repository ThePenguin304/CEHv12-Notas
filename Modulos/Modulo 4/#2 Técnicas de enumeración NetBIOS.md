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
- -a < NombreRemoto >. Muestra la tabla de nombres NetBIOS de un equipo remoto, donde NombreRemoto es el nombre de equipo NetBIOS del equipo remoto
- -A < Dirección IP >. Muestra la tabla de nombres NetBIOS de un equipo remoto, especificada por la dirección IP (en notación decimal con puntos) del equipo remoto
- -c. Enumera el contenido de la caché de nombres NetBIOS, la tabla de nombres NetBIOS y sus direcciones IP resueltas
- -n. Muestra los nombres registrados localmente por aplicaciones NetBIOS como el servidor y el redirector
- -r. Muestra un recuento de todos los nombres resueltos por un servidor de difusión o WINS
- -R. Purga la caché de nombres y vuelve a cargar todas las entradas etiquetadas con #PRE del archivo Lmhosts
- -RR. Libera y vuelve a registrar todos los nombres con el servidor de nombres
- -s. Enumera la tabla de sesiones NetBIOS convirtiendo las direcciones IP de destino en nombres NetBIOS de computadora
- -S. Enumera las sesiones NetBIOS actuales y su estado con las direcciones IP
- Interval. Vuelve a mostrar las estadísticas seleccionadas, deteniéndose en cada visualización durante la cantidad de segundos especificada en el intervalo

Ejemplo:
` nbtstat -A 192.168.1.10 -r -c -s `

>-A 192.168.1.10: Consulta la tabla de nombres de NetBIOS en el equipo con dirección IP 192.168.1.10.

>-r: Muestra las estadísticas de resolución de nombres NetBIOS.

>-c: Muestra la tabla de nombres NetBIOS en caché, que contiene los nombres ya resueltos.

>-s: Muestra las sesiones de NetBIOS activas y el estado de cada conexión.

### NetBIOS Tools

Las herramientas de enumeración NetBIOS exploran y escanean una red dentro de un rango determinado de direcciones IP y listas de computadoras para identificar fallas o vulnerabilidades de seguridad en los sistemas en red. Estas herramientas también enumeran sistemas operativos (OS), usuarios, grupos, identificadores de seguridad (SID), políticas de contraseñas, servicios, paquetes de servicios y revisiones, recursos compartidos NetBIOS, transportes, sesiones, discos y registros de eventos de seguridad, etc.

- NetBIOS Enumerator

NetBIOS Enumerator es una herramienta de enumeración que muestra cómo utilizar el soporte de red remota y cómo manejar otros protocolos web, como SMB. Los atacantes utilizan NetBIOS Enumerator para enumerar detalles como nombres NetBIOS, nombres de usuario, nombres de dominio y direcciones de control de acceso a medios (MAC) para un rango determinado de direcciones IP.

- NMAP

Tiene scripts específicos para escanear y enumerar NetBIOS en una red.

` nmap -p 137,138,139 --script nbstat <dirección IP objetivo> `

>Salida esperada: Información sobre los servicios NetBIOS en los puertos 137, 138 y 139, que puede incluir nombres NetBIOS y otros detalles de configuración.

- OTRAS

> Global Network Inventory, Advanced IP Scanner, Hyena, Nsauditor, etc.

## 3. Enumeración cuentas de usuario

Enumerar cuentas de usuario es un paso crucial en la recopilación de información durante un pentest. 

## PsTools

Herramientas de Sysinternals diseñada para administrar sistemas de Windows de manera remota.

### PsExec

PsExec permite ejecutar comandos en sistemas remotos y, cuando se combina con ciertos comandos de Windows, se convierte en una potente herramienta para la enumeración de usuarios.

Puedes ejecutar el comando net user en un sistema remoto para listar las cuentas locales usando PsExec. El comando básico sería:

 psexec \\<IP_del_objetivo> -u <usuario> -p <contraseña> cmd /c "net user" `

>  <IP_del_objetivo>: Dirección IP del sistema remoto.

>  -u <usuario> y -p <contraseña>: Credenciales de administración en el equipo remoto.

>  cmd /c "net user": Ejecuta el comando net user en el equipo remoto para listar los usuarios locales.

Firewall: PsExec necesita acceso remoto, así que el firewall debe permitirlo. En un pentest real, es útil verificar si el puerto 445 (SMB) está abierto.

### PsFile

Permite ver los archivos abiertos de manera remota en un sistema Windows. Para identificar archivos que están en uso y sus respectivos usuarios, lo cual puede ser útil para identificar accesos sospechosos o uso compartido de archivos en redes.

` psfile \\<IP_del_objetivo> `

### PsKill

Descripción: Termina procesos en sistemas locales o remotos, útil para finalizar procesos específicos que pueden estar consumiendo recursos o comportándose de forma anómala. Se puede emplear para detener procesos maliciosos o para simular actividad en pentests.

` pskill \\<IP_del_objetivo> <nombre_proceso_o_PID> `

### PsInfo

Muestra información sobre el sistema y su configuración, incluyendo detalles del hardware y el sistema operativo. Para obtener rápidamente un perfil del sistema en un entorno de auditoría.

### PsList

Lista todos los procesos y subprocesos activos en un sistema remoto. Identificación de procesos inusuales o de alto consumo de recursos, útil para la detección de malware o actividad anómala.

### PsGetSid

Obtiene el Security Identifier (SID) de una máquina o de un usuario específico. Los SIDs se utilizan para identificar usuarios y equipos, lo cual puede ser útil para la correlación de eventos o auditorías de acceso.

### PsLoggedOn

Muestra los usuarios actualmente conectados a un sistema, incluyendo sesiones locales y remotas. Enumerar sesiones activas es útil para evaluar actividad de usuarios y posibles puntos de entrada.

` psloggedon \\<IP_del_objetivo> `


### PsLogList

Extrae eventos desde el registro de eventos de Windows. Ayuda a identificar eventos específicos como intentos de acceso fallidos, errores de sistemas, y actividad de inicio de sesión.

Descripción: Permite cambiar contraseñas de usuarios en el sistema local o remoto.
Uso: Para actualizar contraseñas sin tener que iniciar sesión en cada máquina, especialmente útil en auditorías de acceso o administración de cuentas.

### PsPasswd

Permite cambiar contraseñas de usuarios en el sistema local o remoto. Para actualizar contraseñas sin tener que iniciar sesión en cada máquina, especialmente útil en auditorías de acceso o administración de cuentas.

` pspasswd \\<IP_del_objetivo> <usuario> <nueva_contraseña> `

### PsShutdown

Cierra o reinicia sistemas remotos. Puede emplearse para simular fallas de sistemas en pruebas de recuperación o en gestión de servidores.

` psshutdown \\<IP_del_objetivo> -r -t 60 `

## 4. Enumeración de recursos compartidos mediante Net View

Net View es un comando de Windows que permite ver los recursos compartidos en una red.

` net view \\<nombre del equipo o IP> `

>Salida esperada: Listado de recursos compartidos, como carpetas o impresoras accesibles en la red. Esta información es útil para identificar qué recursos podrían ser vulnerables a un acceso no autorizado.

` net view \\<computername> /ALL `
>El comando anterior muestra todos los recursos compartidos en la computadora remota especificada, junto con los recursos compartidos ocultos.

` net view /domain `
>El comando anterior muestra todos los recursos compartidos en el dominio.

`net view /domain:<domain name>`
>El comando anterior muestra todos los recursos compartidos en el dominio especificado.

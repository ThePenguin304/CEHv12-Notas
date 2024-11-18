# Técnicas de Enumeración NetBIOS

La enumeración de NetBIOS es una técnica utilizada en hacking ético para obtener información detallada sobre equipos en una red local, especialmente en entornos Windows. NetBIOS (Network Basic Input/Output System) es un protocolo que facilita la comunicación entre dispositivos de una red, permitiendo la identificación de recursos compartidos, usuarios y servicios disponibles.

**Redirección a [Información General](https://github.com/ThePenguin304/CEHv12-Notas/blob/main/Modulos/Modulo%204/README.md)**

---

## 1. ¿Qué es NetBIOS?

NetBIOS es un conjunto de servicios que opera en la capa de sesión del modelo OSI y permite que las aplicaciones en una red se comuniquen entre sí. Proporciona servicios como la resolución de nombres, el manejo de conexiones y la gestión de sesiones de red. Aunque originalmente fue diseñado para redes locales, NetBIOS sigue siendo ampliamente utilizado en redes Windows.

Un nombre NetBIOS tiene una longitud máxima de 16 caracteres ASCII. Los primeros 15 caracteres representan el nombre del dispositivo o recurso en la red, y el último carácter se reserva para denotar el tipo de servicio o nombre.

> **Importante:** NetBIOS no es compatible con IPv6 en sistemas Microsoft.

### Objetivos de la enumeración NetBIOS:

- **Nombres de usuario y grupos de trabajo.**
- **Recursos compartidos en la red** (archivos, impresoras).
- **Políticas de seguridad y configuración.**
- **Direcciones IP y nombres de equipo del dominio.**

> **Razón de su uso:** NetBIOS es una fuente rica de información confidencial sobre la red, como nombres de usuarios, recursos compartidos y configuraciones. Esto lo convierte en un objetivo prioritario para los atacantes, quienes pueden explotar vulnerabilidades aunque NetBIOS no esté en uso activo.

---

## 2. Herramientas para la Enumeración NetBIOS

### Nbtstat

`nbtstat` es una herramienta de línea de comandos de Windows que permite ver la tabla de nombres NetBIOS de un equipo remoto.

#### Comandos útiles:

|Comandos|Descripción|
|-------|-----|
|`-a <NombreRemoto>`|Muestra la tabla de nombres NetBIOS de un equipo remoto, donde NombreRemoto es el nombre de equipo NetBIOS del equipo remoto|
|`-A <NombreRemoto>`|Muestra la tabla de nombres NetBIOS de un equipo remoto, especificada por la dirección IP (en notación decimal con puntos) del equipo remoto|
|-c|Enumera el contenido de la caché de nombres NetBIOS, la tabla de nombres NetBIOS y sus direcciones IP resueltas|
|-n|Muestra los nombres registrados localmente por aplicaciones NetBIOS como el servidor y el redirector|
|-r|Muestra un recuento de todos los nombres resueltos por un servidor de difusión o WINS|
|-R|Purga la caché de nombres y vuelve a cargar todas las entradas etiquetadas con #PRE del archivo Lmhosts|
|-RR|Libera y vuelve a registrar todos los nombres con el servidor de nombres|
|-s|Enumera la tabla de sesiones NetBIOS convirtiendo las direcciones IP de destino en nombres NetBIOS de computadora|
|-S|Enumera las sesiones NetBIOS actuales y su estado con las direcciones IP|
|-Interval|Vuelve a mostrar las estadísticas seleccionadas, deteniéndose en cada visualización durante la cantidad de segundos especificada en el intervalo

Ejemplo:
```bash 
nbtstat -A 192.168.1.10 -r -c -s `
```

>-A 192.168.1.10: Consulta la tabla de nombres de NetBIOS en el equipo con dirección IP 192.168.1.10.

>-r: Muestra las estadísticas de resolución de nombres NetBIOS.

>-c: Muestra la tabla de nombres NetBIOS en caché, que contiene los nombres ya resueltos.

>-s: Muestra las sesiones de NetBIOS activas y el estado de cada conexión.

Redirección a [NetBIOS Name List](https://github.com/ThePenguin304/CEHv12-Notas/blob/main/Modulos/Modulo%204/NetBIOS%20Name%20List%20Consulta.md)

### NetBIOS Tools

Las herramientas de enumeración NetBIOS exploran y escanean una red dentro de un rango determinado de direcciones IP y listas de computadoras para identificar fallas o vulnerabilidades de seguridad en los sistemas en red. Estas herramientas también enumeran sistemas operativos (OS), usuarios, grupos, identificadores de seguridad (SID), políticas de contraseñas, servicios, paquetes de servicios y revisiones, recursos compartidos NetBIOS, transportes, sesiones, discos y registros de eventos de seguridad, etc.

- NetBIOS Enumerator

NetBIOS Enumerator es una herramienta de enumeración que muestra cómo utilizar el soporte de red remota y cómo manejar otros protocolos web, como SMB. Los atacantes utilizan NetBIOS Enumerator para enumerar detalles como nombres NetBIOS, nombres de usuario, nombres de dominio y direcciones de control de acceso a medios (MAC) para un rango determinado de direcciones IP.

- NMAP

Tiene scripts específicos para escanear y enumerar NetBIOS en una red.

```bash
nmap -p 137,138,139 --script nbstat <dirección IP objetivo>
```

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

```bash
psexec \\<IP_del_objetivo> -u <usuario> -p <contraseña> cmd /c "net user"
```

>  <IP_del_objetivo>: Dirección IP del sistema remoto.

>  -u <usuario> y -p <contraseña>: Credenciales de administración en el equipo remoto.

>  cmd /c "net user": Ejecuta el comando net user en el equipo remoto para listar los usuarios locales.

Firewall: PsExec necesita acceso remoto, así que el firewall debe permitirlo. En un pentest real, es útil verificar si el puerto 445 (SMB) está abierto.

### PsFile

Permite ver los archivos abiertos de manera remota en un sistema Windows. Para identificar archivos que están en uso y sus respectivos usuarios, lo cual puede ser útil para identificar accesos sospechosos o uso compartido de archivos en redes.

```bash
psfile \\<IP_del_objetivo>
```

### PsKill

Descripción: Termina procesos en sistemas locales o remotos, útil para finalizar procesos específicos que pueden estar consumiendo recursos o comportándose de forma anómala. Se puede emplear para detener procesos maliciosos o para simular actividad en pentests.

```bash
pskill \\<IP_del_objetivo> <nombre_proceso_o_PID>
```

### PsInfo

Muestra información sobre el sistema y su configuración, incluyendo detalles del hardware y el sistema operativo. Para obtener rápidamente un perfil del sistema en un entorno de auditoría.

### PsList

Lista todos los procesos y subprocesos activos en un sistema remoto. Identificación de procesos inusuales o de alto consumo de recursos, útil para la detección de malware o actividad anómala.

### PsGetSid

Obtiene el Security Identifier (SID) de una máquina o de un usuario específico. Los SIDs se utilizan para identificar usuarios y equipos, lo cual puede ser útil para la correlación de eventos o auditorías de acceso.

### PsLoggedOn

Muestra los usuarios actualmente conectados a un sistema, incluyendo sesiones locales y remotas. Enumerar sesiones activas es útil para evaluar actividad de usuarios y posibles puntos de entrada.

```bash
psloggedon \\<IP_del_objetivo>
```

### PsLogList

Extrae eventos desde el registro de eventos de Windows. Ayuda a identificar eventos específicos como intentos de acceso fallidos, errores de sistemas, y actividad de inicio de sesión.

Descripción: Permite cambiar contraseñas de usuarios en el sistema local o remoto.
Uso: Para actualizar contraseñas sin tener que iniciar sesión en cada máquina, especialmente útil en auditorías de acceso o administración de cuentas.

### PsPasswd

Permite cambiar contraseñas de usuarios en el sistema local o remoto. Para actualizar contraseñas sin tener que iniciar sesión en cada máquina, especialmente útil en auditorías de acceso o administración de cuentas.

```bash
pspasswd \\<IP_del_objetivo> <usuario> <nueva_contraseña>
```

### PsShutdown

Cierra o reinicia sistemas remotos. Puede emplearse para simular fallas de sistemas en pruebas de recuperación o en gestión de servidores.

```bash
psshutdown \\<IP_del_objetivo> -r -t 60
```

## 4. Enumeración de recursos compartidos mediante Net View

Net View es un comando de Windows que permite ver los recursos compartidos en una red.

```bash
net view \\<nombre del equipo o IP>
```

```bash
net view \\192.168.1.10

Shared resources at \\192.168.1.10

Share name     Type      Used as  Comment
-------------------------------------------
ADMIN$         Disk               Remote Admin
C$             Disk               Default share
IPC$           IPC                Remote IPC
Documents      Disk               Shared folder for company files
Software       Disk               Software repository
```

Salida esperada: Listado de recursos compartidos, como carpetas o impresoras accesibles en la red. Esta información es útil para identificar qué recursos podrían ser vulnerables a un acceso no autorizado.

>IPC$ es un recurso compartido especial en sistemas Windows conocido como Inter-Process Communication (IPC) share. Este recurso no es una carpeta compartida tradicional sino un mecanismo que permite la comunicación entre procesos y compartición de datos entre diferentes partes de un sistema, generalmente usado para tareas administrativas remotas.



```bash
net view \\<computername> /ALL
```

>El comando anterior muestra todos los recursos compartidos en la computadora remota especificada, junto con los recursos compartidos ocultos.

```bash
net view /domain
```

>El comando anterior muestra todos los recursos compartidos en el dominio.

```bash
net view /domain:<domain name>
```

>El comando anterior muestra todos los recursos compartidos en el dominio especificado.

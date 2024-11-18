# 2. Técnicas de enumeración SNMP

Redirección a [Información General](https://github.com/ThePenguin304/CEHv12-Notas/blob/main/Modulos/Modulo%204/README.md)

Protocolo que trabaja con clientes en los equipos y un controlador general. Permite a los administradores controlar los dispositvos de una red de forma remota. 

> Dispone de vulnerabilidades si existe una mala configuración que pueden ser explotadas para obtener información de cuentas y dispositivos que forma la red objetivo.
 
>Notas: Las `Default Community Strings` (o "cadenas de comunidad por defecto") son identificadores predefinidos utilizados en el protocolo SNMP (Simple Network Management Protocol) para controlar y monitorear dispositivos en una red. Estas cadenas actúan como contraseñas que permiten el acceso a la información de gestión de dispositivos como routers, switches, impresoras y servidores.

En el protocolo SNMP existen dos tipos de `Comunity Strings` (contraseñas de control de acceso), modo `read` y modo `read and write`. La primera por defecto es `public` y la segunda `private`. Con la primera puede obtener información pero no modificarla.

>Cuando los administradores dejan las cadenas de comunidad en la configuración predeterminada, los atacantes pueden usar estas cadenas de comunidad predeterminadas (contraseñas) para cambiar o ver la configuración del dispositivo o sistema.

>Notas: SNMPv3 ofrece autenticación y cifrado robustos, protocolo más seguro.

La información extraída con la enumeración SNMP está relacionada con recursos de red, como hosts, enrutadores, dispositivos y recursos compartidos, e información de red, como tablas ARP, tablas de enrutamiento y el tráfico.

>Nota: SNMP es un protocolo de capa de aplicación que se ejecuta en UDP y mantiene y administra enrutadores, concentradores y conmutadores en una red IP. Los agentes SNMP se ejecutan en redes Windows y Unix en dispositivos de red.

> Herramienta usada: OpUtils

Comandos (PDU = Protocol Data Units) asociados al SNMP:

|Comando|Descripción|
|---|---|
|GetRequest|Solicita información específica a un agente SNMP
|GetNextRequest|Permite la enumeración secuencial de todos los valores dentro de una tabla de OIDs, como una lista de interfaces o procesos en un dispositivo
|GetResponse|Es la respuesta que el agente SNMP envía en respuesta a comandos como GetRequest, GetNextRequest o SetRequest
|SetRequest|Modifica o establece el valor de una variable en el agente
|Trap|Notifica a la estación de administración un evento específico en el agente

Ejemplo de Flujo SNMP:

```bash
- GetRequest: La estación de administración solicita la tasa de uso de CPU en un dispositivo.
- GetResponse: El agente envía el valor actual de la tasa de uso de CPU.
- Trap: Si el dispositivo detecta que la CPU está sobrecargada, envía un trap automáticamente para notificar el problema.
- SetRequest: La estación de administración envía una orden para cambiar la configuración del umbral de uso de CPU.
- GetNextRequest: La estación consulta todas las interfaces en el dispositivo usando GetNextRequest para obtener datos secuenciales.
```

## 1. MIB

La INFO de SNMP está en el MIB (Management Information Base).

> El MIB es una estructura de datos que contiene la información que los agentes SNMP pueden monitorear y gestionar en un dispositivo de red. La MIB define qué datos pueden ser consultados o configurados en un dispositivo mediante SNMP y organiza esta información en una jerarquía de OIDs (Object Identifiers), que son identificadores únicos para cada variable de administración.

http://IP.Address/Lseries.mib o http://library_name/Lseries.mib.Microsoft proporciona la lista de MIB que se instalan con el servicio SNMP en el kit de recursos de Windows:

```bash
- DHCP.MIB: Monitors network traffic between DHCP servers and remote hosts
- HOSTMIB.MIB: Monitors and manages host resources
- LNMIB2.MIB: Contains object types for workstation and server services
- MIB_II.MIB: Manages TCP/IP-based Internet using a simple architecture and system
- WINS.MIB: For the Windows Internet Name Service (WINS)
```
## 2. Herramientas

Encontramos SnmpWalk y Nmap

### SnmpWalk

Herramienta dedicada a interactuar con agentes SNMP y permite realizar un recorrido secuencial (o "camino") de los OIDs en la MIB (Management Information Base).

```bash
snmpwalk -v <versión> -c <community_string> <IP_del_objetivo>

-v: Especifica la versión de SNMP a usar. Puede ser 1, 2c, o 3.
-c: Define la community string, que es la "contraseña" para acceder a los datos. Las cadenas de comunidad predeterminadas son "public" (solo lectura) y "private" (lectura/escritura).
IP_del_objetivo: La dirección IP del dispositivo objetivo.
```

```bash
snmpwalk -v 2c -c public 192.168.1.100

Este comando solicita la versión SNMPv2c para conectarse al dispositivo 192.168.1.100 usando la cadena de comunidad public. snmpwalk intentará extraer toda la información accesible del dispositivo usando esta configuración.
```

El comando anterior puede devolver una lista de OIDs con sus valores, por ejemplo:

```bash
SNMPv2-MIB::sysDescr.0 = STRING: Linux server 3.10.0-957.el7.x86_64
SNMPv2-MIB::sysUpTime.0 = Timeticks: (452457) 1:15:24.57
SNMPv2-MIB::ifNumber.0 = INTEGER: 2
SNMPv2-MIB::ifDescr.1 = STRING: eth0
SNMPv2-MIB::ifDescr.2 = STRING: wlan0
```
Otros comandos:

Comando para enumerar SNMPv2 con una cadena de comunidad "public":
```bash
snmpwalk -v2c -c public <Dirección IP del objetivo>
```

Comando para buscar software instalado:
```bash
snmpwalk -v2c -c public <Dirección IP del objetivo> hrSWInstalledName
```

Comando para determinar la cantidad de RAM en el host:
```bash
snmpwalk -v2c -c public <Dirección IP del objetivo> hrMemorySize
```

Comando para cambiar un OID a un valor diferente:
```bash
snmpwalk -v2c -c public <Dirección IP del objetivo> <OID> <Nuevo Valor>
```

Comando para cambiar el OID de sysContact:
```bash
snmpwalk -v2c -c public <Dirección IP del objetivo> sysContact <Nuevo Valor>
```

Estos comandos permiten interactuar y modificar variables en un dispositivo con SNMPv2 utilizando una cadena de comunidad conocida (public).

## NMAP

Tiene capacidades de enumeración SNMP mediante scripts específicos de NSE (Nmap Scripting Engine) diseñados para interactuar con SNMP. Estos scripts son útiles para recolectar datos SNMP rápidamente y sin necesidad de conocer en detalle los OIDs específicos.

```bash
nmap -sU -p 161 --script snmp-* <IP_del_objetivo>

-sU: Realiza un escaneo de puertos UDP, necesario ya que SNMP utiliza el puerto UDP 161.
-p 161: Especifica el puerto 161, el puerto predeterminado de SNMP.
--script snmp-*: Indica que se usen todos los scripts de SNMP que Nmap tiene disponibles.
```

```bash
nmap -sU -p 161 --script snmp-brute,snmp-info 192.168.1.100

Starting Nmap 7.91 ( https://nmap.org ) at 2024-11-12 15:32 UTC
Nmap scan report for 192.168.1.100
Host is up (0.0032s latency).
PORT    STATE SERVICE
161/udp open  snmp
| snmp-info:
|   Device: Linux
|   Hostname: server
|   Uptime: 4 days, 2:15:42
|   Contact: admin@example.com
|   Location: Data Center 1
|_  Interfaces: 2 (eth0, wlan0)

Nmap done: 1 IP address (1 host up) scanned in 2.53 secondss.
```

>`snmp-brute` Realiza un ataque de fuerza bruta contra SNMP para descubrir community strings válidas.

>`snmp-info` Extrae información básica del dispositivo como el nombre del sistema, descripción, tiempo de actividad y detalles de interfaz.

>`snmp-processes` Permite enumerar los procesos activos en un sistema a través de SNMP. Esto es útil para obtener una lista de procesos que se están ejecutando en el dispositivo, lo que puede ser importante para evaluar el estado o las aplicaciones activas en el sistema.

>`snmp-sysDescr` (System Description) Proporciona una descripción general del sistema o dispositivo, lo que incluye detalles como el sistema operativo, la versión del software o la arquitectura del hardware. Este es uno de los OIDs más comunes y se utiliza para identificar el tipo y versión del dispositivo.

## Resumen

* Snmpwalk es excelente para obtener un recorrido exhaustivo de OIDs cuando se conocen las community strings válidas.
* Nmap con scripts de SNMP permite una enumeración rápida y amplia, incluyendo ataques de fuerza bruta para descubrir community strings.

## Otras herramientas

### SNMPCheck

`Snmp-check` es una herramienta diseñada para realizar una recopilación de información utilizando el protocolo SNMP (Simple Network Management Protocol). Es muy útil para obtener detalles de dispositivos de red, sistemas operativos, configuración de hardware, software instalado y otras informaciones de interés en una red.

### SoftPerfect Network Scanner

`SoftPerfect Network Scanner` es una herramienta de escaneo de red avanzada que permite descubrir dispositivos en la red, obtener información de SNMP, ICMP y otros servicios, y realizar diversas tareas de auditoría y administración de red.


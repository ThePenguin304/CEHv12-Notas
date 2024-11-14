# 1. Conceptos básicos de la enumeración

Redirección a [Información General](https://github.com/ThePenguin304/CEHv12-Notas/blob/main/Modulos/Modulo%204/%230%20Info%20general.md)

## 1. ¿Qué es la Enumeración?

La **enumeración** es el proceso activo mediante el cual se recopila información detallada sobre sistemas, redes o servicios de un objetivo después de realizar un escaneo de red. A diferencia de la fase de **reconocimiento**, que no involucra interacción directa, la enumeración establece conexiones activas con el objetivo (target).

Se utiliza para identificar recursos compartidos, usuarios, grupos, puertos abiertos, servicios disponibles y otras configuraciones de red que podrían ser aprovechadas en ataques posteriores, como el acceso no autorizado a recursos del sistema objetivo o ataques de contraseñas.

> Durante la enumeración, es posible que se dejen rastros en los registros de los sistemas. Este proceso es más intrusivo que la fase de reconocimiento.

> Un recurso comúnmente encontrado en sistemas Windows durante la enumeración es el **IPC$**, que permite acceder a recursos compartidos administrativos mediante técnicas de fuerza bruta para obtener credenciales de administrador.

> Las actividades de enumeración generalmente se realizan dentro de la red interna (intranet) del objetivo.

> Es importante tener en cuenta que realizar enumeración sin autorización puede ser ilegal según las leyes locales y las políticas de la organización. Un hacker ético debe siempre obtener la autorización correspondiente antes de realizar este tipo de actividades.

---

### Información que se recolecta durante la enumeración:
- Recursos y servicios de red compartidos, incluyendo tablas de enrutamiento.
- Configuración de auditoría y servicios.
- Detalles sobre SNMP (Simple Network Management Protocol) y nombre de dominio completo (FQDN).
- Nombres de máquinas, usuarios y grupos, además de aplicaciones y banners de servicios.

---

## 2. Técnicas para la Enumeración

### 2.1. Extracción de los usuarios usando los ID's de los mails

Esta técnica se basa en la obtención de nombres de usuario o IDs de correo electrónico asociados a un dominio específico, lo cual facilita la identificación de cuentas válidas en el sistema objetivo.

> Herramientas como **Nmap** y **Metasploit** permiten realizar consultas SMTP o EXPN para obtener direcciones de correo electrónico y sus respectivos nombres de usuario (username@domain).

- **Herramientas**: Nmap, Metasploit.

---

### 2.2. Extracción de información usando contraseñas por defecto

Muchos dispositivos y sistemas operativos utilizan contraseñas predeterminadas, que son conocidas o fácilmente adivinables. Los atacantes aprovechan estas contraseñas para acceder sin autorización a los sistemas.

> Los atacantes intentan acceder a los servicios utilizando contraseñas por defecto que están ampliamente disponibles en bases de datos públicas o documentación de fabricantes.

- **Herramientas**: Bases de datos de contraseñas por defecto, Hydra, Medusa.

---

### 2.3. Extracción de información usando fuerza bruta en Active Directory (AD)

En entornos de **Windows** con **Active Directory (AD)**, los atacantes pueden realizar ataques de fuerza bruta para adivinar contraseñas de usuarios y obtener acceso a las cuentas.

> Este tipo de ataques pueden llevarse a cabo de manera online (directamente sobre el sistema de login) o de forma offline (mediante hashes de contraseñas obtenidos previamente). Un aspecto importante a considerar es el comportamiento de **Microsoft AD** cuando se habilitan las "horas de inicio de sesión", lo que puede ayudar a los atacantes a identificar nombres de usuario válidos mediante errores de autenticación.

- **Herramientas**: Hydra, John the Ripper, CrackMapExec, Impacket.

---

### 2.4. Extracción de información mediante "DNS Zone Transfer"

Un **DNS Zone Transfer** permite a los atacantes obtener una copia completa de la base de datos DNS de un servidor, revelando información sobre subdominios, direcciones IP y otros detalles clave de la infraestructura de la red.

> Si un servidor DNS está mal configurado, puede permitir transferencias de zona no autorizadas, lo que facilita la recopilación de información sobre la red del objetivo.

- **Herramientas**: Dig, Nslookup, Nmap (usando scripts NSE para transferencia de zona DNS).

---

### 2.5. Extracción de grupos de usuarios en Windows

En entornos **Windows**, los grupos de usuarios pueden proporcionar información valiosa sobre la estructura y privilegios de los usuarios dentro de un dominio o máquina local.

> Para extraer información sobre los grupos de usuarios en **Active Directory**, el atacante necesita una cuenta registrada en el sistema. Esto permite obtener una lista de grupos como "Administradores", "Usuarios", "Operadores de servidores", entre otros.

- **Herramientas**: Net, PowerShell, Enum4Linux, Metasploit, Impacket.

---

### 2.6. Extracción de nombres de usuario usando SNMP

El **Simple Network Management Protocol (SNMP)** se utiliza para gestionar y supervisar dispositivos de red. Si está mal configurado, puede exponer información sensible, incluidos los nombres de usuario en dispositivos como routers y switches.

> Los atacantes pueden hacer consultas SNMP para obtener detalles sobre los usuarios de dispositivos gestionados, como servidores y otros dispositivos de red.

- **Herramientas**: Snmpwalk, Snmpget, Metasploit, Nmap.

---

## 3. Servicios y Puertos Clave para Enumerar

### SMB (Server Message Block)
- **Puerto**: 139 TCP, 445 TCP/UDP
- **Descripción**: Utilizado para compartir archivos e impresoras en redes **Windows**. Puede revelar carpetas compartidas y usuarios.

### SNMP (Simple Network Management Protocol)
- **Puerto**: 161 UDP
- **Descripción**: Protocolo utilizado para administrar dispositivos de red. Puede exponer detalles sobre la configuración del sistema.

### LDAP (Lightweight Directory Access Protocol)
- **Puerto**: 389 TCP/UDP (sin cifrar), 636 (con SSL)
- **Descripción**: Utilizado para acceder a directorios, especialmente **Active Directory**.

### DNS (Domain Name System)
- **Puerto**: 53 TCP/UDP
- **Descripción**: Resolución de nombres de dominio y gestión de registros de zona DNS. Es útil para obtener registros de zona y estructuras de subdominios.

### NTP (Network Time Protocol)
- **Puerto**: 123 UDP
- **Descripción**: Utilizado para la sincronización de tiempo en redes. Es útil para comprobar diferencias de hora y registros de eventos.

### SMTP (Simple Mail Transfer Protocol)
- **Puerto**: 25, 465 (con SSL), 587 (con TLS)
- **Descripción**: Protocolo para el envío de correos electrónicos. Permite enumerar usuarios y detectar vulnerabilidades como el **email spoofing**.

### RPC (Remote Procedure Call)
- **Puerto**: 135 TCP/UDP
- **Descripción**: Permite la comunicación entre diferentes servicios y dispositivos. Es crucial en entornos **Windows**.

### NetBIOS Name Service (NBNS)
- **Puerto**: 137 UDP
- **Descripción**: Resolución de nombres **NetBIOS** en una red local, similar al DNS pero para redes que utilizan **NetBIOS**.

### NetBIOS Datagram Service
- **Puerto**: 138 UDP
- **Descripción**: Transmisión de datagramas sin conexión, útil para aplicaciones que no requieren comunicación continua.

### NetBIOS Session Service
- **Puerto**: 139 TCP
- **Descripción**: Permite la comunicación basada en sesión entre dispositivos, utilizada para compartir archivos e impresoras en redes **Windows**.

---

Es recomendable bloquear estos puertos (137-139) en redes externas, ya que pueden ser utilizados para ataques de enumeración, acceso no autorizado y otros vectores de ataque.

# 1. Conceptos básicos de la enumeración

## 1. ¿Qué es la Enumeración?

La enumeración es un proceso activo que busca recopilar información detallada sobre sistemas, redes o servicios de un objetivo después de realizar un escaneo de red. Para ello se crean conexiones activas con el "target".

Se usa para identificar usuarios, grupos, recursos compartidos, servicios, puertos abiertos y otras configuraciones de red. Posteriormente, se realizaran ataques al sistema y ataques a contraseñas para conseguir acceso a los recursos del sistema objetivo.

> Esta fase implica interactuar directamente con el sistema objetivo, lo cual puede dejar rastros en los registros, a diferencia de la fase de reconocimiento.

> Durante la enumeración, los atacantes pueden encontrarse con un recurso compartido de comunicación entre procesos (IPC) remoto, como IPC$ en Windows, que pueden investigar más a fondo para conectarse a un recurso compartido administrativo mediante la fuerza bruta de las credenciales de administrador y obtener información completa sobre la lista del sistema de archivos que representa el recurso compartido.

> Las ténicas se realizan en la intranet del target.

> Las actividades de enumeración pueden ser ilegales según las políticas de la organización y las leyes vigentes
    
> Un hacker ético siempre debe obtener la autorización correspondiente.

---

Información que se recolecta:
- Recursos de red y recursos compartidos de red, así como tablas de enrutamiento.

- Configuración de auditoría y servicio.

- Detalles de SNMP y nombre de dominio completo (FQDN).

- Nombres de máquinas, usuarios y grupos, así como aplicaciones y banners.

---

## 2. Técnicas para la enumeración

***1. Extracción de los usuarios usando los ID's de los mails***

  Esta técnica se basa en la extracción de los nombres de usuario o IDs de correo electrónico asociados a un dominio, lo cual puede facilitar la obtención de información sobre cuentas válidas en el sistema objetivo.

  > Se pueden usar diversas herramientas o consultas de correo electrónico (como SMTP o EXPN) para obtener las direcciones de correo y sus IDs asociados (username@domainname).
  
  - Herramientas: Nmap, Metasploit

***2.  Extracción de información usando contraseñas por defecto***

  Muchos dispositivos y sistemas operativos utilizan contraseñas por defecto que son conocidas o fácilmente adivinables. Los atacantes pueden aprovechar estas contraseñas predeterminadas para obtener acceso no autorizado a los sistemas.

  > Se intenta acceder a los servicios con las contraseñas por defecto que están listadas en bases de datos públicas o en documentación de los fabricantes. Esto incluye routers, impresoras, bases de datos y otros dispositivos conectados a la red.

 - Herramientas: Bases de datos de contraseñas por defecto, Hydra, Medusa.

***3. Extracción de infomración usando fuerza bruta en AD***

En un entorno de Windows con Active Directory (AD), los atacantes pueden utilizar ataques de fuerza bruta para adivinar contraseñas de usuarios y obtener acceso a cuentas.

> Se intenta adivinar las contraseñas de los usuarios en el directorio activo mediante un ataque de fuerza bruta, que puede ser realizado utilizando diccionarios o combinaciones de caracteres. Esto puede hacerse de forma online (directamente en el sistema de login) o offline (mediante hashes de contraseñas obtenidos previamente).

> Existe un error de diseño en la implementación de Microsoft Active Directory. Si un usuario habilita la función de “logon hours”, todos los intentos de autenticación del servicio generan mensajes de error diferentes. Los atacantes aprovechan esto para enumerar nombres de usuario válidos. Un atacante que logre extraer nombres de usuario válidos puede realizar un ataque de fuerza bruta para descifrar las contraseñas respectivas. 

 - Herramientas: Hydra, John the Ripper, CrackMapExec, Impacket.

***4. Extracción de información usando "DNS Zone Transfer"***

  Un "DNS Zone Transfer" permite a un atacante obtener una copia completa de la base de datos DNS de un servidor, que contiene información sobre los subdominios, direcciones IP y otros detalles clave de la red de la víctima.

  > Si el servidor DNS está configurado incorrectamente, puede permitir transferencias de zona no autorizadas. El atacante puede solicitar una copia de la base de datos DNS para obtener información sobre la infraestructura del objetivo.

 - Herramientas: Dig, Nslookup, Nmap (con scripts NSE para transferencia de zona DNS).

***5. Extracción de grupos de usuarios en Windows***

En un entorno Windows, los grupos de usuarios pueden proporcionar una gran cantidad de información sobre la estructura y privilegios de los usuarios dentro de un dominio o máquina.

    Para extraer grupos de usuarios de Windows, el atacante debe tener una identificación registrada como usuario en Active Directory.

> Utilizando herramientas que interactúan con el sistema Windows, se puede enumerar los grupos de usuarios, lo cual ayuda a identificar posibles objetivos para el escalado de privilegios o ataques internos. Esto puede incluir grupos como "Administradores", "Usuarios", "Operadores de servidores", etc.

- Herramientas: Net, PowerShell, Enum4Linux, Metasploit, Impacket.

***6. Extracción de nombres de usuario usando SNMP***

El protocolo SNMP se utiliza para gestionar y supervisar dispositivos de red. Si está mal configurado, puede exponer información sensible sobre la red, incluyendo los nombres de usuario de los dispositivos gestionados.

> Utilizando herramientas específicas, un atacante puede realizar consultas SNMP para obtener información sobre los usuarios en dispositivos de red, como routers, switches, servidores, etc.

- Herramientas: Snmpwalk, Snmpget, Metasploit, Nmap. 

---

## 3. Servicios y Puertos Clave para Enumerar
***SMB (Server Message Block):***
> Puerto: 139 TCP) y 445 (TCP/UDP)

> Se utiliza para compartir archivos e impresoras en redes Windows. Puede revelar carpetas compartidas y usuarios.

***SNMP (Simple Network Management Protocol):***
> Puerto: 161 (UDP)

> Utilizado para la administración de dispositivos de red. Puede revelar información detallada del sistema.

***LDAP (Lightweight Directory Access Protocol):***
> Puerto: 389 (TCP/UDP) (sin cifrar) y 636 (con cifrado SSL)

> Protocolo de acceso a directorios que se usa para extraer información de Active Directory y otros servicios de directorio.

***DNS (Domain Name System):***
> Puerto: 53 (TCP/UDP)

> Resolución de nombres de dominio y registro de zonas DNS. Útil para obtener registros de zona y estructuras de subdominios.

***NTP (Network Time Protocol):***
> Puerto: 123

> Sincronización de tiempo en redes. Útil para comprobar diferencias de hora y logs.

***SMTP (Simple Mail Transfer Protocol):***
> Puerto: 25, 465 (con SSL) y 587 (con TLS)

> Protocolo para el envío de correos electrónicos. Permite enumerar usuarios y detectar si el objetivo es vulnerable a ataques de email spoofing.

***RCP (Remote Procedure Call):***
> Puerto: 135 (TCP/UDP)

> Permite la interacción entre diferentes servicios y dispositivos. Es crítico en entornos Windows y puede exponer vulnerabilidades en los servicios RPC.

***NetBIOS Name Service (NBNS)***
> Puerto 137 (UDP)

> Este servicio se utiliza para la resolución de nombres NetBIOS en una red. Permite que los equipos se identifiquen y resuelvan nombres NetBIOS a direcciones IP en una red local. Similar al servicio DNS, pero específicamente para redes locales que utilizan NetBIOS. Es esencial para la comunicación y descubrimiento de dispositivos dentro de una LAN.

***NetBIOS Datagram Service***
> Puerto 138 (UDP)

> Este servicio se utiliza para la transmisión de datagramas de NetBIOS, que son mensajes de datos enviados de un dispositivo a otro sin establecer una conexión directa. Permite la transmisión de mensajes y datos sin necesidad de mantener una conexión permanente, lo que lo hace útil para aplicaciones que requieren comunicaciones de un solo sentido o sin conexión.

***NetBIOS Session Service***
> Puerto 139 (TCP)

> Este puerto permite la comunicación de sesión entre dos dispositivos, proporcionando una comunicación basada en conexión (como TCP/IP).
Función: Es el puerto utilizado para compartir archivos e impresoras en una red local utilizando el protocolo SMB (Server Message Block) a través de NetBIOS. Este servicio es esencial para la conexión de dispositivos y la transferencia de archivos en sistemas Windows.

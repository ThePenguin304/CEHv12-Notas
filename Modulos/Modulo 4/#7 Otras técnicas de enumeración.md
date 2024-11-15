# 7. Enumeración IPsec, VoIP, RPC, Unix/Linux, Telnet, FTP, TFTP, SMB, IPv6, y BGP

## Enumeración IPsec

IPsec (Internet Protocol Security) es un conjunto de protocolos diseñados para proporcionar seguridad en las comunicaciones sobre redes IP, como Internet. IPsec protege los datos transmitidos mediante mecanismos de cifrado, autenticación y verificación de integridad, lo que asegura que las comunicaciones sean confidenciales y estén protegidas contra alteraciones o accesos no autorizados.

Componentes Clave de IPsec
- Protocolo AH (Authentication Header). Proporciona autenticación e integridad, pero no cifrado. Protocolo IP 51.
- Protocolo ESP (Encapsulating Security Payload). Proporciona cifrado, autenticación e integridad. Protocolo IP 50.
- IKE (Internet Key Exchange). Protocolo utilizado para establecer y gestionar asociaciones de seguridad (SA), que definen cómo se protegerán los datos. Versiones: IKEv1 (obsoleto) e IKEv2 (moderno) `Puerto 500`.

Modos de Funcionamiento
- Modo Transporte. Protege solo la carga útil (datos) del paquete IP, dejando intacto el encabezado IP. Se utiliza típicamente en comunicaciones extremo a extremo, como entre dos hosts.
- Modo Túnel. Protege el paquete IP completo, incluyendo el encabezado, encapsulándolo en un nuevo paquete IP. Se usa principalmente para conexiones VPN (Virtual Private Network), como entre gateways o firewalls.

La **enumeración de IPsec (Internet Protocol Security)** se refiere a la recopilación de información sobre configuraciones y características de IPsec en un sistema objetivo.

La mayoría de las VPN basadas en IPsec utilizan el Protocolo de administración de claves de la Asociación de seguridad de Internet (ISAKMP), una parte de IKE, para establecer, negociar, modificar y eliminar asociaciones de seguridad (SA) y claves criptográficas en un entorno VPN.

```bash
nmap -sU -p 500,4500 <IP objetivo>
```

Ike-scan. Escaneo y descubrimiento de configuraciones IKE.

```bash
ike-scan –M <target gateway IP address>
```

- Algoritmos de cifrado compatibles.
- Métodos de autenticación soportados.

## Enumeración VolIP

Usa SIP (Sesion Initation Protocol) permite llamadas por voz y video por IP.

SIP usa los puertos 2000, 2001, 5060 y 5061.

Mediante la enumeración de VoIP, los atacantes pueden recopilar información confidencial, como servidores y puertas de enlace de VoIP, sistemas de centralitas privadas (PBX) de IP y direcciones IP de agentes de usuario y extensiones de usuario de software de cliente (softphones) o teléfonos VoIP. Esta información se puede utilizar para lanzar diversos ataques de VoIP, como ataques de denegación de servicio (DoS), secuestro de sesión, suplantación de identidad de llamadas, escuchas ilegales, spam a través de telefonía por Internet (SPIT) y phishing de VoIP (Vishing).

Svmap

Herramienta utilizada en el contexto de pruebas de penetración, especialmente para el escaneo de puertos en redes. Es una herramienta escrita en Python que está basada en nmap y se utiliza para descubrir servicios de red a través de escaneos de puertos.

```bash
vmap <target network range/IP Addres
```

## Enumeración RPC

RPC (Remote Procedure Call, en español: Llamada a Procedimiento Remoto) es un mecanismo que permite la ejecución de procedimientos o funciones en un sistema remoto a través de una red. En lugar de invocar directamente una función en el proceso local, se realiza la llamada a través de la red, permitiendo la interacción con recursos remotos como si estuvieran en el sistema local.

En el ámbito informático y de redes, los sistemas que utilizan RPC pueden ejecutar operaciones en máquinas remotas a través de mensajes enviados a través de la red. Esto es posible gracias a que el servidor RPC expone una interfaz de funciones y procedimientos accesibles a través de una red.

Composición básica de RPC:
- Cliente RPC: Realiza la solicitud para invocar una función en el servidor RPC.
- Servidor RPC: Ejecuta la operación solicitada y devuelve el resultado al cliente.
- Protocolo de comunicación: RPC utiliza diferentes protocolos para la transmisión de mensajes (como TCP, UDP) para establecer la comunicación entre cliente y servidor.
- 
Un ejemplo clásico de uso de RPC es en sistemas operativos como Windows y Linux para el acceso a servicios y recursos remotos, tales como SMB (Server Message Block) o NFS (Network File System).

TCP/UDP 111

```bash
nmap -sR <target IP/network>
nmap -T4 –A <target IP/network>
```

## Enumeración usuarios UNIX/LINUX

En sistemas Unix y Linux, la enumeración de usuarios y la obtención de información sobre los usuarios conectados puede hacerse utilizando herramientas como rusers, rwho y finger. Estas herramientas pueden ser útiles en pruebas de penetración y auditorías de seguridad para recopilar información sobre las cuentas de usuario activas en un sistema remoto o local. A continuación, te explico cada una de estas herramientas:

`rusers` Muestra el nombre de usuario, la terminal, el tiempo de conexión y la máquina remota.
`rwho` Herramienta que muestra información sobre los usuarios conectados a la red. Funciona con el servicio rwho de RWHO (Remote Who). Es un método comúnmente utilizado para mostrar la lista de usuarios que han iniciado sesión en máquinas remotas dentro de una red.
`finger` Herramienta más detallada para obtener información sobre los usuarios de un sistema, tanto locales como remotos. Puede proporcionar detalles como el nombre completo, el directorio de inicio, la última vez que se conectó un usuario y otra información relevante.

## Enumeración usuarios Telnet y SMB

Telnet es un protocolo de red que se usa para conectarse a un dispositivo remoto a través de la red. Aunque Telnet no es seguro (ya que transmite datos en texto plano), aún se utiliza en redes internas y algunos servicios pueden estar expuestos a través de Telnet. Durante las pruebas de penetración, Telnet se puede utilizar para verificar puertos abiertos, interactuar con servicios o incluso para obtener información sobre los sistemas remotos.

`Port 23`

```bash
# nmap -p 23 --script telnet-ntlm-info <target I
```

SMB es un protocolo de red utilizado para compartir archivos, impresoras, y otros recursos entre máquinas en una red. SMB es muy común en sistemas Windows y también en Samba en sistemas Linux/Unix. Durante las pruebas de penetración, la enumeración de SMB puede revelar recursos compartidos, versiones de software, nombres de máquina, y más.

SMB se ejecuta directamente en el puerto TCP 445 o a través de la API NetBIOS en los puertos UDP 137 y 138 y los puertos TCP 137 y 139.

Los atacantes también pueden utilizar herramientas de enumeración SMB como Nmap, SMBMap, enum4linux, nullinux y NetScanTool Pro para realizar un escaneo dirigido en el servicio SMB que se ejecuta en el puerto 445.

Nmap tiene un script de SMB que se utiliza para realizar enumeración de SMB en máquinas remotas. Esto puede ayudar a obtener información sobre los recursos compartidos, los usuarios, las versiones de SMB y más.

```bash
# nmap -p 445 -A <target IP>
```

```bash
nmap --script smb-enum-shares,smb-enum-users <IP>

# nmap -p 445 –-script smb-protocols <Target IP>
# nmap -p 139 –-script smb-protocols <Target>
```

Enum4linux. Una herramienta que facilita la enumeración de SMB en sistemas Windows y Samba. Proporciona información sobre usuarios, grupos, políticas de contraseñas, recursos compartidos y más.



```bash
enum4linux -a <IP>
```

>-a muestra todos los detalles posibles

Información obtenida mediante SMB:
- Recursos compartidos: Archivos, impresoras y otros servicios expuestos a través de SMB.
- Usuarios: Enumeración de usuarios válidos del sistema.
- Grupos: Enumeración de los grupos a los que los usuarios pertenecen.
- Versiones de SMB: Información sobre la versión del protocolo SMB que está en uso.
- Información de NetBIOS: Nombres de equipo y dominio.

## Enumeración usuarios FTP y TFTP

FTP es un protocolo de transferencia de archivos que se utiliza para compartir archivos entre computadoras a través de la red. Aunque FTP no es seguro, ya que las credenciales y los datos se transmiten en texto plano, sigue siendo ampliamente utilizado.

Port 21

```bash
# nmap -p 21 <target domain>
```

També hi ha coses de metasploit:
```bash
use auxiliary/scanner/ftp/ftp_version
msf auxiliary(scanner/ftp/ftp_version) > set RHOSTS <target IP>
msf auxiliary(scanner/ftp/ftp_version) > exploi
```

Los datos se transfieren entre un remitente y un receptor en texto simple, exponiendo información crítica como nombres de usuario y contraseñas a los atacantes.

TFTP es un protocolo mucho más simple que FTP y se utiliza para la transferencia de archivos en redes locales, especialmente para la configuración de dispositivos como routers y switches. A diferencia de FTP, TFTP no tiene autenticación incorporada, lo que lo hace más vulnerable a la explotación.

TCP/UDP Port 69

PortQry

PortQry es una herramienta de diagnóstico de red utilizada para consultar el estado de puertos en máquinas remotas. Es particularmente útil en el ámbito de pruebas de penetración y auditorías de seguridad para verificar la accesibilidad de servicios a través de puertos específicos, detectar servicios vulnerables y mapear configuraciones de red.

```bash
> portqry -n <target domain/IP> -e 69 -p udp
```

NMAP

```bash
nmap -p 69 <target domain/IP>
```

## Enumeración usuarios IPv6


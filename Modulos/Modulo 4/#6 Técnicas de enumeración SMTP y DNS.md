# 6. Técnicas de enumeración SMTP y DNS

Redirección a [Información General](https://github.com/ThePenguin304/CEHv12-Notas/blob/main/Modulos/Modulo%204/README.md)

`SMTP (Simple Mail Transfer Protocol)` Protocolo estándar para envío de correos electrónicos en internet. Funciona en el puerto 25 (o el puerto 587 en conexiones autenticadas) y establece las reglas para enviar mensajes de correo de un servidor a otro.

`DNS (Network File System)` Sistema encargado de traducir nombres de dominio legibles por humanos (como ejemplo.com) en direcciones IP (como 192.168.1.1) que las computadoras pueden utilizar para comunicarse entre sí.

## 6.1 SMTP

Se usa com POP3 y IMAP, lo que permite guardar los correos y bajarlos cuando es necesario.

SMTP usa los servidores de exchange (MX) para enviar los correos hacia els DNS.

`Puertos TCP 25, 2525, 587`

Comandos de construcción:
* VRFY: Verifica los usuarios
* EXPN: Muestra los correos de correo y las listas de grupos de correo.
* RCPT TO: Define los receptores del correo electrónico.

Los atacantes pueden interactuar com el servidor STMP directamente con Telnet prompt y listas cuentas de correo válidas.

> Telnet es un protocolo que permite a los usuarios establecer una conexión remota con otro dispositivo en la red y trabajar en él como si estuvieran físicamente en la máquina.

### 6.1.1 Enumeración usando NMAP y METASPLOIT

### NMAP

Nmap presenta NSE (Scripts).

Aplica todos los NSE relacionados con smtp:

```bash
$nmap -p 25, 365, 587 -script=smtp-commands <Target IP Address >
```

Identifica los relay abiertos:
```bash
$nmap -p 25 -script=smtp-open-relay <Target IP Address>
```

Enumera todos los usuarios del servidor STMP:
```bash
$nmap -p 25 –script=smtp-enum-users <Target IP Address
```

### METASPLOIT

Enumera users del servidor SMTP usando un módulo predefinido, le carga una lista predefinida de usuarios al comando VRFY para per que le devuelven.

`auxiliary/scanner/smtp/smtp_enum`

La lista de usuarios usada se encuentra en:

`/usr/share/64etasploit-framework/data/wordlists/unix_users.txt`

### Otras herramientas

`NetScanTools Pro` testea el proceso de enviar un correo al servidor SMTP.

`smtp-user-enum` Herramienta para enumerar cuentas de usuario a nivel de sistema operativo en Solaris a través del servicio SMTP.

```bash
smtp-user-enum.pl [options] (-u username|-U file-of-usernames) (-t host|-T file-of-targets)
```

---

## DNS

### 1. DNS Zone Transfer
La transferencia de zona DNS es una técnica que algunos atacantes aprovechan para obtener información detallada sobre la infraestructura de una red de destino. Cuando un servidor DNS está mal configurado y permite transferencias de zona (por lo general, en el puerto TCP 53), un atacante puede solicitar una copia completa de la zona DNS, lo que le da acceso a datos sensibles del dominio.

En un entorno seguro, la transferencia de zona (o zone transfer) se utiliza para replicar registros DNS entre servidores autorizados dentro de la misma organización o red. Esto es esencial para la redundancia y disponibilidad de servicios DNS, ya que ayuda a sincronizar los registros entre los servidores primarios y secundarios.

Si este mecanismo no está adecuadamente restringido, cualquier persona puede solicitar la transferencia de zona y recibir una lista completa de los registros DNS del dominio. 

Esta lista puede contener:
* Nombres de servidores DNS.
* Nombres de host y máquinas: Identifica dispositivos específicos en la red.
* Nombres de usuario o alias: Puede proporcionar pistas sobre la estructura de los nombres de usuario.
* Direcciones IP: Permite conocer la IP de los hosts y servicios, facilitando futuras exploraciones.
* Alias (CNAME): Estos alias pueden revelar información sobre servicios o subdominios internos.

Para realizar una transferencia de zona DNS, el atacante envía una solicitud de transferencia de zona al servidor DNS haciéndose pasar por un cliente; el servidor DNS envía entonces una parte de su base de datos como zona al atacante. Esta zona puede contener una gran cantidad de información sobre la red de zonas DNS.

### 2. Dig Command

El comando dig (Domain Information Groper) es una herramienta de línea de comandos utilizada para realizar consultas DNS. Se usa principalmente en sistemas Unix/Linux, aunque también está disponible en Windows. dig permite obtener información sobre nombres de dominio, como direcciones IP, registros de servidor de correo (MX), registros de alias (CNAME), entre otros.

```bash
dig [opciones] [nombre_dominio] [tipo_de_registro]

dig ejemplo.com MX
dig alias.ejemplo.com CNAME
dig ejemplo.com ANY
```

Transfer zone activada funcionarà:
```bash
dig @servidor_dns ejemplo.com AXFR
```

### 3. nslookup Command

```bash
>nslookup
>set querytype=soa
><target domain>

/ls -d <domain of name server>
```

### 4. DNSRecon

Revisar todos los registros NS del objetivo para las zonas de transferencia

```bash
dnsrecon -t axfr -d <target domain>
```

### 5. DNS Cache Snooping

El atacante consulta al servidor DNS para obtener un registro DNS almacenado en caché específico.

En DNS Cache Snooping, el atacante envía una consulta al servidor DNS preguntando por un dominio específico, pero sin solicitar la resolución completa. En lugar de resolver el dominio, simplemente se pregunta al servidor si tiene ese dominio en caché:
* Si el servidor responde rápidamente con la dirección IP, significa que el dominio está en caché. Esto indica que alguien en la red ha solicitado recientemente ese dominio.
* Si el servidor toma más tiempo en responder o no tiene el dominio en caché, entonces es probable que ese dominio no haya sido solicitado recientemente.

Al observar qué dominios están en caché, un atacante podría deducir a qué sitios están accediendo los usuarios de una red. Esto podría revelar patrones de tráfico, servicios en uso o incluso aplicaciones específicas.

`root.hints file` fichero que contiene toda la informació sobre todos los servidores DNS roots.
Ejemplos de Información Obtenible

A través de DNS Cache Snooping, un atacante podría obtener:

- Patrones de tráfico: Detectar qué sitios o servicios están siendo utilizados con frecuencia.
- Presencia de servicios específicos: Identificar si se están utilizando servicios externos específicos o sitios populares, lo cual podría orientar ataques futuros.
- Información sobre la red: Esto puede revelar nombres de dominio internos y subdominios en uso, especialmente si los servidores DNS no están bien configurados.

Herramientas Utilizadas en DNS Cache Snooping
- dig: Una de las herramientas más utilizadas, con opciones como +norecurse.
- dnsrecon: Una herramienta de recolección de información DNS en Python que incluye técnicas de cache snooping.
- nmap: Aunque nmap es más amplio, en ciertos scripts permite obtener información de DNS que puede ser útil en análisis.

### Método no recursivo

Consulta no recursiva (consulta iterativa): Este es un método más directo y menos detectable. Aquí el atacante pregunta al servidor DNS sin solicitar la resolución completa, simplemente para saber si el dominio está en la caché. Para esto, se especifica que la consulta sea no recursiva utilizando la opción `+norecurse` con herramientas como `dig`:

```bash
dig +norecurse example.com @DNS_SERVER_IP
```

```bash
dig @<IP of DNS server> <Target domain> A +norecurse
```

 Si devuelve `status NOERROR` ha sido aceptada la petición pero no ha respondido nada.

### Método recursivo

Si el servidor permite consultas recursivas, el atacante puede preguntar por un dominio y observar si la respuesta es rápida o no. Las respuestas rápidas suelen indicar que el dominio ya está en caché, mientras que una respuesta más lenta indica que el servidor necesita consultar otros servidores para resolverlo.

Se especifica que la consulta sea no recursiva utilizando la opción `recurse`.

```bash
dig @<IP of DNS server> <Target domain> A +recurse
```

Si el TTL es muy elevado quiere decir que no está en la caché, ya que ha de ir a resolverlo.

## DNSSEC Zone Walking

Técnica utilizada para enumerar o descubrir todos los registros de un dominio protegido con DNSSEC (DNS Security Extensions). Esto sucede debido a una característica llamada NSEC (Next Secure), que DNSSEC usa para indicar que un dominio o subdominio específico no existe. Sin embargo, a través de esta respuesta de inexistencia, un atacante puede reconstruir la zona completa del dominio y obtener información sobre todos los registros que existen en esa zona, incluso aquellos que están protegidos por DNSSEC.

DNSSEC proporciona integridad y autenticidad de las respuestas DNS mediante la firma de registros DNS con claves criptográficas. Sin embargo, para que DNSSEC pueda responder a una consulta por un dominio inexistente de manera segura, usa registros NSEC o NSEC3 para demostrar la inexistencia de ese dominio:

- NSEC: Cuando se consulta un dominio que no existe, el servidor DNS devuelve un registro NSEC que indica cuál es el próximo dominio "seguro" en la zona.
- NSEC3: Introducido como una mejora de seguridad, utiliza un valor hash de los nombres de dominio para que la información no se lea tan fácilmente.
Con la implementación de NSEC, cualquier persona puede solicitar registros consecutivos, y al encadenar las respuestas, puede reconstruir la zona entera del dominio. Esto significa que un atacante puede listar nombres de host, direcciones IP y servicios internos o sensibles.

### HERRAMIENTAS

LDNS

LDNS-walk es una herramienta diseñada específicamente para realizar zone walking en servidores DNS protegidos por DNSSEC. Aprovecha la vulnerabilidad inherente a algunos registros DNSSEC, particularmente NSEC y, en menor medida, NSEC3, para intentar enumerar todos los nombres de dominio y subdominios dentro de una zona DNS.

DNSRecon

```bash
./dnsrecon -d <target domain> -z
```

NMAP

```bash
nmap --script=broadcast-dns-service-discovery <Target Domain>
```

```bash
nmap -T4 -p 53 --script dns-brute <Target Domain>
```

```bash
nmap -Pn -sU -p 53 --script=dns-recursion 192.168.1.150
```

DNS Security Extensions (DNSSEC) Enumeration Using Nmap

```bash
nmap -sU -p 53 --script dns-nsec-enum --script-args dns-nsec-enum.domains= eccouncil.org <target>
```

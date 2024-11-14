# 5. Técnicas de enumeración SMTP y DNS

Redirección a [Información General](https://github.com/ThePenguin304/CEHv12-Notas/blob/main/Modulos/Modulo%204/%230%20Info%20general.md)

`SMTP (Simple Mail Transfer Protocol)` Protocolo estándar para envío de correos electrónicos en internet. Funciona en el puerto 25 (o el puerto 587 en conexiones autenticadas) y establece las reglas para enviar mensajes de correo de un servidor a otro.

`DNS (Network File System)` Sistema encargado de traducir nombres de dominio legibles por humanos (como ejemplo.com) en direcciones IP (como 192.168.1.1) que las computadoras pueden utilizar para comunicarse entre sí.

## 5.1 SMTP

Se usa com POP3 y IMAP, lo que permite guardar los correos y bajarlos cuando es necesario.

SMTP usa los servidores de exchange (MX) para enviar los correos hacia els DNS.

`Puertos TCP 25, 2525, 587`

Comandos de construcción:
* VRFY: Verifica los usuarios
* EXPN: Muestra los correos de correo y las listas de grupos de correo.
* RCPT TO: Define los receptores del correo electrónico.

Los atacantes pueden interactuar com el servidor STMP directamente con Telnet prompt y listas cuentas de correo válidas.

> Telnet es un protocolo que permite a los usuarios establecer una conexión remota con otro dispositivo en la red y trabajar en él como si estuvieran físicamente en la máquina.

### 5.1.1 Enumeración usando NMAP y METASPLOIT

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

### Método no recursivo



### Método recursivo

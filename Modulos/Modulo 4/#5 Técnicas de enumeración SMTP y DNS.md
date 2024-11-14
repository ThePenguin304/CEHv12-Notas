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




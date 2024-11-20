# 6. Gaining Access

Demostrar diferentes técnicas de descifrado de contraseñas y explotación de vulnerabilidades para obtener acceso al sistema.

En el módulo 1 se hablo de CHM (CEH Hacking Methodology), dónde habia pasos de hackeo de sistema. En estos, ganar acceso al sistema es el primer paso en un hackeo de sistema.

  - Ejemplo de técnicas: Crackeo de contraseñas, Buffer Overflows y explotación de vulnerabilidades identificadas.

## 6.1 Cracking Passwords

## 6.1.1 Microsfot Authentication

Windows OS autentica los usuarios con tres mecanismos (protocolos):

### SAM (Security Accounts Manager)

- Windows usa la base de datos de SAM o AD para gestionar cuentas y contraseñas de usuario en formato hash (one way hash).

- El los resgistros del sistema se encuentra la base de datos SAM y el Kernel de Windows tiene privilegios para acceder. Se almacenan con seguridad filesystem lock, que no deja copiarlos ni moverlos de ubicación hasta que el sistema de apaga. Ahí es dónde se producen los ataques de fuerza bruta de forma offline. 

- El fichero SAM se encripta con la función SYSKEY para encriptar los hashes. Se encuentra en `%SystemRoot%/system32/config/SAM` y en los registros de sistema `HKLM/SAM registry hive`. Guarda las contraseñas hasheadas de LM y NTLM.

- NTLM remplaza LM hash, lo que es sucectible de ataques de cracking. Pero a partir de Windows Vista LM está desactivado por defecto. Está blank.

- Se aumentó la seguridad con NTLM.

---

### NTLM (NET LAN Manager)

- Autenticación por defecto por sistema de reto/respuesta. NTLM protocolo de autenticación y LM (LAN Manager). Usan diferentes metodologías hash para almacenar contraseás en el fichero SAM.

- NTLMv1, NTLMv2 aumentan la seguridad de encriptación.

- En la autenticación NTLM se realiza mediante Microsoft-negociated Security Support Provider (SSP).

- En Domain Controller envia "nonce" que el cliente ha de encriptar y comparada la encriptación para asegurar que es correcto.

- La seguridad evolucionó con Kerberos.

---

### Kerberos Authentication

- Kerberos es un protocolo de autenticación en red que utiliza criptografía de clave simétrica y un modelo de tickets para permitir la autenticación segura entre clientes y servidores. Es común en redes como Active Directory. Los mensajes protegen de replica o eaverdropping.

- Evita transmitir contraseñas directamente y usa criptografía simétrica (ej., AES) y claves secretas compartidas. Esto permite seguridad contra ataques de escucha y reducción de contraseñas en la red. La desventaja principal que tiene es la dependencia del tiempo; requiere sincronización precisa.

- KDC (Key Distribution Center), tecera parte segura, que se usa con AS (Authentication Server) y TGS (Ticket Granting Server). Con este ticket kerberos autentifica a los usuarios.

Proceso de autenticación en Kerberos:
- Inicio de sesión (AS-REQ/AS-REP):
    - El cliente solicita un Ticket Granting Ticket (TGT) al Authentication Server (AS).
    - Si las credenciales son válidas, el AS emite el TGT, cifrado con la clave del cliente.
- Solicitar acceso a servicios (TGS-REQ/TGS-REP):
    - El cliente usa el TGT para pedir un ticket específico al Ticket Granting Server (TGS).
    - El TGS emite un ticket de servicio (ST), que incluye información cifrada para el servidor.
- Acceso al servicio:
    - El cliente presenta el ST al servidor objetivo para autenticarse y usar el servicio.

(Single Sing-On) Inicio de sesión único mediante el cual el usuario no necesita volver a introducir la contraseña para acceder a los servicios autorizados. Cabe destacar que no existe comunicación directa entre los servidores de aplicaciones y el KDC; los tickets de servicio, incluso si están empaquetados por TGS, llegan al servicio únicamente a través del cliente que desea acceder a ellos.

---

### 6.1.2 Password Cracking

1. Recuperar contraseñas de los sistemas.
2. Acceder a sistemas vulnerados.
3. La mayoría de técnicas de crakeo suceden porque las contraseas son fáciles y por defecto.

Ejemplos: Ataques de fuerza bruto o de diccionario

### 1. Ataques no electrónicos
Los ataques no electrónicos son técnicas que no requieren herramientas tecnológicas avanzadas, sino que explotan el comportamiento humano o el acceso físico para comprometer la seguridad.
  - Social Engeenering:
    - Manipulación psicológica para obtener información confidencial (contraseñas, datos personales, etc.) a través de engaños, como llamadas telefónicas, correos falsos o interacción directa.
    - Protección: Capacitación y concienciación del personal, verificación estricta de identidades.
      
  - Shoulder surfing:
    - Observación directa para obtener información sensible, como contraseñas, PINs o datos en pantalla, mientras alguien los introduce.
    - Protección: Uso de protectores de pantalla de privacidad y cuidado en lugares públicos.
      
  - Dumpster Diving:
    - Búsqueda de información valiosa en documentos desechados, como facturas, notas, registros o medios de almacenamiento, que podrían haber sido eliminados de manera inapropiada.
    - Protección: Destrucción segura de documentos mediante trituradoras y eliminación adecuada de medios digitales.


### 2. Online activos 
Los ataques online activos implican interacción directa con el sistema objetivo para explotar vulnerabilidades o forzar credenciales. Requieren acceso a la máquina víctima y suelen generar tráfico que puede ser detectado.
  - Ataques de Diccionario:
    - Utilizan listas predefinidas de contraseñas comunes para intentar autenticarse.
      
  - Fuerza Bruta:
    - Prueba sistemática de todas las combinaciones posibles de contraseñas hasta encontrar la correcta.
      
  - Ataques Basados en Reglas:
    - Personalizan ataques de fuerza bruta o diccionario aplicando transformaciones, como añadir números o caracteres especiales.
      
  - Ataque de Hash Injection:
    - Inyección de un hash robado directamente en un proceso para obtener acceso sin conocer la contraseña original.
      
  - Envenenamiento de LLMNR/NBT-NS:
    - Explota protocolos de resolución de nombres en redes locales para interceptar credenciales.
      
  - Troyanos/Spyware/Keyloggers:
    - Instalación de malware para capturar contraseñas o datos directamente desde la máquina víctima.
      
  - Password Guessing/Spraying:
    - Prueba de contraseñas comunes contra múltiples cuentas para evitar bloqueos por intentos fallidos.
    - Herramienta: `CrackMapExec`.
      
  - Ataque de Monólogo Interno (Internal Monologue):
    - Abusa del proceso interno de validación de contraseñas en sistemas Windows para obtener hashes de NTLM sin interacciones de red.
      
  - Cracking de Contraseñas de Kerberos:
    - AS-REP Roasting es un ataque contra cuentas de Active Directory que tienen la opción "Do not require Kerberos preauthentication" habilitada. Este ataque explota el hecho de que, sin preautenticación, el KDC (Key Distribution Center) entrega un Ticket Granting Ticket (TGT) cifrado directamente con la clave de la contraseña del usuario, lo que permite al atacante realizar un ataque offline para descifrar la contraseña. Heraramientas: `hashcat` o `John the Ripper`.
    - Kerberoasting es un ataque contra el servicio de Kerberos donde el atacante solicita un Ticket Granting Service (TGS) para servicios asociados a cuentas en Active Directory. Este ticket está cifrado con la clave de la contraseña de la cuenta del servicio, lo que permite realizar un ataque offline para descifrar la contraseña. Herramientas: `Mimikatz`, `Impacket (GetUserSPNs.py)`, `Rubeus` o `hashcat`/`John the Ripper` para descifrar el hash.
    - Pass-The-Ticket (PTT): Pass-the-Ticket es un ataque donde el atacante utiliza un Ticket Granting Ticket (TGT) o un Ticket Granting Service (TGS) válido y previamente robado para acceder a recursos en un entorno Kerberos, sin necesidad de conocer las credenciales originales del usuario o servicio.
      
  - Medidas de Protección:
    - Implementar autenticación multifactor (MFA).
    - Monitorizar y analizar el tráfico en busca de comportamientos anómalos.
    - Configurar políticas de bloqueo por intentos fallidos.
    - Usar contraseñas complejas y únicas.
    - Deshabilitar protocolos no utilizados como LLMNR o NetBIOS.

### 3. Online pasivos 
Los ataques online pasivos se enfocan en interceptar y analizar datos sin alterar el tráfico o los sistemas. Su objetivo principal es capturar información sensible como contraseñas, hashes o datos confidenciales.
  -  Wire Sniffing:
    - Captura de tráfico de red utilizando herramientas como Wireshark o tcpdump para analizar paquetes en busca de credenciales u otros datos sensibles que puedan transmitirse en texto plano.
  
  -  MITM
    - El atacante se posiciona entre el cliente y el servidor para interceptar, registrar y potencialmente modificar la comunicación. Ejemplos: ataques ARP spoofing o en redes Wi-Fi abiertas.
  
  -  Replay Attack
    - Consiste en capturar un paquete o sesión válida y reproducirla posteriormente para obtener acceso sin necesidad de las credenciales originales. Ejemplo: reutilización de tokens o cookies.

  - Medidas de protección:
    - Cifrado de tráfico: Implementar protocolos como HTTPS, SSL/TLS y VPN para proteger los datos en tránsito.
    - Autenticación robusta: Usar autenticación basada en tokens o con marcas temporales (timestamp) para mitigar ataques de repetición.
    - Seguridad en redes: Configurar firewalls, segmentar redes y deshabilitar protocolos inseguros.
    - Detección y monitoreo: Implementar sistemas IDS/IPS para identificar actividades sospechosas en la red.
  
### 4. Ataques offline 
Los ataques offline se producen cuando el atacante obtiene una copia de los hashes de contraseñas desde el sistema objetivo y los procesa fuera de este, utilizando sus propios recursos para descifrar las contraseñas sin alertar a la víctima.
  - Rainbow Table Attack (Pre-Computed Hashes):
    - Utiliza tablas precomputadas que contienen pares de contraseñas y sus hashes. El atacante compara los hashes obtenidos con los de la tabla para descifrar rápidamente las contraseñas.
    - Protección: Usar salting (agregar un valor aleatorio único a cada contraseña antes de calcular su hash) para invalidar las tablas.
      
  - Distributed Network Attack.
    - Divide el proceso de descifrado en múltiples sistemas distribuidos, utilizando la potencia combinada de varias máquinas para acelerar el crackeo. Es eficaz para ataques de fuerza bruta en grandes volúmenes de datos.
    - Implementar políticas de contraseñas fuertes y limitar el número de intentos de inicio de sesión.
      
  - Medidas de Protección:
    - Hashing seguro: Utilizar algoritmos robustos como bcrypt, Argon2 o PBKDF2 con sal.
    - Almacenamiento seguro: Proteger los archivos de contraseñas con permisos restrictivos y cifrado.
    - Supervisión de accesos: Monitorizar intentos de acceso no autorizados o descargas sospechosas de ficheros.

### Herramientas online para encontrar contraseñas por defecto
- https://www.fortypoundhead.com
- https://cirt.net 
- http://www.defaultpassword.us 
- https://www.routerpasswords.com 
- https://default-password.info 
- https://192-168-1-1ip.mobi

 

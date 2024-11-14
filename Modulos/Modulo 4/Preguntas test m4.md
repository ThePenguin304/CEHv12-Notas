# Preguntas Test Módulo 4

Redirección a [Información General](https://github.com/ThePenguin304/CEHv12-Notas/blob/main/Modulos/Modulo%204/%230%20Info%20general.md)

1. **¿Qué herramienta se utiliza comúnmente para realizar la enumeración de servicios SMB en una red?**
    
   a) Nmap  
   b) Netcat  
   c) Enum4linux  
   d) Nessus  
   **Respuesta:** c) Enum4linux

2. **¿Cuál es el objetivo principal de la enumeración en un ataque de red?**

   a) Acceder a los sistemas de la red  
   b) Obtener información sobre usuarios, servicios y recursos de la red  
   c) Realizar un escaneo de puertos  
   d) Enviar tráfico malicioso para desbordar los sistemas  
   **Respuesta:** b) Obtener información sobre usuarios, servicios y recursos de la red

4. **¿Qué tipo de información se obtiene generalmente durante la enumeración de LDAP?**
   
   a) Servicios DNS disponibles  
   b) Información sobre los usuarios y grupos de una red  
   c) Información sobre las vulnerabilidades del sistema  
   d) Puertos abiertos en el sistema  
   **Respuesta:** b) Información sobre los usuarios y grupos de una red

6. **¿Qué protocolo se utiliza para la enumeración de usuarios en un entorno Windows a través de NetBIOS?**
   
   a) FTP  
   b) SMB  
   c) HTTP  
   d) DNS  
   **Respuesta:** b) SMB

8. **¿Qué comando de Netcat se usa para establecer una conexión remota con un puerto específico en un objetivo?**
   
   a) nc -v -l  
   b) nc -v <IP> <puerto>  
   c) nc -l -p <puerto>  
   d) nc -z <IP>  
   **Respuesta:** b) nc -v <IP> <puerto>

10. **¿Qué herramienta permite realizar la enumeración de grupos y usuarios en un sistema Windows remoto?**
    
   a) Nmap  
   b) Netstat  
   c) Net user  
   d) Enum4linux  
   **Respuesta:** d) Enum4linux

12. **¿Qué tipo de ataques pueden realizarse después de obtener información de la enumeración SMB?**
    
   a) Ataques de denegación de servicio  
   b) Ataques de man-in-the-middle  
   c) Ataques de fuerza bruta sobre contraseñas  
   d) Ataques de inyección SQL  
   **Respuesta:** c) Ataques de fuerza bruta sobre contraseñas

14. **¿Qué herramienta permite la enumeración de la configuración de un servidor DNS?**
    
   a) Nslookup  
   b) Netstat  
   c) Netcat  
   d) Nmap  
   **Respuesta:** a) Nslookup

16. **¿Cuál de los siguientes servicios es más comúnmente objetivo en la enumeración de puertos 139 y 445?**
    
   a) SMTP  
   b) FTP  
   c) SMB  
   d) HTTP  
   **Respuesta:** c) SMB

18. **¿Cuál es el comando en Linux para realizar la enumeración de recursos compartidos en una máquina remota a través de SMB?**
    
   a) smbclient -L <IP>  
   b) nmap -sS <IP>  
   c) nc -v <IP>  
   d) netstat -an  
   **Respuesta:** a) smbclient -L <IP>

20. **¿Qué es una "zona de búsqueda inversa" en la enumeración DNS?**
    
   a) El proceso de escanear la red para encontrar hosts  
   b) Una consulta para resolver direcciones IP a nombres de dominio  
   c) El proceso de escanear puertos abiertos en un servidor  
   d) Un ataque para modificar registros DNS  
   **Respuesta:** b) Una consulta para resolver direcciones IP a nombres de dominio

22. **¿Qué comando de Nmap se usa para realizar un escaneo de la versión de los servicios en un host?**
    
   a) nmap -sT  
   b) nmap -sV  
   c) nmap -O  
   d) nmap -p <puertos>  
   **Respuesta:** b) nmap -sV

24. **¿Qué servicio se puede enumerar con el protocolo SNMP?**
    
   a) Puertos abiertos en un servidor  
   b) Información sobre los sistemas y dispositivos en la red  
   c) Los usuarios y grupos de una red  
   d) La configuración de un servidor FTP  
   **Respuesta:** b) Información sobre los sistemas y dispositivos en la red

26. **¿Qué tipo de ataques se pueden realizar después de obtener información sobre un servidor mediante la enumeración de SNMP?**
    
   a) Ataques DoS  
   b) Ataques de suplantación de identidad  
   c) Ataques de elevación de privilegios  
   d) Ataques basados en la configuración obtenida del servidor  
   **Respuesta:** d) Ataques basados en la configuración obtenida del servidor

28. **¿Qué comando de Enum4linux se usa para obtener la información sobre los grupos en un sistema Windows?**
    
   a) enum4linux -a <IP>  
   b) enum4linux -G <IP>  
   c) enum4linux -S <IP>  
   d) enum4linux -U <IP>  
   **Respuesta:** b) enum4linux -G <IP>

30. **¿Qué técnica de enumeración puede utilizarse para identificar los servicios que están ejecutándose en un puerto abierto?**
    
   a) Escaneo de huella digital (Fingerprinting)  
   b) Escaneo de servicios  
   c) Escaneo de vulnerabilidades  
   d) Escaneo de contraseñas  
   **Respuesta:** b) Escaneo de servicios

32. **¿Qué herramienta se utiliza para enumerar usuarios en un servidor basado en Linux?**
    
   a) Ldapsearch  
   b) Enum4linux  
   c) Getmac  
   d) Netstat  
   **Respuesta:** a) Ldapsearch

34. **¿Qué tipo de información se puede obtener de una enumeración de un servidor HTTP?**
    
   a) Configuración de DNS  
   b) Puertos abiertos  
   c) Información sobre los directorios y archivos  
   d) Información sobre usuarios y contraseñas  
   **Respuesta:** c) Información sobre los directorios y archivos

36. **¿Qué comando de Nmap se usa para enumerar los servicios que están ejecutándose en un puerto específico?**
    
   a) nmap -sT  
   b) nmap -sS  
   c) nmap -sV  
   d) nmap -sP  
   **Respuesta:** c) nmap -sV

38. **¿Cuál es el objetivo de la enumeración de servicios en una red?**

   a) Identificar la arquitectura de red  
   b) Obtener información sobre los servicios activos y sus configuraciones  
   c) Evitar posibles ataques a la red  
   d) Asegurar que no existan puertos abiertos  
   **Respuesta:** b) Obtener información sobre los servicios activos y sus configuraciones


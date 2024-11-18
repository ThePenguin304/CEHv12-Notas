# Resumir los conceptos de evaluación de vulnerabilidades

Información como un factor clave. Escaneos de vulnerabilidades informan sobre vulnerabilidades.

Para proteger una red, un administrador debe realizar la gestión de parches, instalar el software antivirus adecuado, verificar las configuraciones, resolver problemas conocidos en aplicaciones de terceros y solucionar problemas de hardware con configuraciones predeterminadas. Todas estas actividades juntas constituyen una evaluación de vulnerabilidad.

## Qué es una vulnerabilidad?

Una vulnerabilidad es una debilidad o fallo en un sistema, aplicación, red o protocolo que puede ser explotado por un atacante para comprometer la confidencialidad, integridad o disponibilidad de los datos o recursos.

Razones comunes detrás de una vulnerabilidad:
- Errores de diseño o desarrollo: Código inseguro o mal escrito, Falta de validación de entradas del usuario o Errores en la lógica del programa.
- Falta de gestión de parches: No actualizar regularmente los sistemas o aplicaciones para abordar vulnerabilidades conocidas.
- Configuraciones incorrectas: Configuración predeterminada de dispositivos y aplicaciones (por ejemplo, credenciales por defecto) o Servicios innecesarios habilitados.
- Software desactualizado o no soportado: Uso de sistemas operativos o aplicaciones que ya no reciben actualizaciones de seguridad.
- Mala implementación de protocolos de seguridad: Protocolos obsoletos como SSL 3.0 o TLS 1.0, Uso de claves criptográficas débiles.
- Errores humanos: Contraseñas débiles o prácticas de autenticación inseguras o Falla en el monitoreo y análisis de los sistemas.
- Amenazas internas: Empleados maliciosos o negligentes que explotan fallos internos.
- Interacción con aplicaciones de terceros: Dependencia de software externo vulnerable.
- Falta de concienciación en seguridad: Usuarios que caen en ataques de ingeniería social como phishing.

## Ejemplos de vulnerabilidades agrupadas por categoría:
- Vulnerabilidades del protocolo TCP/IP
> IP Spoofing: Suplantación de direcciones IP para engañar al sistema objetivo.

> SYN Flood Attack: Explotación del proceso de handshake de TCP para agotar recursos del servidor.

> ICMP Flooding: Uso masivo de paquetes ICMP para sobrecargar un dispositivo.

> Fragmentation Attacks: Envío de fragmentos de paquetes maliciosos que el sistema no puede ensamblar.
- Vulnerabilidades del sistema operativo (OS Vulnerabilities)
> Fallas en la validación de permisos: Permitir a usuarios no autorizados acceder a recursos restringidos.

> No aplicar actualizaciones de seguridad: Explotación de vulnerabilidades conocidas, como EternalBlue en Windows.

> Configuraciones de seguridad predeterminadas: Permitir servicios o puertos innecesarios abiertos.

> Errores en el kernel: Bugs que permiten elevación de privilegios o ejecución de código arbitrario.
- Vulnerabilidades de dispositivos de red
> Firmware desactualizado: Exposición a exploits conocidos en routers o switches.

> SNMP (Simple Network Management Protocol) mal configurado: Permitir acceso sin autenticación.

>Exceso de servicios habilitados: Telnet o servicios de administración remota abiertos sin protección.

> Default credentials: Uso de contraseñas predeterminadas como "admin/admin".
- Vulnerabilidades en cuentas de usuario
> Contraseñas débiles: Uso de contraseñas fáciles de adivinar como "123456".

> Falta de políticas de bloqueo de cuenta: Permitir intentos ilimitados de inicio de sesión (ataques de fuerza bruta).

> Privilegios excesivos: Asignación de privilegios administrativos a cuentas de usuario regulares.
- Vulnerabilidades en cuentas del sistema (System Account Vulnerabilities)
> Cuentas no utilizadas activas: Dejar habilitadas cuentas de sistema que no se usan.

> Acceso no restringido a cuentas críticas: Ejemplo, "root" o "administrator" sin doble factor de autenticación.

> Cuentas predeterminadas activas: Uso de cuentas preinstaladas sin cambio de configuraciones.
- Errores de configuración en servicios de Internet
> Directorios abiertos en servidores web: Permitir acceso directo a archivos sensibles.

> Configuraciones débiles en servidores de correo: Permitir el envío de correos no autenticados (open relays).

> Uso de protocolos inseguros: Servicios que aún permiten HTTP en lugar de HTTPS o FTP en lugar de SFTP.

> Exposición de información en banners: Servidores que revelan versión o configuración en headers de respuesta.
- Contraseñas y configuraciones predeterminadas
> Uso de contraseñas predeterminadas: Contraseñas como "admin" o "1234" no cambiadas tras la instalación inicial.

> Configuración predeterminada de seguridad: Firewalls o sistemas IDS/IPS deshabilitados por defecto.

> Cuentas predeterminadas habilitadas: Usuarios como "guest" o "test" activas en el sistema.
- Errores de configuración en dispositivos de red
> ACLs (Listas de Control de Acceso) mal configuradas: Permitir tráfico no deseado.

> Puertos innecesarios abiertos: Por ejemplo, permitir acceso Telnet en lugar de SSH.

> Falta de segmentación de red: No separar adecuadamente redes críticas de las públicas.

> Logs desactivados: No registrar eventos críticos en el dispositivo para análisis futuro.

## Investigación de vulnerabidad

Proceso de investigar protocolos, servicios y configuraciones pare descubrir las vulnerabilidades de los OS y acplicaciones que están expuestas a vulnerabilidades. Se ha de estar al día de las nuevas vulnerabilidades que salen, sobretodo de día zero.

Los expertos clasifican las vulnerabilidades por:
- Severidad (Baja, Media o Alta).
- Rango de explotación (Remoto o local).

## Fuentes de vulnerabilidades

- MSRC (Microsoft Security Response Center). Todas las vulnerabilidades que afectan a los productos de microsoft. 
- Packer Storm
- Dark Reading
- Trend Micro
- Security Magazine
- PenTest Magazine

 ## Que es una evaluación de vulnerabilidades?

Capacidad del sistema o aplicación a resistir a una explotación. Se clasifican vulnerabilidades en un sistema, red o canales de comunicación.

Se puede realizar para ver las debilidades que pueden explotarse o si las aplicaciones de seguridad aplicadas han sido efectivas.

Información recogida:
- Vulnerabilidades de red
- Puertos abiertos y servicios
- Vulnerabilidades de los puertos y los servicios
- Configuraciones de los errores en aplicaciones y servicios

El software escanea los equipos contra CVE (Common Vulnerability and Exposoures).

El escaneo puede ser `activo` o `pasivo` (sin interactuar directamente).

Algunas herramientas: Nessus Profesional, Qualys, GFI LanGuard o OpenVas.

Limitaciones:
- Capacidad limitada en un tiempo específico.
- Requiere estar actualizado el software que lanza los escaneos.
- No mida la fuerza de los controles de seguridad.
- Complejidad de los informes reportados.

## Sistemas de puntuación de vulnerabilidades y bases de datos

Para gestionar y priorizar vulnerabilidades de manera efectiva, los profesionales de la ciberseguridad utilizan estándares y bases de datos reconocidos a nivel global. A continuación, se describen los sistemas más comunes:

### CVSS (Common Vulnerability Scoring System)

¿Qué es?
Un estándar para puntuar la severidad de las vulnerabilidades.

Escala: 0.0 a 10.0 (0 = sin impacto, 0.1 - 3.9 = Bajo, 4-0 - 6.9 = Medio, 7.0 - 8.9 = Alto, 10 = impacto crítico).
Ayuda a determinar la urgencia de aplicar parches o medidas de mitigación.
Componentes de puntuación:

Base Score: Evalúa características inherentes a la vulnerabilidad (exploitabilidad e impacto).
Temporal Score: Considera factores temporales, como la disponibilidad de exploits o parches.
Environmental Score: Adapta la puntuación al entorno específico de la organización afectada.
Ejemplo: Una vulnerabilidad con CVSS de 9.8 es crítica y requiere atención inmediata.

### CVE(Common Vulnerabilities and Exposures)
¿Qué es?
Un identificador único para vulnerabilidades conocidas, mantenido por MITRE.

Formato: CVE-año-número (Ej.: CVE-2024-12345).
Actúa como un identificador universal para que los equipos técnicos puedan referirse a vulnerabilidades de manera consistente.
Beneficios:

Facilita la comunicación entre equipos.
Vincula herramientas y bases de datos (por ejemplo, NVD).
Ejemplo: CVE-2024-12345 puede ser una vulnerabilidad en un servidor web popular.

### NVD(National Vulnerability Database)
¿Qué es?
Una base de datos mantenida por el NIST (National Institute of Standards and Technology) en EE.UU.

Incluye detalles de vulnerabilidades referenciadas por CVE.
Proporciona puntuaciones CVSS, descripciones y métricas adicionales.
Usos:

Buscar información técnica detallada.
Evaluar y priorizar vulnerabilidades en un entorno específico.
Ejemplo: Al buscar un CVE en NVD, se obtienen datos como el CVSS, soluciones disponibles y posibles mitigaciones.

### CWE(Common Weakness Enumeration)
¿Qué es?
Un catálogo de fallos de seguridad comunes en el software y diseño del sistema.

Mantenido también por MITRE.
Se enfoca en las causas subyacentes de las vulnerabilidades.
Clasificación de debilidades:

Ejemplo: CWE-79 (Cross-Site Scripting), CWE-89 (SQL Injection).
Relación con CVE:

Cada vulnerabilidad referenciada en CVE puede tener una o más debilidades CWE asociadas, lo que ayuda a los desarrolladores a entender el problema raíz.

Relación entre CVSS, CVE, NVD y CWE:
CVE: Identifica la vulnerabilidad.
NVD: Proporciona detalles técnicos y puntuación CVSS.
CVSS: Ayuda a priorizar según la gravedad.
CWE: Describe la causa raíz o el tipo de debilidad.
Estos sistemas y bases de datos son esenciales para evaluar riesgos, gestionar vulnerabilidades y proteger los sistemas de manera proactiva.









 



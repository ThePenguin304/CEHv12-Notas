# Evaluación de Vulnerabilidades

Redireccionar al [módulo 5](https://github.com/ThePenguin304/CEHv12-Notas/tree/main/Modulos/Modulo%205)

La **evaluación de vulnerabilidades** es un proceso esencial para identificar, clasificar y mitigar las debilidades en sistemas, redes y aplicaciones, asegurando la confidencialidad, integridad y disponibilidad de los recursos. 

## ¿Qué es una vulnerabilidad?

Una vulnerabilidad es una debilidad o fallo en un sistema, aplicación, red o protocolo que puede ser explotado por un atacante. Estas fallas comprometen la seguridad y pueden surgir por diversas razones:

### Razones comunes detrás de una vulnerabilidad:
- **Errores de diseño o desarrollo**: Código inseguro, falta de validación de entradas o errores lógicos.
- **Falta de gestión de parches**: No aplicar actualizaciones de seguridad regularmente.
- **Configuraciones incorrectas**: Uso de credenciales predeterminadas o habilitación de servicios innecesarios.
- **Software obsoleto**: Sistemas operativos y aplicaciones sin soporte.
- **Implementación débil de protocolos de seguridad**: Uso de TLS 1.0 o claves criptográficas débiles.
- **Errores humanos**: Contraseñas débiles, malas prácticas de autenticación y falta de monitoreo.
- **Amenazas internas**: Empleados maliciosos o negligentes.
- **Dependencia de aplicaciones de terceros**: Uso de software externo vulnerable.
- **Falta de concienciación**: Usuarios susceptibles a ataques de ingeniería social como phishing.

---

## Ejemplos de vulnerabilidades comunes

### Vulnerabilidades del protocolo TCP/IP
- **IP Spoofing**: Suplantación de direcciones IP para engañar a sistemas.
- **SYN Flood Attack**: Explotación del handshake TCP para agotar recursos del servidor.
- **ICMP Flooding**: Sobrecarga de dispositivos con paquetes ICMP.
- **Fragmentation Attacks**: Paquetes maliciosos que no pueden ensamblarse correctamente.

### Vulnerabilidades del sistema operativo (OS)
- Fallos en la validación de permisos.
- Configuraciones de seguridad predeterminadas.
- Bugs en el kernel que permiten escalamiento de privilegios.

### Vulnerabilidades de dispositivos de red
- Firmware desactualizado.
- SNMP mal configurado (sin autenticación).
- Uso de credenciales predeterminadas.

### Vulnerabilidades en cuentas
- **Cuentas de usuario**: Contraseñas débiles y privilegios excesivos.
- **Cuentas del sistema**: Cuentas predeterminadas activas, como "admin".

### Errores de configuración en servicios de Internet
- Directorios abiertos en servidores web.
- Uso de HTTP en lugar de HTTPS.
- Configuraciones de servidores de correo como open relays.

### Contraseñas y configuraciones predeterminadas
- Uso de credenciales predeterminadas como "admin/admin".
- Firewalls deshabilitados por defecto.

---

## Fuentes de información sobre vulnerabilidades

- **Microsoft Security Response Center (MSRC)**: Vulnerabilidades en productos de Microsoft.
- **Packet Storm**, **Trend Micro**, **Dark Reading**, **PenTest Magazine**.
- **NVD**: Base de datos oficial del NIST.

---

## Evaluación de vulnerabilidades

### ¿Qué es?
La evaluación de vulnerabilidades determina la capacidad de un sistema o aplicación para resistir intentos de explotación, identificando debilidades en:
- **Redes**: Puertos abiertos y servicios inseguros.
- **Aplicaciones**: Errores en configuraciones y servicios.

### Métodos
- **Escaneo activo**: Interactúa directamente con el sistema.
- **Escaneo pasivo**: Observa sin interactuar directamente.

### Herramientas populares
- **Nessus Professional**
- **Qualys**
- **OpenVAS**
- **GFI LanGuard**

### Limitaciones
- Requiere herramientas actualizadas.
- No mide la eficacia de controles de seguridad.
- Informes técnicos pueden ser complejos.

---

## Ciclo de vida de una vulnerabilidad

### 1. Fase pre-evaluatoria
- **Identificación de activos**: Clasificar recursos críticos.
- **Definir líneas de acción**: Crear políticas para gestionar vulnerabilidades.
- **Asignar responsabilidades**: Delegar roles clave.

### 2. Escaneo de vulnerabilidades
- Uso de herramientas para detectar debilidades.
- Generación de reportes con detalles sobre las vulnerabilidades.

### 3. Post evaluación
- **Evaluación de riesgos**: Priorización según severidad.
- **Remediación**: Aplicar parches y ajustes de configuración.
- **Verificación**: Confirmar que las vulnerabilidades fueron resueltas.
- **Monitoreo continuo**: Detectar nuevas amenazas y evaluar políticas.

---

## Sistemas de puntuación y bases de datos de vulnerabilidades

### CVSS (Common Vulnerability Scoring System)
- **Escala**: 0.0 a 10.0.
- **Componentes**:
  - Base Score: Gravedad inherente.
  - Temporal Score: Disponibilidad de exploits o parches.
  - Environmental Score: Impacto en el entorno específico.

### CVE (Common Vulnerabilities and Exposures)
- Identificador único para vulnerabilidades.
- Formato: `CVE-AÑO-NÚMERO`.

### NVD (National Vulnerability Database)
- Base de datos del NIST.
- Proporciona detalles técnicos y métricas CVSS.

### CWE (Common Weakness Enumeration)
- Catálogo de fallos comunes.
- Ejemplos: CWE-79 (XSS), CWE-89 (SQL Injection).

---

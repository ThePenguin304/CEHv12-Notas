# Resumen: Escalada de Privilegios

## ¿Qué es la escalada de privilegios?
La escalada de privilegios es una técnica en ciberseguridad mediante la cual un atacante eleva su nivel de acceso dentro de un sistema. Su objetivo es explotar vulnerabilidades o configuraciones incorrectas para obtener permisos superiores y controlar más recursos de un sistema.  
### Tipos de escalada de privilegios:
1. **Vertical**: Pasar de un usuario estándar a un administrador o root.
2. **Horizontal**: Acceder a cuentas de otros usuarios con los mismos privilegios.

---

## Técnicas de escalada de privilegios

### 1. **DLL Hijacking**
- **Descripción**: Aprovecha una búsqueda incorrecta de bibliotecas en el sistema operativo para cargar una DLL maliciosa en lugar de la legítima.
- **Procedimiento**:
  1. Identificar programas que cargan DLLs sin especificar rutas absolutas.
  2. Crear una DLL maliciosa con el mismo nombre que la legítima.
  3. Colocar la DLL maliciosa en el directorio buscado primero por la aplicación.
- **Herramientas**:
  - **Robber**: Detecta posibles vulnerabilidades de DLL hijacking.
  - **PowerSploit**: Módulo de PowerShell para automatizar pruebas de seguridad, incluyendo la creación de DLL maliciosas.

### 2. **Explotación de vulnerabilidades con Exploit-DB y Vuln-DB**
- **Exploit-DB**: Repositorio público de exploits documentados. Utiliza su base de datos para buscar vulnerabilidades conocidas.
- **Vuln-DB**: Base de datos de vulnerabilidades enfocada en registrar CVEs y análisis de impacto.
- **Procedimiento**:
  1. Identificar software vulnerable en el objetivo.
  2. Buscar exploits específicos en Exploit-DB o Vuln-DB.
  3. Configurar y ejecutar el exploit.

### 3. **Dylib Hijacking (iOS/macOS)**
- **Descripción**: Similar al DLL hijacking, pero afecta a bibliotecas dinámicas en iOS y macOS.
- **Procedimiento**:
  1. Identificar aplicaciones que cargan Dylibs sin rutas completas.
  2. Crear un archivo Dylib malicioso.
  3. Insertarlo en el directorio apropiado para su ejecución.

### 4. **Vulnerabilidades Spectre y Meltdown**
- **Spectre**: Explotación de técnicas de ejecución especulativa para acceder a datos en memoria.
- **Meltdown**: Permite leer datos protegidos en la memoria del kernel.
- **Impacto**:
  - Acceso no autorizado a información sensible, como claves de cifrado o credenciales.
- **Mitigaciones**:
  - Aplicar parches de software y firmware.

### 5. **Named Pipe Impersonation**
- **Descripción**: Manipula las tuberías nombradas en Windows para suplantar credenciales de usuarios privilegiados.
- **Procedimiento**:
  1. Identificar servicios que utilizan tuberías nombradas.
  2. Configurar una tubería maliciosa para interceptar conexiones.
  3. Capturar y usar las credenciales para elevar privilegios.

### 6. **Misconfigured Services**
#### a. **Unquoted Service Paths**
- **Descripción**: Los servicios con rutas de ejecución sin comillas permiten la inyección de archivos ejecutables maliciosos.
- **Procedimiento**:
  1. Buscar servicios con rutas no citadas.
  2. Colocar un binario malicioso en una ruta parcial interpretada antes del programa legítimo.
  
#### b. **Service Object Permissions**
- **Descripción**: Configuraciones incorrectas en permisos de objetos de servicios permiten modificar o reemplazar servicios legítimos.
- **Procedimiento**:
  1. Analizar los permisos del servicio con herramientas como *AccessChk*.
  2. Modificar el binario asociado al servicio para ejecutar código malicioso.

#### c. **Unattend.xml**
- **Descripción**: Archivos de configuración mal protegidos que contienen contraseñas u otra información sensible.
- **Procedimiento**:
  1. Buscar archivos `unattend.xml` en directorios del sistema.
  2. Extraer información sensible como contraseñas en texto plano.

### 7. Pivoting y Relaying
#### **Pivoting**
- **Descripción**: Usar una máquina comprometida como punto intermedio para atacar otras en la red.
- **Pasos**:
  1. Comprometer un sistema inicial.
  2. Configurar un túnel (e.g., con SSH o herramientas como Metasploit).
  3. Escanear la red interna desde la máquina comprometida.

#### **Relaying**
- **Descripción**: Redirigir autenticaciones legítimas a través de un atacante para obtener acceso.
- **Pasos**:
  1. Configurar un servidor de retransmisión (e.g., NTLMRelayX).
  2. Interceptar una solicitud de autenticación.
  3. Usar las credenciales capturadas para autenticarse contra otro recurso.

### 8. Misconfigured NFS (Puerto 2049)
- **Descripción**: Configuraciones incorrectas de sistemas de archivos en red (NFS) permiten acceso no autorizado.
- **Procedimiento**:
  1. Enumerar recursos compartidos con herramientas como *showmount*.
  2. Montar los recursos compartidos para explorar archivos sensibles.

#### RPC (Remote Procedure Call)
- **Descripción**: RPC permite que procesos se comuniquen entre sí, y configuraciones incorrectas pueden dar acceso a atacantes.
- **Procedimiento**:
  1. Identificar servicios RPC expuestos en la red.
  2. Explotar la configuración incorrecta para ejecutar comandos remotos.

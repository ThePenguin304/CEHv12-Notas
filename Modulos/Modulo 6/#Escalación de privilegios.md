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
 
---

## Cómo protegerse ante escaladas de privilegios

1. Restringir los privilegios de inicio de sesión interactivo
2. Ejecutar usuarios y aplicaciones con los privilegios más bajos
3. Implementar la autenticación y autorización multifactor
4. Ejecutar servicios como cuentas sin privilegios
5. Implementar una metodología de separación de privilegios para limitar el alcance de errores y fallos de programación
6. Usar una técnica de cifrado para proteger datos confidenciales
7. Reducir la cantidad de código que se ejecuta con un privilegio en particular
8. Realizar la depuración mediante comprobadores de límites y pruebas de estrés
9. Probar exhaustivamente el sistema para detectar errores y fallos de codificación de aplicaciones
10. Aplicar parches y actualizar el kernel con regularidad
11. Cambie la configuración de UAC a “Notificar siempre”
12. Restringa a los usuarios la posibilidad de escribir archivos en las rutas de búsqueda de las aplicaciones
13. Supervise continuamente los permisos del sistema de archivos mediante herramientas de auditoría
14. Reduzca los privilegios de los usuarios y grupos para que solo los administradores legítimos puedan realizar cambios en el servicio
15. Use herramientas de listas blancas para identificar y bloquear software malicioso
16. Use rutas totalmente calificadas en todas las aplicaciones de Windows
17. Asegúrese de que todos los ejecutables se coloquen en directorios protegidos contra escritura
18. En macOS, haga que los archivos plist sean de solo lectura
19. Bloquee utilidades del sistema no deseadas o software que pueda usarse para programar tareas
20. Aplique parches y actualice periódicamente el servidor web

## Defensa contra vulnerabilidades Spectre y Meltdown

1. Actualice y aplique parches de forma regular a los sistemas operativos y al firmware
2. Habilite la supervisión continua de aplicaciones y servicios críticos que se ejecutan en el sistema y la red
3. Aplique parches de forma regular al software vulnerable, como los navegadores
4. Instale y actualice bloqueadores de anuncios y software antimalware para bloquear la inyección de malware a través de sitios web comprometidos
5. Habilite medidas de protección tradicionales, como herramientas de seguridad de puntos finales, para evitar el acceso no autorizado al sistema
6. Bloquee servicios y aplicaciones que permitan a usuarios sin privilegios ejecutar código
7. Nunca instale software no autorizado ni acceda a sitios web no confiables desde sistemas que almacenan información confidencial
8. Use soluciones de prevención de pérdida de datos (DLP) para evitar la fuga de información crítica de la memoria de tiempo de ejecución
9. Consulte con frecuencia al fabricante para obtener actualizaciones del BIOS y siga las instrucciones proporcionadas por el fabricante para instalar las actualizaciones

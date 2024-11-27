## 6.2 Vulnerability Explotation

La explotación de vulnerabilidades es el proceso de utilizar debilidades en sistemas, aplicaciones o redes para obtener acceso no autorizado, controlar recursos o interrumpir la operación de un sistema.

### Vulnerabilidad:
Es una debilidad o error en el diseñopuede ser aprovechada por atacantes.

### Explotación:
Aprovechar esta vulnerabilidad mediante un exploit, que puede ser un programa, script o conjunto de acciones que interactúan con el sistema para lograr un objetivo malicioso.

Objetivos de la explotación: Acceso, Escalada, Persistencia, Robo de información y Interrupción.

### Pasos

**1. Identificar la vulnerabilidad**
- Localizar debilidades específicas que puedan ser explotadas.
- Uso de escáneres de vulnerabilidades (Nessus, OpenVAS) o herramientas específicas según el sistema.

**2. Determinar la capacidad de la vulnerabilidad**
- Evaluar qué tan explotable es la vulnerabilidad.
- Tipo de acceso que otorga, Compatibilidad con sistemas objetivos y versión del software.

**3. Determinar el riesgo asociado con la vulnerabilidad**
- Evaluar el impacto potencial si la vulnerabilidad se explota.
- Consideraciones: Calificación de riesgo basada en CVSS y existencia de mitigaciones (parches, firewalls, IDS/IPS).

**4. Desarrollar el exploit**
- Crear o ajustar un código/ataque para aprovechar la vulnerabilidad identificada.
- Herramientas comunes: Frameworks como Metasploit o Canvas, Scripts personalizados en lenguajes como Python, Ruby o PowerShell, Exploits públicos (e.g., de bases de datos como Exploit-DB).
  
**5. Generar y entregar la carga útil (payload)**
- Diseñar el código que se ejecutará en el sistema objetivo una vez que el exploit sea exitoso.
- Ejemplos de payloads: Shell inversa (Reverse Shell), meterpreter para interacción avanzada y keyloggers, acceso a archivos, o modificación de configuración.
- Usar herramientas como msfvenom para combinar exploit y payload según las necesidades.

**6. Obtener acceso remoto**
- Acceder al sistema objetivo mediante una conexión establecida tras la explotación exitosa.
- Mantener el control mediante herramientas como Netcat, Meterpreter o conexiones persistentes.
- Pasos siguientes: Elevar privilegios si es necesario y establecer mecanismos de persistencia para conservar el acceso.

--- 

## Buffer Overflow

Un **Buffer Overflow** ocurre cuando un programa escribe más datos en un buffer de los que este puede manejar, sobrescribiendo datos adyacentes en la memoria. Este comportamiento puede ser explotado para ejecutar código malicioso o causar fallos en el programa.


### Tipos de Buffer Overflow

1. **Stack-based Buffer Overflow**  
   Este tipo de ataque ocurre en la pila (stack). Es el más común y ocurre cuando los datos sobrescriben direcciones de retorno o variables locales.

2. **Heap-based Buffer Overflow**  
   Este tipo de ataque ocurre en el montón (heap). Es más difícil de explotar, pero puede permitir al atacante sobrescribir estructuras de datos en memoria dinámica.

---

## Windows Buffer Overflow Exploitation

La explotación de **Buffer Overflow** en sistemas Windows es una técnica común utilizada para ejecutar código malicioso, obteniendo acceso no autorizado o control total sobre el sistema objetivo. Este tipo de explotación a menudo se realiza manipulando la memoria del programa.

### Fundamentos de Windows Buffer Overflow

1. **EIP (Extended Instruction Pointer)**  
   El objetivo principal de un ataque de Buffer Overflow es sobrescribir el registro **EIP**, que contiene la dirección de la siguiente instrucción a ejecutar. Al controlar el EIP, el atacante puede redirigir el flujo de ejecución del programa.

2. **Protecciones comunes en Windows**  
   Aunque modernas versiones de Windows implementan protecciones contra Buffer Overflow, estas pueden ser eludidas bajo ciertas condiciones:
   - **DEP (Data Execution Prevention)**: Marca ciertas regiones de memoria como no ejecutables.
   - **ASLR (Address Space Layout Randomization)**: Aleatoriza las direcciones base de bibliotecas y ejecutables.
   - **SafeSEH (Safe Structured Exception Handling)**: Verifica excepciones para evitar manipulaciones maliciosas.

## Etapas de la explotación

1. **Identificación de la vulnerabilidad**. Encontrar un software vulnerable que permita escribir más datos de los que el buffer puede manejar.
2. **Control del EIP**. Sobrescribir el EIP con una dirección controlada por el atacante, generalmente apuntando a un **shellcode**.
3. **Creación del shellcode**. Desarrollar un payload que se ejecute en el sistema objetivo. Por ejemplo:
  - Ejecutar una shell reversa.
  - Cargar un ejecutable malicioso.

4. **Evasión de protecciones**  
   Diseñar el exploit para evitar DEP, ASLR u otras protecciones.

---

## Return Oriented Programming (ROP)

**Return Oriented Programming (ROP)** es una técnica avanzada de explotación que permite ejecutar código malicioso sin inyectar código directamente, evadiendo protecciones como **DEP (Data Execution Prevention)**.

---

## Exploit Chaining

**Exploit Chaining** es la técnica de combinar múltiples vulnerabilidades o exploits para maximizar el impacto de un ataque. Este enfoque permite a un atacante avanzar progresivamente en un sistema objetivo, superando restricciones individuales o elevando privilegios.

## ¿Cómo funciona Exploit Chaining?
  1. **Identificar varias vulnerabilidades**  
   Las fallas individuales pueden ser limitadas en impacto, pero al combinarse pueden lograr un objetivo mayor.

  2. **Encadenar las vulnerabilidades**  
   Cada exploit prepara el terreno para el siguiente, formando una secuencia lógica de acciones.

  3. **Ejecutar el ataque completo**  
   Se implementan los exploits en orden para lograr el acceso o control deseado.

---

## Active Directory Enumeration Using PowerView

**PowerView** es un módulo de PowerShell ampliamente utilizado para realizar enumeración en entornos de Active Directory (AD). Permite a los atacantes o pentesters recopilar información sobre usuarios, grupos, máquinas y políticas en un dominio de manera eficiente.

## ¿Qué es PowerView?
  - **Enumeración de usuarios, grupos y máquinas**.
  - **Identificación de configuraciones y permisos inseguros en AD**.
  - **Localización de objetivos privilegiados** como cuentas de administrador de dominio.

## Principales comandos de PowerView

- **Enumerar dominios**:
```powershell
Get-ADDomain
Get-NetDomain
Get-DomainSID
```

- **Enumerar políticas de dominio**:
```powershell
Get-DomainPolicy
(Get-DomainPolicy)."SystemAccess"
(Get-DomainPolicy)."kerberospolicy"
```

- **Enumerar Domain Controllers (DC)**:
```powershell
Get-NetDomainController
```

- **Enumerar Domain Users**:
```powershell
Get-NetUser
Get-NetLoggedon -ComputerName <computer-name>
Get-UserProperty –Properties pwdlastset
Find-LocalAdminAccess Invoke-EnumerateLocalAdmin
```

- **Enumerar Domain Computers**:
```powershell
Get-NetComputer
Get-NetComputer - OperatingSystem "*Server 2022*"
Get-NetComputer -Ping
```

- **Enumerar Domain Groups**:
```powershell
Get-NetGroup
Get-NetGroup -Domain <targetdomain>
Get-NetGroup 'Domain Administrators'
Get-NetGroup “*admin*”
Get-NetGroupMember - GroupName "Domain Admins"
Get-NetGroup -UserName <"username">
Get-NetLocalGroup - ComputerName <computername>
Get-NetLoggedon - ComputerName <DomainName>
Get-LastLoggedOn - ComputerName <DomainName>
```

- **Enumerar Domain Shares**:
```powershell
Invoke-ShareFinder -Verbose
Get-NetShare
Get-NetFileServer -Verbose
Invoke-FileFinder
```

- **Enumerar GPO**:
```powershell
Get-NetGPO Get-NetGPO| select displayname
Get-NetOU
```

- **Enumerar ACL**:
```powershell
Get-ObjectAcl -
SamAccountName "users" - ResolveGUIDs
Get-NetGPO | %{Get-ObjectAcl -ResolveGUIDs - Name $_.Name}
Invoke-ACLScanner - ResolveGUIDs
Get-PathAcl -Path \\Windows11\Users (Works only with the shared folder)
```

---

## Herramientas

### BloodHound
**BloodHound** es una herramienta gráfica utilizada para enumerar y analizar relaciones dentro de un dominio de Active Directory (AD). Es particularmente útil para identificar rutas hacia objetivos de alto valor, como cuentas de administrador de dominio.

Casos de uso:
- Identificación de configuraciones inseguras (e.g., permisos excesivos en objetos).
- Descubrimiento de relaciones ocultas que pueden facilitar el movimiento lateral.
- Priorización de objetivos durante un pentest.

## GhostPack

GhostPack es una colección de herramientas desarrolladas en C# para tareas de post-explotación en Windows. Incluye herramientas que permiten a los atacantes realizar diversas acciones después de haber comprometido un sistema Windows.

Herramientas dentro de GhostPack:
- Seatbelt: Herramienta para la recolección de información de post-explotación y análisis de privilegios en sistemas Windows.
- Rubeus: Herramienta de Kerberos para la explotación y manipulación de tickets de autenticación en Active Directory.
- SharpSploit: Biblioteca que permite la ejecución de varios ataques de post-explotación, como inyecciones de DLL y ejecución de código remoto.

Casos de uso:
- Escalada de privilegios: Usando técnicas de inyección de código y abuso de configuraciones inseguras.
- Post-explotación: Recolección de información sobre usuarios, grupos y configuraciones de red.
- Explotación de credenciales: Obtención y manipulación de credenciales a través de Kerberos.

## Seatbelt
Seatbelt es una herramienta de post-explotación que se utiliza para auditar y explorar los privilegios y configuraciones del sistema en máquinas Windows. Su objetivo principal es facilitar la recolección de información para determinar si es posible una escalada de privilegios o movimiento lateral.

Características principales:
- Recolección de información: Recopila detalles sobre el sistema, usuarios, grupos, y configuraciones de seguridad.
- Pruebas de privilegios: Evalúa las configuraciones y permite probar el acceso a recursos sensibles.
- Soporte para PowerShell: Puede ejecutarse directamente desde PowerShell para auditar sistemas comprometidos.

Casos de uso:
- Auditoría de privilegios: Identificar cuentas de usuarios con privilegios elevados y configuraciones inseguras.
- Evaluación de seguridad: Determinar si un atacante podría escalar privilegios en el sistema comprometido.

## OllyDbg
OllyDbg es un depurador estático para programas de Windows que permite el análisis de código binario en busca de vulnerabilidades. Es ampliamente utilizado por los analistas de malware y los pentesters para analizar binarios y estudiar su comportamiento.

---

# Defensa contra los Desbordamientos de Búfer (Buffer Overflows)

### 1. Desarrollar programas siguiendo prácticas y directrices de codificación segura
Es fundamental crear software siguiendo estándares y mejores prácticas de seguridad para minimizar los riesgos de desbordamientos de búfer y otras vulnerabilidades. Esto incluye la validación de entradas, el uso de bibliotecas seguras y evitando la ejecución de código no seguro.

### 2. Usar la técnica de Randomización del Espacio de Direcciones (ASLR)
La **ASLR** (Address Space Layout Randomization) es una técnica de seguridad que aleatoriza las direcciones de memoria utilizadas por un programa, lo que hace más difícil para un atacante predecir la ubicación de las funciones y datos importantes del programa, dificultando la explotación de vulnerabilidades como los desbordamientos de búfer.

### 3. Validar los argumentos y minimizar el código que requiere privilegios de root
Siempre que sea posible, valida las entradas del usuario y evita dar privilegios de root o administrador a partes del código que no los necesiten. Esto reduce el riesgo de que un atacante pueda ejecutar código malicioso con privilegios elevados.

### 4. Realizar revisiones de código a nivel de código fuente utilizando analizadores estáticos y dinámicos
Las herramientas de análisis estático y dinámico ayudan a identificar vulnerabilidades, como los desbordamientos de búfer, sin necesidad de ejecutar el código. Los analizadores estáticos revisan el código fuente, mientras que los dinámicos monitorean el comportamiento del programa durante la ejecución.

### 5. Permitir que el compilador añada límites a todos los búferes e implementar comprobaciones automáticas de límites
La mayoría de los compiladores modernos permiten habilitar opciones para realizar comprobaciones de límites en los búferes durante la compilación. Esto ayuda a prevenir desbordamientos al garantizar que los datos no se escriban fuera de los límites de los búferes.

### 6. Siempre proteger el puntero de retorno en la pila
El puntero de retorno en la pila es un objetivo común de los atacantes, que pueden sobrescribirlo durante un desbordamiento de búfer. Utilizar técnicas como el **Stack Canaries** o la **Protección de Puntero de Retorno** ayuda a prevenir este tipo de ataques.

### 7. Nunca permitir la ejecución de código fuera del espacio de código
Utiliza medidas como **Data Execution Prevention** (DEP) para asegurarte de que las regiones de memoria no ejecutables no puedan ser ejecutadas, bloqueando así intentos de ejecutar código malicioso que haya sido inyectado en áreas no previstas para código ejecutable.

### 8. Parchear regularmente las aplicaciones y sistemas operativos
Mantener actualizado el software es crucial para protegerlo de vulnerabilidades conocidas, como los desbordamientos de búfer. Los parches y actualizaciones de seguridad ayudan a corregir fallos en el software que podrían ser explotados por atacantes.

### 9. Realizar inspección de código manualmente con una lista de verificación para asegurar que el código cumpla ciertos criterios
El análisis manual del código fuente, usando listas de verificación de seguridad, es una buena práctica para identificar vulnerabilidades que las herramientas automáticas pueden pasar por alto.

### 10. Emplear la prevención de ejecución de datos (DEP) para marcar las regiones de memoria como no ejecutables
El **DEP** es una característica de seguridad que evita la ejecución de código en áreas de memoria no ejecutables. Esto ayuda a proteger las aplicaciones contra ataques de desbordamiento de búfer, ya que evita que el código inyectado en la pila o el heap se ejecute.

### 11. Implementar comprobaciones de integridad de punteros de código para detectar si un puntero de código ha sido alterado
Las comprobaciones de integridad de punteros de código ayudan a detectar alteraciones en los punteros de ejecución, lo que podría ser un indicio de un ataque de desbordamiento de búfer. Esta técnica valida que los punteros de código no se hayan modificado de manera maliciosa.

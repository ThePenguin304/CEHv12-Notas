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

### 1. Enumeración de usuarios

- **Lista de todos los usuarios en el dominio**:
```powershell
Get-DomainUser
```


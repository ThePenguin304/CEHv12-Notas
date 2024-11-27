## 6.2 Vulnerability Explotation

La explotación de vulnerabilidades es el proceso de utilizar debilidades en sistemas, aplicaciones o redes para obtener acceso no autorizado, controlar recursos o interrumpir la operación de un sistema. En el contexto de la ciberseguridad ética, este proceso forma parte de un test de penetración o auditoría para identificar riesgos y fortalecer la seguridad.

#### Vulnerabilidad:
Es una debilidad o error en el diseño, implementación o configuración de un sistema que puede ser aprovechada por atacantes para causar daño o acceder a recursos.

#### Explotación:
Es el acto de aprovechar esta vulnerabilidad mediante un exploit, que puede ser un programa, script o conjunto de acciones que interactúan con el sistema para lograr un objetivo malicioso (o ético, en caso de un pentester).
- Objetivos de la explotación
  - Acceso: Obtener control parcial o completo sobre un sistema.
  - Escalada: Elevar privilegios en el sistema objetivo (e.g., de usuario regular a administrador/root).
  - Persistencia: Mantener el acceso para acciones futuras.
  - Robo de información: Acceso a datos confidenciales.
  - Interrupción: Afectar la disponibilidad de un sistema (e.g., ataques DoS).

## 1. Identificar la vulnerabilidad

- Definición: Localizar debilidades específicas en un sistema, software o configuración que puedan ser explotadas.
- Técnicas: Uso de escáneres de vulnerabilidades (Nessus, OpenVAS) o herramientas específicas según el sistema (e.g., Nmap para detectar servicios vulnerables).
- Objetivo: Asegurarse de que la vulnerabilidad es relevante y aplicable a tu entorno de pruebas.

## 2. Determinar la capacidad de la vulnerabilidad

- Definición: Evaluar qué tan explotable es la vulnerabilidad.
- Aspectos clave:
  - Tipo de acceso que otorga (local, remoto, privilegios de usuario).
  - Requisitos previos (e.g., credenciales, acceso físico).
  - Compatibilidad con sistemas objetivos y versión del software.

## 3. Determinar el riesgo asociado con la vulnerabilidad
- Definición: Evaluar el impacto potencial si la vulnerabilidad se explota.
- Consideraciones:
  - Impacto en la confidencialidad, integridad y disponibilidad (Triada CIA).
  - Calificación de riesgo basada en CVSS (Common Vulnerability Scoring System).
  - Existencia de mitigaciones (parches, firewalls, IDS/IPS).

## 4. Desarrollar el exploit
- Definición: Crear o ajustar un código/ataque para aprovechar la vulnerabilidad identificada.
- Herramientas comunes:
  - Frameworks como Metasploit o Canvas.
  - Scripts personalizados en lenguajes como Python, Ruby o PowerShell.
  - Exploits públicos (e.g., de bases de datos como Exploit-DB).
- Pruebas: Probar el exploit en un entorno controlado antes de ejecutarlo en el objetivo.
  
## 5. Generar y entregar la carga útil (payload)
- Definición: Diseñar el código que se ejecutará en el sistema objetivo una vez que el exploit sea exitoso.
- Ejemplos de payloads:
  - Shell inversa (Reverse Shell).
  - Meterpreter para interacción avanzada.
  - Keyloggers, acceso a archivos, o modificación de configuración.
- Personalización: Usar herramientas como msfvenom para combinar exploit y payload según las necesidades.
- Seleccionar el método de entrega: local o remoto
  - Local:
    - Cuando ya se tiene acceso parcial al sistema.
    - Ejemplo: Exploits que requieren ejecución por parte de un usuario autenticado.
  - Remoto:
  -   Para atacar sistemas accesibles a través de la red.
  -   Ejemplo: Ataques contra servidores web, servicios RDP, o SSH.

## 6. Obtener acceso remoto
- Definición: Acceder al sistema objetivo mediante una conexión establecida tras la explotación exitosa.
- Objetivo: Mantener el control mediante herramientas como Netcat, Meterpreter o conexiones persistentes.
- Pasos siguientes:
  - Elevar privilegios si es necesario (e.g., exploits de escalada de privilegios).
  - Establecer mecanismos de persistencia para conservar el acceso.

--- 

# Buffer Overflow

Un **Buffer Overflow** ocurre cuando un programa escribe más datos en un buffer de los que este puede manejar, sobrescribiendo datos adyacentes en la memoria. Este comportamiento puede ser explotado para ejecutar código malicioso o causar fallos en el programa.

---

## Tipos de Buffer Overflow

1. **Stack-based Buffer Overflow**  
   Este tipo de ataque ocurre en la pila (stack). Es el más común y ocurre cuando los datos sobrescriben direcciones de retorno o variables locales.

2. **Heap-based Buffer Overflow**  
   Este tipo de ataque ocurre en el montón (heap). Es más difícil de explotar, pero puede permitir al atacante sobrescribir estructuras de datos en memoria dinámica.

---

## Ejemplo básico en C

A continuación, un ejemplo simple que demuestra un **Stack-based Buffer Overflow**:

```c
#include <stdio.h>
#include <string.h>

void vulnerable_function(char *input) {
    char buffer[8]; // Buffer con tamaño fijo
    strcpy(buffer, input); // Copia sin verificar el tamaño del input
    printf("Buffer: %s\n", buffer);
}

int main(int argc, char *argv[]) {
    if (argc < 2) {
        printf("Usage: %s <input>\n", argv[0]);
        return 1;
    }
    vulnerable_function(argv[1]);
    return 0;
}
```

# Windows Buffer Overflow Exploitation

La explotación de **Buffer Overflow** en sistemas Windows es una técnica común utilizada para ejecutar código malicioso, obteniendo acceso no autorizado o control total sobre el sistema objetivo. Este tipo de explotación a menudo se realiza manipulando la memoria del programa.

---

## Fundamentos de Windows Buffer Overflow

1. **EIP (Extended Instruction Pointer)**  
   El objetivo principal de un ataque de Buffer Overflow es sobrescribir el registro **EIP**, que contiene la dirección de la siguiente instrucción a ejecutar. Al controlar el EIP, el atacante puede redirigir el flujo de ejecución del programa.

2. **Protecciones comunes en Windows**  
   Aunque modernas versiones de Windows implementan protecciones contra Buffer Overflow, estas pueden ser eludidas bajo ciertas condiciones:
   - **DEP (Data Execution Prevention)**: Marca ciertas regiones de memoria como no ejecutables.
   - **ASLR (Address Space Layout Randomization)**: Aleatoriza las direcciones base de bibliotecas y ejecutables.
   - **SafeSEH (Safe Structured Exception Handling)**: Verifica excepciones para evitar manipulaciones maliciosas.

---

## Etapas de la explotación

1. **Identificación de la vulnerabilidad**  
   Encontrar un software vulnerable que permita escribir más datos de los que el buffer puede manejar.

2. **Control del EIP**  
   Sobrescribir el EIP con una dirección controlada por el atacante, generalmente apuntando a un **shellcode**.

3. **Creación del shellcode**  
   Desarrollar un payload que se ejecute en el sistema objetivo. Por ejemplo:
   - Ejecutar una shell reversa.
   - Cargar un ejecutable malicioso.

4. **Evasión de protecciones**  
   Diseñar el exploit para evitar DEP, ASLR u otras protecciones.

---

## Ejemplo básico: Windows Buffer Overflow

El siguiente ejemplo ilustra cómo enviar una cadena maliciosa para provocar un **Buffer Overflow** y sobrescribir el registro EIP.

### Código vulnerable en C:

```c
#include <stdio.h>
#include <string.h>

void vulnerable_function(char *input) {
    char buffer[256]; // Buffer estático
    strcpy(buffer, input); // Copia sin límites
    printf("Buffer: %s\n", buffer);
}

int main(int argc, char *argv[]) {
    if (argc < 2) {
        printf("Usage: %s <input>\n", argv[0]);
        return 1;
    }
    vulnerable_function(argv[1]);
    return 0;
}
```

# Return Oriented Programming (ROP)

**Return Oriented Programming (ROP)** es una técnica avanzada de explotación que permite ejecutar código malicioso sin inyectar código directamente, evadiendo protecciones como **DEP (Data Execution Prevention)**.

---

# Exploit Chaining

**Exploit Chaining** es la técnica de combinar múltiples vulnerabilidades o exploits para maximizar el impacto de un ataque. Este enfoque permite a un atacante avanzar progresivamente en un sistema objetivo, superando restricciones individuales o elevando privilegios.

## ¿Cómo funciona Exploit Chaining?

Un ataque con Exploit Chaining consiste en:
1. **Identificar varias vulnerabilidades**  
   Las fallas individuales pueden ser limitadas en impacto, pero al combinarse pueden lograr un objetivo mayor.

2. **Encadenar las vulnerabilidades**  
   Cada exploit prepara el terreno para el siguiente, formando una secuencia lógica de acciones.

3. **Ejecutar el ataque completo**  
   Se implementan los exploits en orden para lograr el acceso o control deseado.

---


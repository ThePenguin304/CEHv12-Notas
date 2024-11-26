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

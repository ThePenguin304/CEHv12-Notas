# Clasificación de vulnerabilidades

## 1. Malas configuraciones

Las malas configuraciones son una de las vulnerabilidades más comunes, generalmente causadas por errores humanos.

- Red (Network Misconfigurations):
```bash
 - Protocolos inseguros: Uso de versiones antiguas como FTP, HTTP o Telnet.
 - Puertos abiertos y servicios innecesarios: Pueden ser utilizados como vectores de ataque.
 - Errores de configuración: Como contraseñas predeterminadas o sin restricciones de acceso.
 - Software desactualizado: Deja expuestas vulnerabilidades conocidas.
 - Cifrado débil: Uso de algoritmos obsoletos (MD5, SHA-1) o mala distribución de claves.

```
- Host (Host Misconfigurations):

```bash
Elevación de permisos indebida: Configuración que permite a usuarios no autorizados obtener permisos de administrador.
Permisos abiertos: Recursos accesibles para usuarios no autorizados.
Cuentas de root inseguras: No están protegidas adecuadamente (contraseñas débiles o configuraciones predeterminadas).
```

## 2.Errores en aplicaciones (Application Vulnerabilities)

Las vulnerabilidades en aplicaciones son fallos de programación que pueden ser explotados por atacantes. Ejemplos:

```bash
- Buffer Overflows: Cuando un programa escribe datos más allá de su área asignada en memoria, permitiendo la ejecución de código malicioso.
- Memory Leaks: Pérdida de memoria debido a una gestión incorrecta, lo que puede derivar en la caída del sistema.
- Ataques de agotamiento de recursos (Resource Exhaustion): Consumo de recursos hasta que el sistema no pueda responder (DoS o DDoS).
- Integer Overflows: Exceder el rango permitido para números enteros, causando comportamientos inesperados.
- Null Pointer/Object Dereference: Referencia a objetos o punteros nulos, provocando fallos en el programa.
- DLL Injection: Inyección de una biblioteca dinámica (DLL) maliciosa para ejecutar código en un programa legítimo.
- Race Conditions: Comportamientos no deseados cuando múltiples hilos acceden a recursos compartidos simultáneamente. Ejemplo: Ataques de Time-of-Check to Time-of-Use (TOCTOU).
- Input Improper Handling: No validar correctamente los datos que ingresan a la aplicación (ej.: SQL Injection).
- Output Improper Handling: Exponer información sensible en las respuestas o logs.
```

## 3.Falta de gestión de parches (Poor Patch Management)

* Servidores y firmware sin parchear: Sistemas que no aplican actualizaciones de seguridad, dejando puertas abiertas a exploits conocidos.
* Sistemas Operativos (OS): Vulnerabilidades en SO desactualizados.
* Aplicaciones: Software con versiones obsoletas o sin parches aplicados.


## 4.Errores de diseño (Design Flaws)

* Vulnerabilidades intrínsecas en la arquitectura o lógica de los sistemas.
* Falta de separación de privilegios, diseños que no contemplan seguridad desde el inicio (Security by Design).

## 5.Riesgos por terceras partes y gestión de proveedores (Third-Party Risks)

* Gestión de proveedores (Vendor Management): Falta de controles para asegurar que los proveedores cumplen con estándares de seguridad.
* Integración de sistemas (System Integration): Problemas al unir sistemas de diferentes proveedores que no fueron diseñados para trabajar juntos.
* Falta de soporte del proveedor: Hardware o software abandonado por los fabricantes, sin actualizaciones ni soporte.

## 6.Riesgos en la cadena de suministro (Supply Chain Risks)

Desarrollo de código externo (Outsourced Code Development):
* Riesgos en almacenamiento de datos: Posibles fugas o acceso no autorizado.
* Nube (Cloud-based) vs. on-premise:

```bash
* En la nube: Mayor exposición a ataques si no se configuran adecuadamente.
* On-premise: Riesgos relacionados con recursos limitados y actualizaciones.
```

## 7.Instalaciones y configuraciones por defecto
- Errores del sistema operativo (OS Errors): Configuraciones predeterminadas que no garantizan la seguridad (por ejemplo, permisos innecesarios).
- Contraseñas por defecto: Facilitan el acceso no autorizado si no se cambian.

## 8.Vulnerabilidades Zero-Day
- Fallos desconocidos para el fabricante y la comunidad de seguridad, explotables hasta que se publique un parche.

## 9.Plataformas con vulnerabilidades heredadas (Legacy Systems)
- Sistemas antiguos que no soportan actualizaciones o no están diseñados para resistir ataques modernos.

## 10.Dispersión del sistema y activos no documentados (System Sprawl/Undocumented Assets)
- Infraestructuras extendidas sin un inventario claro, lo que dificulta la identificación y mitigación de vulnerabilidades.

## 11.Gestión inadecuada de certificados y claves (Improper Certificate and Key Management)
Uso incorrecto o almacenamiento inseguro de certificados y claves criptográficas, lo que permite su robo o uso indebido.

---

# Tipos de evaluación de vulnerabilidades

Las evaluaciones de vulnerabilidades son esenciales para identificar, clasificar y priorizar las debilidades en un sistema. Existen varios tipos de evaluación que se realizan utilizando distintas metodologías y herramientas. A continuación, se describen los tipos de evaluaciones y enfoques relevantes en el contexto de la ciberseguridad:

## 1. Tipos de Evaluaciones de Vulnerabilidad

A. Evaluación Activa (Active Assessment)
- Descripción: Implica el escaneo activo de los sistemas, donde el evaluador interactúa directamente con los activos, enviando solicitudes de prueba (paquetes, consultas) para identificar posibles vulnerabilidades.
- Ventajas: Detecta vulnerabilidades en tiempo real y permite obtener información detallada sobre los sistemas.
- Desventajas: Intrusiva y puede generar tráfico de red sospechoso o alertas en los sistemas y puede interrumpir la operatividad de los servicios si no se maneja adecuadamente.

B. Evaluación Pasiva (Passive Assessment)
- Descripción: No interactúa directamente con los sistemas ni realiza cambios visibles. Se centra en la observación y análisis del tráfico de la red sin intervención activa.
- Ventajas: No intrusiva, por lo que minimiza el riesgo de ser detectado y No interrumpe los servicios.
- Desventajas: Menos precisa, ya que depende del tráfico que está disponible sin realizar interacciones directas.

C. Evaluación Externa (External Assessment)
- Descripción: Se lleva a cabo desde fuera de la red de la organización, simulando un atacante externo que intenta acceder a los recursos sin tener acceso directo al sistema.
- Enfoque: Evaluación de la seguridad perimetral, como firewalls, VPNs y servicios expuestos a Internet.
- Ventajas: Identifica vulnerabilidades que pueden ser explotadas por atacantes externos.
- Desventajas: Puede no descubrir vulnerabilidades internas.

D. Evaluación Interna (Internal Assessment)
- Descripción: Realizada desde dentro de la red de la organización, simula un ataque por parte de un atacante con acceso físico o lógico dentro de la red.
- Enfoque: Evaluación de la seguridad interna, como controles de acceso, credenciales, configuraciones de red interna y comunicaciones entre sistemas.
- Ventajas: Identifica vulnerabilidades que solo pueden ser vistas desde dentro de la red.
- Desventajas: Requiere más permisos o acceso a sistemas internos.

E. Evaluación Basada en Host (Host-based Assessment)
- Descripción: Se enfoca en los sistemas individuales (servidores, estaciones de trabajo, dispositivos) y analiza configuraciones, parches y vulnerabilidades locales.
- Enfoque: Inspección de configuraciones de seguridad, acceso de usuario, sistemas operativos y software.
- Ventajas: Permite encontrar vulnerabilidades específicas a nivel de sistema.
- Desventajas: No evalúa la red ni la infraestructura externa.

F. Evaluación Basada en Red (Network-based Assessment)
- Descripción: Evaluación de la infraestructura de red para encontrar vulnerabilidades como puertos abiertos, servicios sin asegurar y configuraciones incorrectas.
- Enfoque: Escaneo de puertos, servicios y configuraciones de dispositivos de red.
- Ventajas: Permite obtener una visión clara de la seguridad de la red.
- Desventajas: No se enfoca en la seguridad de los sistemas o aplicaciones individuales.

G. Evaluación de Aplicaciones (Application Assessment)
- Descripción: Se enfoca en las vulnerabilidades de las aplicaciones, buscando fallos como inyecciones de SQL, XSS, CSRF y errores de autenticación.
- Enfoque: Análisis de código fuente, interacción con aplicaciones web y revisión de parámetros de seguridad.
- Ventajas: Identifica vulnerabilidades críticas en aplicaciones que pueden ser explotadas por atacantes.
- Desventajas: Requiere conocimientos específicos del código y la arquitectura de la aplicación.

H. Evaluación de Bases de Datos (Database Assessment)
- Descripción: Evaluación de las bases de datos, incluyendo su configuración, control de acceso, cifrado y gestión de datos sensibles.
- Enfoque: Análisis de permisos, contraseñas, almacenamiento de datos y vulnerabilidades comunes en sistemas de bases de datos.
- Ventajas: Ayuda a proteger datos críticos y sensibles.
- Desventajas: Puede ser limitado a la base de datos y no a la infraestructura completa.

I. Evaluación de Redes Inalámbricas (Wireless Network Assessment)
- Descripción: Analiza la seguridad de las redes Wi-Fi, buscando vulnerabilidades como contraseñas débiles, redes abiertas o protocolos inseguros.
- Enfoque: Escaneo de redes Wi-Fi para encontrar configuraciones inseguras o puntos de acceso no autorizados.
- Ventajas: Protege la infraestructura inalámbrica.
- Desventajas: Puede ser difícil de realizar en redes de gran alcance.

J. Evaluación Distribuida (Distributed Assessment)
- Descripción: Se realiza desde múltiples ubicaciones para simular un ataque distribuido que proviene de diferentes puntos en una red global.
- Enfoque: Escaneo de redes, sistemas y aplicaciones desde diferentes lugares para cubrir posibles vectores de ataque.
- Ventajas: Proporciona una visión más completa de la infraestructura.
- Desventajas: Requiere más recursos y puede ser más complejo de administrar.

K. Evaluación con Credenciales (Credentialed Assessment)
- Descripción: Se realiza con acceso autorizado, usando credenciales válidas para examinar los sistemas de forma más profunda.
- Enfoque: El evaluador tiene acceso completo a las configuraciones y archivos, permitiendo un análisis más exhaustivo.
- Ventajas: Ofrece un análisis profundo de la seguridad interna del sistema.
- Desventajas: Requiere permisos de acceso que no siempre están disponibles.

L. Evaluación sin Credenciales (Non-credentialed Assessment)
- Descripción: Se realiza sin acceso autorizado al sistema, simulando un atacante sin privilegios dentro de la red.
- Enfoque: Se enfoca en la superficie de ataque visible sin explorar detalles internos.
- Ventajas: Puede simular ataques de externos sin acceso a la red interna.
- Desventajas: Limitada en profundidad, ya que no tiene acceso a configuraciones internas.

M. Evaluación Manual (Manual Assessment)
 Descripción: Implica una revisión detallada realizada por un experto en seguridad que analiza los sistemas en busca de vulnerabilidades, generalmente utilizando herramientas específicas.
- Ventajas: Permite una revisión personalizada y más detallada.
- Desventajas: Consume más tiempo y recursos.

N. Evaluación Automatizada (Automated Assessment)
- Descripción: Utiliza herramientas automáticas para realizar escaneos regulares de vulnerabilidades.
- Ventajas: Rápida y eficiente, ideal para escaneos frecuentes y en entornos grandes.
- Desventajas: Puede no detectar vulnerabilidades complejas que requieren un análisis manual.

# Clasificación de vulnerabilidades y tipos de evaluación de vulnerabilidades

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




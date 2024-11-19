# 3. Herramientas de evaluación

## 3.1 Evaluación vulnerabilidades 

La evaluación de vulnerabilidades también puede diferenciarse según producto, servicio, árbol (tree) e inferencia (inference).

### 1. Producto (Product-Based):

- Se enfoca en evaluar vulnerabilidades de un software o hardware específico.
- Ejemplo: Analizar vulnerabilidades de una versión específica de Apache, MySQL, o un sistema operativo.
- Ideal para evaluar componentes individuales de la infraestructura.

### 2. Servicio (Service-Based):

- Examina servicios en ejecución dentro de una red o sistema, como servicios web, bases de datos o servicios de red.
- Busca identificar configuraciones incorrectas o debilidades en el servicio.
- Ejemplo: Evaluar un servidor web (HTTP) o un servicio FTP.

### 3. Árbol (Tree-Based):

- Sigue un enfoque estructurado y jerárquico para evaluar vulnerabilidades.
- Construye un "árbol de decisión" donde cada nodo representa una vulnerabilidad o punto de interés, y las ramas muestran relaciones o dependencias.
- Útil para priorizar evaluaciones y representar visualmente posibles caminos de ataque.

### 4. Inferencia (Inference-Based):

- Utiliza datos de evaluación previos para inferir nuevas vulnerabilidades.
- Ejemplo: Si un sistema tiene una versión obsoleta de un software, puede inferirse que es vulnerable a ciertos exploits conocidos.
- Es útil para análisis predictivos y para ahorrar tiempo en evaluaciones.

---

## 3.2 Carasterísticas de una buena evaluación de riesgos

- **Cobertura Completa.** Abarca todos los activos de la infraestructura: hardware, software, redes, aplicaciones y dispositivos conectados. Incluye tanto vulnerabilidades conocidas como posibles configuraciones inseguras.
- **Precisión.** Reduce los falsos positivos (alertas incorrectas) y los falsos negativos (vulnerabilidades no detectadas). Utiliza herramientas actualizadas y configuradas adecuadamente.
- **Periodicidad y Actualización.** Se realiza de forma regular (mensual, trimestral o según las necesidades del negocio). Considera actualizaciones constantes de bases de datos de vulnerabilidades (como CVE y NVD).
- **Contexto y Priorización.** Analiza el impacto y la probabilidad de explotación de cada vulnerabilidad. Clasifica las vulnerabilidades según su severidad (baja, media, alta, crítica) y su relevancia para los activos críticos.
- **Eficiencia.** Optimiza los recursos utilizados, como tiempo, herramientas y personal. Evita sobrecargar sistemas durante las evaluaciones.
- **Integración con Mitigación.** Proporciona recomendaciones prácticas y detalladas para corregir las vulnerabilidades detectadas. Permite planificar estrategias de remediación o mitigación priorizadas.
- **Adaptabilidad al Entorno.** Se adapta a infraestructuras específicas, como entornos locales, en la nube, híbridos o industriales (ICS/SCADA).
- **Cumplimiento Normativo.** Alineada con estándares y regulaciones relevantes (PCI DSS, ISO 27001, GDPR, etc.).
- **Reportes Claros y Accionables.** Genera informes comprensibles para distintos públicos (técnico y gerencial). Incluye detalles técnicos, gráficos de impacto y recomendaciones claras.
- **Confidencialidad y Ética.** Realiza la evaluación sin comprometer la seguridad de la organización. Protege la información recolectada durante el análisis.

---

## 3.3 Funcionamiento

1. Ubicación de Nodos
  - Descripción: Identificar todos los dispositivos conectados a la red organizacional, como servidores, estaciones de trabajo, dispositivos IoT, routers, switches, entre otros.
  - Herramientas Comunes: Escáneres de red como Nmap o Angry IP Scanner.
  - Objetivo: Crear un mapa completo de los activos en la red, incluyendo direcciones IP, nombres de host y puertos abiertos.
  - Consideraciones:
    - Determinar el alcance del escaneo (interno, externo o ambos).
    - Detectar dispositivos ocultos o mal configurados que podrían ser un riesgo.


2. Descubrimiento de Servicios y Sistemas Operativos en los Nodos
- Descripción: Una vez identificados los nodos, el escáner inspecciona los puertos abiertos y utiliza técnicas de fingerprinting para determinar qué servicios están activos y qué sistemas operativos están en uso.
- Herramientas Comunes: Fingerprinting: Nmap (scripts NSE), Netcat, o herramientas específicas para protocolos (SMB, SSH, HTTP).
- Objetivo: Identificar versiones específicas de servicios (por ejemplo, Apache 2.4.41) y sistemas operativos (por ejemplo, Windows Server 2019). Esto permite focalizar las pruebas de vulnerabilidades.
- Consideraciones:
  - Evitar detecciones agresivas que puedan interrumpir servicios críticos.
  - Recopilar información precisa para el siguiente paso.


3. Prueba de Vulnerabilidades Conocidas en los Servicios y Sistemas Operativos
- Descripción: El escáner compara la información recopilada con bases de datos de vulnerabilidades conocidas, como CVE (Common Vulnerabilities and Exposures), NVD (National Vulnerability Database) o bases de datos propias.
- Herramientas Comunes: Escáneres automáticos como Nessus, Qualys, OpenVAS o Rapid7 Nexpose.
- Objetivo: Identificar vulnerabilidades específicas, como configuraciones inseguras, parches faltantes o versiones vulnerables de software.
- Consideraciones:
  - Priorización de vulnerabilidades según su criticidad (usualmente usando CVSS).
  - Evitar causar interrupciones al realizar pruebas en sistemas sensibles.


## Modo de escaneo

1. Base del Host (Host-Based)
- Descripción: Evalúan vulnerabilidades directamente en el sistema objetivo (servidores, estaciones de trabajo, dispositivos).
- Características: Analizan configuraciones locales, permisos, parches instalados y aplicaciones en ejecución. Suelen requerir acceso administrativo al sistema.
- Ejemplo de Herramientas: Nessus (en modo host), Qualys Cloud Agent.

2. Por Profundidad
- Superficial (Broad Scan):
  - Analizan de forma general, revisando múltiples activos rápidamente.
  - Útil para identificar puntos de entrada iniciales o vulnerabilidades más comunes.
- Profundo (Deep Scan):
  - Escanean exhaustivamente un sistema o servicio, incluyendo configuraciones detalladas, vulnerabilidades complejas y dependencias.
  - Más lento y consome más recursos.
- Ejemplo de Herramientas:
  - Superficial: OpenVAS.
  - Profundo: Nexpose, Burp Suite (para aplicaciones web).

3. Por Aplicación
- Aplicaciones Web: Se centran en aplicaciones web y sus vulnerabilidades específicas (XSS, inyección SQL, CSRF).
  - Ejemplo: Burp Suite, OWASP ZAP.
- Aplicaciones Locales: Analizan vulnerabilidades en software instalado en el sistema local.
  - Ejemplo: Nessus, Tenable.io.

4. Por Alcance
- Interno: Escanean sistemas y servicios dentro de la red organizacional. Útil para detectar amenazas internas y configuraciones inseguras.
  - Ejemplo: Nessus, Qualys VMDR.
- Externo: Analizan los sistemas expuestos a Internet, como servidores web y puertas de enlace.
  - Ejemplo: Shodan, Censys.

5. Activo y Pasivo
- Activo: Interactúan directamente con los sistemas, enviando solicitudes y recopilando respuestas.
  - Ejemplo: Nmap, Nessus.
- Pasivo:
  - Analizan el tráfico de red o registros existentes sin interactuar directamente con los sistemas.
- Ejemplo: Zeek (antes Bro), Wireshark.

6. Por Ubicación
- Red: Escanean dispositivos y servicios en una red completa.
  - Ejemplo: Nmap, Qualys.
- Agente: Se instalan en los sistemas para realizar evaluaciones continuas o específicas.
  - Ejemplo: Qualys Cloud Agent, Rapid7 Insight Agent.
- Proxy: Inspeccionan tráfico entre cliente y servidor para detectar vulnerabilidades de aplicaciones.
  - Ejemplo: Burp Suite, OWASP ZAP.
- Clúster: Analizan infraestructuras distribuidas o servicios en la nube.
  - Ejemplo: Kubernetes-specific tools como kube-hunter.
 
## Criterios para Escoger un Tipo de Evaluación de Vulnerabilidades

1. Objetivo de la Evaluación
- Seguridad General: Escaneos amplios para identificar vulnerabilidades en toda la red.
- Cumplimiento Normativo: Evaluaciones específicas alineadas con regulaciones (PCI DSS, ISO 27001).
- Protección Crítica: Enfoque en activos clave o servicios críticos para el negocio.
2. Alcance del Análisis
- Interno: Si el objetivo es proteger contra amenazas internas o identificar configuraciones inseguras.
- Externo: Para proteger sistemas públicos y servicios expuestos a Internet.
3. Tipo de Infraestructura
- Local: Herramientas diseñadas para escanear redes y sistemas en la infraestructura on-premise.
- Cloud: Herramientas especializadas en analizar entornos en la nube (AWS, Azure, Google Cloud).
- Híbrida: Soluciones que soporten ambos entornos.
4. Recursos Disponibles
- Tiempo: Si se requiere una evaluación rápida, optar por escaneos superficiales.
- Presupuesto: Considerar herramientas gratuitas (como OpenVAS) frente a soluciones comerciales (como Nessus).
- Capacidades Técnicas: Algunas herramientas requieren experiencia avanzada para configurarse y operarse correctamente.
5. Naturaleza de los Activos Analizados
- Redes y Servidores: Escáneres de red como Nmap o Nessus.
- Aplicaciones Web: Herramientas como Burp Suite o OWASP ZAP.
- Dispositivos IoT: Soluciones especializadas en identificar vulnerabilidades en dispositivos no tradicionales.
6. Método de Escaneo
- Activo: Si el sistema puede tolerar tráfico adicional, escaneos activos son ideales para obtener resultados detallados.
- Pasivo: Para sistemas sensibles donde los escaneos activos podrían interrumpir servicios.
7. Priorización de Vulnerabilidades
- Evaluación de Impacto: Escoger herramientas que incluyan clasificación por criticidad (usualmente basada en CVSS).
- Integración con Mitigación: Optar por soluciones que proporcionen recomendaciones prácticas y priorizadas.

## Mejores Prácticas para la Selección
- Definir el Alcance y Objetivos Claramente
  - Determinar qué sistemas, servicios o aplicaciones serán evaluados.
  - Alinear los objetivos de la evaluación con los riesgos y prioridades organizacionales.
- Seleccionar Herramientas Adecuadas
  - Optar por herramientas reconocidas y confiables (ejemplo: Nessus, Qualys).
  - Validar que la herramienta soporte los protocolos y tecnologías usados en la organización.
- Evaluar Periodicidad
  - Implementar escaneos regulares (mensuales, trimestrales) y adicionales tras cambios significativos en la infraestructura.
- Capacitar al Personal
  - Asegurarse de que el equipo tenga conocimientos suficientes para configurar, operar y analizar los resultados de las herramientas seleccionadas.
- Implementar Escaneos en Entornos Controlados
  - Probar las herramientas en entornos de prueba antes de implementarlas en sistemas de producción.
- Asegurar la Integridad de los Resultados
  - Validar los resultados con herramientas o métodos complementarios para reducir falsos positivos o negativos.
- Documentar y Reportar Resultados
  - Generar reportes claros que sean útiles tanto para equipos técnicos como para gerencia.
  - Usar los hallazgos para priorizar las estrategias de remediación.

## Herramientas

- Qualys: Plataforma de evaluación de vulnerabilidades basada en la nube, que analiza activos internos y externos, ofreciendo informes detallados y capacidades de cumplimiento normativo.
- Nessus: Herramienta popular para escaneo de vulnerabilidades que detecta configuraciones inseguras, parches faltantes y problemas de seguridad en redes y hosts.
- GFI LanGuard: Solución de gestión de parches y escaneo de vulnerabilidades que ayuda a identificar riesgos en redes locales, proporcionando informes claros y opciones de corrección.
- OpenVAS: Escáner de vulnerabilidades de código abierto que analiza redes y sistemas para detectar configuraciones inseguras, parches faltantes y fallos conocidos, con soporte para plugins actualizables.
- Nikto: Herramienta de escaneo enfocada en servidores web, que identifica configuraciones débiles, archivos inseguros y vulnerabilidades específicas de aplicaciones web.



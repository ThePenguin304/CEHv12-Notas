# Herramientas de evaluación

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

### Carasterísticas de una buena evaluación de riesgos

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

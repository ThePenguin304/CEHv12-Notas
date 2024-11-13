# 4. Técnicas de enumeración LDAP

## LDAP

`LDAP (Lightweight Directory Access Protocol)` es un protocolo de red utilizado para acceder y gestionar directorios distribuidos de información. Estos directorios suelen contener datos sobre usuarios, equipos, aplicaciones, servicios y otros recursos dentro de una red. LDAP se emplea principalmente para organizar y realizar búsquedas en bases de datos jerárquicas a gran escala.

### Usos comunes de LDAP

- Autenticación centralizada: LDAP permite gestionar las credenciales de los usuarios, facilitando el acceso a múltiples servicios dentro de una red, como correo electrónico, bases de datos o sistemas de archivos.

- Autorización: LDAP ayuda a definir qué recursos pueden ser accedidos por cada usuario en función de sus atributos y permisos.

### Modelo Cliente-Servidor

LDAP se basa en un modelo cliente-servidor. En este modelo, el servidor LDAP alberga la base de datos del directorio, y los clientes LDAP (que pueden ser aplicaciones de usuario, herramientas administrativas o sistemas automatizados) realizan solicitudes para obtener o modificar datos.

- Conexión: El cliente inicia una sesión LDAP conectándose a un agente de sistema de directorio (DSA) a través del puerto TCP 389.
- Solicitud de operación: Una vez conectado, el cliente envía una solicitud de operación al DSA.
- Codificación: La información se transmite entre el cliente y el servidor utilizando las reglas básicas de codificación BER (Basic Encoding Rules).

### Consideraciones de seguridad

Los atacantes pueden utilizar consultas LDAP para obtener información sensible, como nombres de usuario válidos, direcciones de correo electrónico o detalles sobre la estructura organizativa, lo que puede ser útil para llevar a cabo otros tipos de ataques.


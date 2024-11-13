# 4. Técnicas de enumeración LDAP

## 1. LDAP

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

## 2. LDAP Enumeración manual y autónoma

### Manual

1. Comprobar que el servidor LDAP está escuchando en el `port 389 | LDAP` o el `636 |LDAP secure`.
2. Instalación de la librería `ldap3`.
3. Crear un objeto de servidor y realizar una conexión, en caso de puerto 636 añadimos `use_ssl=True`.
4. Realizar operaciones (como obtener usuarios o hacer un ataque de enumeración).


```bash
>>> import ldap3
>>> server = ldap3.Server('Target IP Address', get_info = ldap3.ALL, port =389)
>>> connection = ldap3.Connection(server)
>>> connection.bind() True
>>> server.info
```
Si queremos todos los objetos del directorio:
```bash
>>> connection.search(search_base='DC=DOMAIN,DC=DOMAIN', search_filter='(&(objectClass=*))', search_scope='SUBTREE', attributes='*') True
>> connection.entries
```
El LDAP entero:
```bash
>> connection.search(search_base='DC=DOMAIN,DC=DOMAIN', search_filter='(&(objectClass=person))', search_scope='SUBTREE', attributes='userPassword')
True >>> connection.entries
```

Ejemplo de script completo para enumeración LDAP
```bash
from ldap3 import Server, Connection, ALL

# Datos del servidor y usuario
target_ip = "<Target IP>"
user_dn = "cn=admin,dc=example,dc=com"  # Nombre de usuario completo (DN)
password = "<password>"  # Contraseña

# Paso 1: Crear el objeto de servidor para una conexión LDAP
server = Server(target_ip, get_info=ALL)

# Paso 2: Crear la conexión (puerto 389)
conn = Connection(server, user=user_dn, password=password, auto_bind=True)

# Paso 3: Realizar la búsqueda para enumerar usuarios (por ejemplo, buscando todos los objetos tipo 'person')
conn.search('dc=example,dc=com', '(objectClass=person)', attributes=['uid', 'cn', 'sn'])

# Paso 4: Mostrar los resultados de la búsqueda
print(conn.entries)
```



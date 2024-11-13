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
### Autónomo

`ldap-brute NSE script` los atacantes usan el NSE de nmap para realizar ataques de fuerza bruta a LDAP.

```bash
nmap -p 389 --script ldap-base='"cn=users,dc=CEH,dc=com" ldap-brute' <Target IP Address>

-p 389: Escanea el puerto 389 (LDAP).
--script ldap-base: Usa el script ldap-base de Nmap.
'cn=users,dc=CEH,dc=com': Especifica el DN (Distinguished Name) de base donde se encuentran los usuarios que el script intentará enumerar.
ldap-brute: Especifica el script de Nmap que realiza un ataque de fuerza bruta para identificar usuarios válidos en el servidor LDAP.
<Target IP Address>: La dirección IP del servidor objetivo.
```

Ejemplo de resultado de la ejecución:
```bash
PORT    STATE SERVICE
389/tcp open  ldap
| ldap-base: 
|   Base DN: cn=users,dc=CEH,dc=com
|   ldap-brute: 
|     user: testuser1
|     user: testuser2
|     user: administrator
|     user: guest
|   Bruteforce successful: testuser1 (password: test123)
|_  Bruteforce unsuccessful: guest

- PORT 389/tcp open ldap: El puerto 389 está abierto y el servicio LDAP está en ejecución.
- ldap-base: El script consulta el base DN configurado (cn=users,dc=CEH,dc=com) para buscar usuarios.
- ldap-brute: El script intenta un ataque de fuerza bruta para adivinar los nombres de usuario. En la simulación, se enumeran los siguientes usuarios: testuser1, testuser2, administrator, y guest.
- Bruteforce successful: El ataque de fuerza bruta tuvo éxito para testuser1, encontrando que la contraseña es test123.
- Bruteforce unsuccessful: El ataque no tuvo éxito con el usuario guest, lo que significa que no se encontró una contraseña válida.
```

## 3. LDAP herramientas enumeración 

Los atacantes utilizan herramientas específicas para interactuar con servidores LDAP y obtener información valiosa como nombres de usuario válidos, direcciones de correo electrónico, detalles de departamentos, y más.

### Softerra LDAP Administrator

`Softerra LDAP Administrator` Herramienta GUI potente para la administración de servidores LDAP, útil para enumerar usuarios, grupos y obtener detalles de la estructura organizativa.

### ldapsearch

`ldapsearch` Herramienta de línea de comandos utilizada para realizar búsquedas en un servidor LDAP. Está disponible en sistemas Unix/Linux y forma parte de las utilidades estándar del paquete de cliente LDAP. Es muy útil para realizar consultas en un directorio LDAP y enumerar información valiosa sobre usuarios y grupos.

Es accesible usando la libreria `ldap_search_ext(3)`

Enumeración de usuarios:
```bash
ldapsearch -x -h 192.168.1.10 -b "dc=example,dc=com" "(objectClass=person)"

`-h` <Target IP Address>: Especifica la dirección IP del servidor LDAP al que te quieres conectar.
`-x:` Usa un modo de autenticación simple (sin SASL).
`-s` base: Realiza una búsqueda de base, lo que significa que solo se consulta la base del directorio LDAP, sin hacer búsqueda recursiva.
```

Buscar todos los grupos:
```bash
ldapsearch -x -h 192.168.1.10 -b "dc=example,dc=com" "(objectClass=group)"
```

Obtener los contextos de nomenclatura (namingcontexts):
```bash
ldapsearch -h <Target IP Address> -x -s base namingcontexts
- `namingcontexts:` Recupera los contextos de nomenclatura, que indican las bases principales del directorio LDAP (en otras palabras, los dominios o unidades que se pueden buscar).
```
Obtener más información sobre el dominio principal:
```bash
ldapsearch -h <Target IP Address> -x -b "DC=htb,DC=local"

-b "DC=htb,DC=local": Especifica la base DN (Distinguished Name), que en este caso es DC=htb,DC=local. Esto indica que la búsqueda se realizará dentro del dominio htb.local.
Este comando devolverá una lista de entradas en el directorio bajo el dominio htb.local.
```

# 4. Técnicas de enumeración LDAP

Redirección a [Información General](https://github.com/ThePenguin304/CEHv12-Notas/blob/main/Modulos/Modulo%204/README.md)

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

---

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

---

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

--- 

## BONUS TEST

1. ¿Cuál de los siguientes comandos de `ldapsearch` se usa para obtener la lista de todos los objetos dentro de un directorio LDAP sin aplicar filtros?

- A) `ldapsearch -x -b "dc=example,dc=com" "(objectClass=*)" `
- B) `ldapsearch -x -b "dc=example,dc=com" "(objectClass=user)" `
- C) `ldapsearch -x -b "dc=example,dc=com" "(cn=*)" `
- D) `ldapsearch -x -b "dc=example,dc=com" "(objectClass=groupOfNames)" `

**Respuesta correcta:** A) `ldapsearch -x -b "dc=example,dc=com" "(objectClass=*)"`

---

2. ¿Qué puerto se utiliza típicamente para la comunicación LDAP segura (LDAPS)?

- A) 389
- B) 636
- C) 8080
- D) 445

**Respuesta correcta:** B) 636

---

3. ¿Cuál de las siguientes opciones describe correctamente el propósito de la cadena de comunidad "private" en un servidor LDAP?

- A) Permite acceder solo a información pública del directorio.
- B) Permite acceder y modificar la configuración del directorio.
- C) Solo permite ver la estructura jerárquica del directorio.
- D) Bloquea el acceso a los datos en el servidor LDAP.

**Respuesta correcta:** B) Permite acceder y modificar la configuración del directorio.

---

4. ¿Qué tipo de autenticación LDAP se utiliza al realizar una consulta `ldapsearch -x` sin especificar un mecanismo de autenticación explícito?

- A) SASL (Simple Authentication and Security Layer)
- B) Autenticación anónima
- C) Autenticación basada en contraseñas
- D) Autenticación Kerberos

**Respuesta correcta:** B) Autenticación anónima

---

5. En un ataque de enumeración LDAP, ¿qué información puede obtener un atacante al realizar una consulta sobre `sAMAccountName`?

- A) El nombre del servidor LDAP.
- B) La lista de todos los grupos en el directorio.
- C) El nombre de cuenta de usuario de Windows.
- D) El número de teléfono de los empleados.

**Respuesta correcta:** C) El nombre de cuenta de usuario de Windows.

---

6. ¿Cuál de los siguientes métodos se utiliza para evitar que los usuarios realicen enumeración LDAP en un servidor?

- A) Habilitar el filtro de `objectClass`.
- B) Deshabilitar la autenticación LDAP.
- C) Implementar autenticación basada en certificados.
- D) Configurar firewalls para bloquear los puertos 389 y 636.

**Respuesta correcta:** D) Configurar firewalls para bloquear los puertos 389 y 636.

---

7. ¿Qué comando LDAP permitiría enumerar los usuarios que son parte del grupo "Sales" en un servidor LDAP?

- A) `ldapsearch -x -b "dc=example,dc=com" "(objectClass=user)"`
- B) `ldapsearch -x -b "dc=example,dc=com" "(cn=Sales)" sAMAccountName`
- C) `ldapsearch -x -b "dc=example,dc=com" "(objectClass=groupOfNames)"`
- D) `ldapsearch -x -b "dc=example,dc=com" "(cn=Sales)"`

**Respuesta correcta:** B) `ldapsearch -x -b "dc=example,dc=com" "(cn=Sales)" sAMAccountName`

---

8. ¿Qué tipo de ataque LDAP permitiría a un atacante obtener información sensible como nombres de usuario, direcciones y detalles departamentales?

- A) Man-in-the-middle
- B) Denegación de servicio
- C) Enumeración LDAP
- D) Inyección LDAP

**Respuesta correcta:** C) Enumeración LDAP

---

9. ¿Qué comando de `nmap` se utiliza para realizar un escaneo de puertos LDAP en busca de vulnerabilidades conocidas?

- A) `nmap -p 389,636 --script ldap-brute <IP>`
- B) `nmap -p 443 --script ldap-enum <IP>`
- C) `nmap -p 389 --script ldap-check <IP>`
- D) `nmap -p 389 --script ldap-dump <IP>`

**Respuesta correcta:** A) `nmap -p 389,636 --script ldap-brute <IP>`

---

10. ¿Cuál de los siguientes comandos LDAP puede usarse para buscar todos los usuarios dentro de un contenedor específico como `ou=Users,dc=example,dc=com`?

- A) `ldapsearch -x -b "ou=Users,dc=example,dc=com" "(objectClass=person)"`
- B) `ldapsearch -x -b "dc=example,dc=com" "(cn=*)" `
- C) `ldapsearch -x -b "ou=Users,dc=example,dc=com" "(objectClass=user)"`
- D) `ldapsearch -x -b "ou=Users,dc=example,dc=com" "(uid=*)" `

**Respuesta correcta:** C) `ldapsearch -x -b "ou=Users,dc=example,dc=com" "(objectClass=user)"`

---

11. ¿Qué comando de `nmap` se utilizaría para identificar la versión de un servidor LDAP de manera detallada?

- A) `nmap -p 389 --script ldap-enum-info <IP>`
- B) `nmap -p 389 --script ldap-info <IP>`
- C) `nmap -p 389 --script ldap-brute <IP>`
- D) `nmap -p 636 --script ldap-version <IP>`

**Respuesta correcta:** B) `nmap -p 389 --script ldap-info <IP>`

---

12. ¿Qué objeto LDAP se utilizaría típicamente para almacenar información relacionada con un empleado en una red corporativa?

- A) `groupOfNames`
- B) `person`
- C) `organizationalUnit`
- D) `user`

**Respuesta correcta:** D) `user`

---

13. ¿Cuál de los siguientes es un riesgo asociado con la habilitación de la cadena de comunidad "public" en un servidor LDAP?

- A) Los atacantes pueden modificar la configuración del servidor.
- B) Los atacantes pueden obtener información sensible sobre la red y los usuarios.
- C) Los atacantes pueden detener el servidor LDAP.
- D) Los atacantes pueden realizar ataques de denegación de servicio.

**Respuesta correcta:** B) Los atacantes pueden obtener información sensible sobre la red y los usuarios.

---

14. ¿Qué atributo LDAP es comúnmente utilizado para almacenar la dirección de correo electrónico de un usuario?

- A) `uid`
- B) `mail`
- C) `cn`
- D) `sn`

**Respuesta correcta:** B) `mail`

---

15. ¿Qué comando de `ldapsearch` permite cambiar la información de contacto del sistema (`sysContact`) en un servidor LDAP?

- A) `ldapsearch -x -b "dc=example,dc=com" "sysContact=NewContact"`
- B) `ldapsearch -x -b "dc=example,dc=com" "sysContact=NewContact" -D "cn=admin"`
- C) `ldapmodify -x -D "cn=admin,dc=example,dc=com" -w password -f sysContact.ldif`
- D) `ldapadd -x -b "dc=example,dc=com" "sysContact=NewContact"`

**Respuesta correcta:** C) `ldapmodify -x -D "cn=admin,dc=example,dc=com" -w password -f sysContact.ldif`

---

16. ¿Qué función proporciona el comando `ldapmodify`?

- A) Modificar la configuración de la cadena de comunidad.
- B) Realizar consultas sobre la base de datos LDAP.
- C) Modificar entradas en un servidor LDAP.
- D) Enumerar los usuarios en el directorio LDAP.

**Respuesta correcta:** C) Modificar entradas en un servidor LDAP.

---

17. ¿Qué herramienta se puede utilizar para realizar ataques de fuerza bruta contra las credenciales de un servidor LDAP?

- A) `Hydra`
- B) `Nikto`
- C) `Nmap`
- D) `Metasploit`

**Respuesta correcta:** A) `Hydra`

---

18. ¿Cuál es la principal diferencia entre el puerto 389 y el puerto 636 en LDAP?

- A) El puerto 389 es usado para conexiones no seguras, mientras que el puerto 636 es para conexiones seguras mediante SSL/TLS.
- B) El puerto 389 es para la autenticación, y el puerto 636 para la enumeración.
- C) El puerto 389 es utilizado solo para consultas, mientras que el puerto 636 solo para la modificación de entradas.
- D) No hay diferencia entre ambos puertos.

**Respuesta correcta:** A) El puerto 389 es usado para conexiones no seguras, mientras que el puerto 636 es para conexiones seguras mediante SSL/TLS.

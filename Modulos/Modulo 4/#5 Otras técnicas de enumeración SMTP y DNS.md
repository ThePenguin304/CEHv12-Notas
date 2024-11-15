# Enumeración IPsec, VoIP, RPC, Unix/Linux, Telnet, FTP, TFTP, SMB, IPv6, y BGP

## Enumeración IPsec

IPsec (Internet Protocol Security) es un conjunto de protocolos diseñados para proporcionar seguridad en las comunicaciones sobre redes IP, como Internet. IPsec protege los datos transmitidos mediante mecanismos de cifrado, autenticación y verificación de integridad, lo que asegura que las comunicaciones sean confidenciales y estén protegidas contra alteraciones o accesos no autorizados.

Componentes Clave de IPsec
- Protocolo AH (Authentication Header). Proporciona autenticación e integridad, pero no cifrado. Protocolo IP 51.
- Protocolo ESP (Encapsulating Security Payload). Proporciona cifrado, autenticación e integridad. Protocolo IP 50.
- IKE (Internet Key Exchange). Protocolo utilizado para establecer y gestionar asociaciones de seguridad (SA), que definen cómo se protegerán los datos. Versiones: IKEv1 (obsoleto) e IKEv2 (moderno) `Puerto 500`.

Modos de Funcionamiento
- Modo Transporte. Protege solo la carga útil (datos) del paquete IP, dejando intacto el encabezado IP. Se utiliza típicamente en comunicaciones extremo a extremo, como entre dos hosts.
- Modo Túnel. Protege el paquete IP completo, incluyendo el encabezado, encapsulándolo en un nuevo paquete IP. Se usa principalmente para conexiones VPN (Virtual Private Network), como entre gateways o firewalls.

La **enumeración de IPsec (Internet Protocol Security)** se refiere a la recopilación de información sobre configuraciones y características de IPsec en un sistema objetivo.

La mayoría de las VPN basadas en IPsec utilizan el Protocolo de administración de claves de la Asociación de seguridad de Internet (ISAKMP), una parte de IKE, para establecer, negociar, modificar y eliminar asociaciones de seguridad (SA) y claves criptográficas en un entorno VPN.

```bash
nmap -sU -p 500,4500 <IP objetivo>
```

Ike-scan. Escaneo y descubrimiento de configuraciones IKE.

```bash
ike-scan –M <target gateway IP address>
```

- Algoritmos de cifrado compatibles.
- Métodos de autenticación soportados.

## Enumeración VolIP

Usa SIP (Sesion Initation Protocol) permite llamadas por voz y video por IP.

SIP usa los puertos 2000, 2001, 5060 y 5061.


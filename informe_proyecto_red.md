# INFORME FINAL DEL PROYECTO DE RED
## Diseño, implementación y validación de una topología segmentada en Cisco Packet Tracer

**Estudiante:** [Escribe tu nombre]  
**Matrícula:** [Escribe tu matrícula si aplica]  
**Asignatura:** [Nombre de la asignatura]  
**Docente:** [Nombre del docente]  
**Institución:** [Nombre de la institución]  
**Fecha:** [Fecha de entrega]  

---

# Índice

1. [Introducción](#1-introducción)  
2. [Objetivos](#2-objetivos)  
   1. [Objetivo general](#21-objetivo-general)  
   2. [Objetivos específicos](#22-objetivos-específicos)  
3. [Desarrollo del proyecto](#3-desarrollo-del-proyecto)  
   1. [Planificación del direccionamiento IP](#31-planificación-del-direccionamiento-ip)  
   2. [Topología física y conexiones](#32-topología-física-y-conexiones)  
   3. [Configuración de routers](#33-configuración-de-routers)  
   4. [Configuración de switches](#34-configuración-de-switches)  
   5. [Configuración de DHCP](#35-configuración-de-dhcp)  
   6. [Configuración de servicios](#36-configuración-de-servicios)  
   7. [Validación y pruebas](#37-validación-y-pruebas)  
4. [Resultados](#4-resultados)  
5. [Dificultades presentadas](#5-dificultades-presentadas)  
6. [Conclusión](#6-conclusión)  
7. [Recomendaciones](#7-recomendaciones)  

---

# 1. Introducción

El presente informe describe el diseño, configuración, implementación y validación de una topología de red desarrollada en Cisco Packet Tracer, estructurada en dos zonas principales denominadas **Oeste** y **Este**. La red fue construida con el propósito de integrar múltiples segmentos lógicos mediante VLANs, aplicar direccionamiento eficiente utilizando VLSM, permitir el enrutamiento entre subredes mediante RIP versión 2, automatizar la asignación de direcciones IP a través de DHCP y proporcionar servicios de red como DNS, WEB, correo electrónico, FTP y acceso simulado a Internet.

Durante el desarrollo del proyecto se realizaron configuraciones en routers, switches, servidores y equipos finales, tomando en cuenta tanto la topología física como la lógica de la red. Asimismo, se implementaron mecanismos de administración remota segura mediante SSH, se verificó la conectividad entre zonas y se validó el funcionamiento de todos los servicios configurados.

---

# 2. Objetivos

## 2.1 Objetivo general

Diseñar e implementar una red segmentada y funcional en Cisco Packet Tracer, capaz de interconectar múltiples VLANs y subredes entre dos zonas geográficas simuladas, incorporando servicios de red esenciales y garantizando conectividad completa.

## 2.2 Objetivos específicos

- Planificar el direccionamiento IP de la topología mediante VLSM.
- Segmentar la red utilizando VLANs en los switches.
- Configurar enlaces troncales para el transporte de múltiples VLANs.
- Implementar subinterfaces en routers para el enrutamiento inter-VLAN.
- Configurar el protocolo de enrutamiento dinámico RIP v2.
- Automatizar la asignación de direcciones IP con DHCP local y remoto.
- Implementar servicios de red como DNS, WEB, MAIL y FTP.
- Simular la salida a Internet mediante un router externo y un servidor Google.
- Habilitar acceso administrativo seguro mediante SSH.
- Realizar pruebas integrales para validar el funcionamiento total de la red.

---

# 3. Desarrollo del proyecto

## 3.1 Planificación del direccionamiento IP

Para el desarrollo del proyecto se realizó una planificación de direccionamiento usando VLSM, con el fin de optimizar el uso de direcciones IP y asignar una red específica a cada VLAN, a los enlaces seriales y a los segmentos de servidores.

### 3.1.1 VLANs de la zona Oeste

| VLAN | ID | Red | Máscara | Gateway |
|------|----|-----|---------|---------|
| VLAN_A | 10 | 192.168.20.0 | /25 | 192.168.20.1 |
| VLAN_B | 20 | 192.168.40.0 | /27 | 192.168.40.1 |
| VLAN_C | 30 | 192.168.10.0 | /24 | 192.168.10.1 |
| VLAN_D | 40 | 192.168.30.0 | /26 | 192.168.30.1 |

### 3.1.2 VLANs de la zona Este

| VLAN | ID | Red | Máscara | Gateway |
|------|----|-----|---------|---------|
| VLAN_M | 50 | 172.16.10.0 | /24 | 172.16.10.1 |
| VLAN_N | 60 | 172.16.20.0 | /24 | 172.16.20.1 |
| VLAN_O | 70 | 172.16.30.0 | /25 | 172.16.30.1 |
| VLAN_P | 80 | 172.16.40.0 | /27 | 172.16.40.1 |

### 3.1.3 Redes de servicios

| Servicio | Dirección IP | Máscara | Gateway |
|----------|--------------|---------|---------|
| WEB | 192.168.50.10 | 255.255.255.0 | 192.168.50.1 |
| DNS | 192.168.50.20 | 255.255.255.0 | 192.168.50.1 |
| MAIL | 192.168.50.30 | 255.255.255.0 | 192.168.50.1 |
| DHCP_OESTE | 192.168.60.10 | 255.255.255.0 | 192.168.60.1 |
| DHCP_ESTE | 192.168.70.10 | 255.255.255.0 | 192.168.70.1 |
| FTP | 192.168.80.10 | 255.255.255.0 | 192.168.80.1 |
| Admin_PC | 192.168.80.20 | 255.255.255.0 | 192.168.80.1 |
| GOOGLE_SERVER | 200.0.0.10 | 255.255.255.0 | 200.0.0.1 |

---

## 3.2 Topología física y conexiones

La topología fue estructurada en dos zonas, Oeste y Este, unidas por enlaces seriales entre routers. Cada zona contiene routers, switches, hosts y servidores específicos.

### 3.2.1 Conexiones de la zona Oeste

- `R1_OESTE G0/0` conectado a `SW1_OESTE Fa0/1`
- `SW1_OESTE Fa0/5` conectado a `SW2_OESTE Fa0/1`
- `SW1_OESTE Fa0/11` conectado a `VLAN_A_OESTE`
- `SW2_OESTE Fa0/11` conectado a `VLAN_B_OESTE`

- `R4_OESTE G0/1` conectado a `SW4_OESTE Fa0/1`
- `SW4_OESTE Fa0/5` conectado a `SW3_OESTE Fa0/1`
- `SW4_OESTE Fa0/11` conectado a `VLAN_D_OESTE`
- `SW3_OESTE Fa0/11` conectado a `VLAN_C_OESTE`

- `R2_OESTE G0/0` conectado a `SW_SERVIDORES`
- `SW_SERVIDORES` conectado a WEB, DNS y MAIL
- `R4_OESTE G0/0` conectado a DHCP_OESTE

### 3.2.2 Conexiones de la zona Este

- `R4_ESTE G0/0` conectado a `SW1_ESTE Fa0/1`
- `SW1_ESTE Fa0/5` conectado a `SW2_ESTE`
- `SW1_ESTE Fa0/11` conectado a `VLAN_M_ESTE`
- `SW2_ESTE Fa0/11` conectado a `VLAN_N_ESTE`

- `R2_ESTE G0/0` conectado a `SW5_ESTE Fa0/1`
- `SW5_ESTE Fa0/5` conectado a `SW4_ESTE`
- `SW5_ESTE Fa0/11` conectado a `VLAN_O_ESTE`
- `SW4_ESTE Fa0/11` conectado a `VLAN_P_ESTE`

- `R3_ESTE G0/0` conectado a `SW3_ESTE`
- `SW3_ESTE` conectado a FTP y Admin_PC
- `R4_ESTE G0/1` conectado a DHCP_ESTE

### 3.2.3 Conexiones WAN

- `R1_OESTE ↔ R2_OESTE`
- `R1_OESTE ↔ R4_OESTE`
- `R2_OESTE ↔ R3_OESTE`
- `R2_OESTE ↔ R4_OESTE`
- `R3_OESTE ↔ R4_OESTE`
- `R3_OESTE ↔ R1_ESTE`
- `R1_ESTE ↔ R2_ESTE`
- `R1_ESTE ↔ R4_ESTE`
- `R2_ESTE ↔ R3_ESTE`
- `R2_ESTE ↔ R4_ESTE`
- `R3_ESTE ↔ R4_ESTE`
- `R1_ESTE ↔ INTERNET`
- `INTERNET G0/0 ↔ GOOGLE_SERVER`

---

## 3.3 Configuración de routers

En los routers se configuraron parámetros básicos, interfaces LAN y WAN, subinterfaces, RIP v2, SSH y rutas estáticas donde correspondía.

### 3.3.1 Configuración básica realizada

- Asignación de hostname
- Desactivación de `ip domain-lookup`
- Configuración del dominio `redeslannoche.com`
- Creación del usuario local `admin`
- Configuración de SSH versión 2
- Generación de claves RSA
- Activación de interfaces con `no shutdown`
- Configuración de `clock rate` en interfaces DCE

### 3.3.2 Parámetros de acceso usados

- Usuario SSH: `admin`
- Contraseña: `Admin123!`
- Dominio: `redeslannoche.com`

### 3.3.3 Ejemplo de configuración básica

```bash
enable
configure terminal
hostname R1_OESTE
no ip domain-lookup
ip domain-name redeslannoche.com
username admin secret Admin123!
crypto key generate rsa
1024
ip ssh version 2
line vty 0 4
login local
transport input ssh
exit
```

### 3.3.4 Ejemplo de interfaz serial

```bash
interface s0/0/0
description HACIA_R2_OESTE
ip address 10.0.0.1 255.255.255.252
clock rate 64000
no shutdown
exit
```

### 3.3.5 Ejemplo de subinterfaces

```bash
interface g0/0
no ip address
no shutdown
exit

interface g0/0.10
encapsulation dot1Q 10
ip address 192.168.20.1 255.255.255.128
exit

interface g0/0.20
encapsulation dot1Q 20
ip address 192.168.40.1 255.255.255.224
exit
```

### 3.3.6 Configuración de RIP v2

```bash
router rip
version 2
no auto-summary
network 10.0.0.0
network 192.168.20.0
network 192.168.40.0
exit
```

### 3.3.7 Ruta por defecto en R1_ESTE

```bash
ip route 0.0.0.0 0.0.0.0 10.0.0.34
```

---

## 3.4 Configuración de switches

Los switches fueron configurados para soportar la segmentación por VLAN, el transporte por enlaces troncales y la administración remota.

### 3.4.1 Tareas realizadas

- Configuración de hostname
- Creación de VLANs
- Asignación de puertos access
- Configuración de puertos trunk
- Configuración de IP de administración
- Configuración de default-gateway
- Habilitación de SSH

### 3.4.2 Ejemplo de VLAN

```bash
vlan 10
name VLAN_A
exit

vlan 20
name VLAN_B
exit
```

### 3.4.3 Ejemplo de puerto access

```bash
interface fa0/11
description PC_VLAN_A_OESTE
switchport mode access
switchport access vlan 10
spanning-tree portfast
exit
```

### 3.4.4 Ejemplo de puerto trunk

```bash
interface fa0/1
description TRUNK_HACIA_R1_OESTE
switchport mode trunk
exit
```

### 3.4.5 Ejemplo de administración

```bash
interface vlan 10
ip address 192.168.20.2 255.255.255.128
no shutdown
exit

ip default-gateway 192.168.20.1
```

---

## 3.5 Configuración de DHCP

Se implementó un esquema mixto de DHCP local y remoto.

### 3.5.1 DHCP en routers

- `R1_OESTE`: VLAN_A y VLAN_B
- `R2_ESTE`: VLAN_O y VLAN_P

Ejemplo:

```bash
ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp pool VLAN_A_OESTE
network 192.168.20.0 255.255.255.128
default-router 192.168.20.1
dns-server 192.168.50.20
exit
```

### 3.5.2 DHCP en servidores

- `DHCP_OESTE`: VLAN_C y VLAN_D
- `DHCP_ESTE`: VLAN_M y VLAN_N

### 3.5.3 Reenvío DHCP

Ejemplo:

```bash
interface g0/1.30
ip helper-address 192.168.60.10
exit
```

---

## 3.6 Configuración de servicios

### 3.6.1 DNS

Servidor configurado en `192.168.50.20`.

Registros:
- `www.redeslannoche.com` → `192.168.50.10`
- `mail.redeslannoche.com` → `192.168.50.30`
- `ftp.redeslannoche.com` → `192.168.80.10`
- `www.google.com` → `200.0.0.10`

### 3.6.2 WEB

Servidor configurado en `192.168.50.10` con HTTP activo.

### 3.6.3 MAIL

Servidor configurado en `192.168.50.30` con SMTP y POP3, utilizando el dominio `redeslannoche.com`.

### 3.6.4 FTP

Servidor configurado en `192.168.80.10` con servicio FTP habilitado.

### 3.6.5 INTERNET

Router configurado con:
- `S0/0/0` hacia `R1_ESTE`
- `G0/0` hacia `GOOGLE_SERVER`

---

## 3.7 Validación y pruebas

Para verificar el funcionamiento del proyecto se realizaron pruebas de conectividad y servicios.

### 3.7.1 Comandos de verificación

En routers:
```bash
show ip interface brief
show ip route
show ip protocols
```

En switches:
```bash
show vlan brief
show interfaces trunk
```

En hosts:
```bash
ipconfig
ping
```

### 3.7.2 Pruebas realizadas

- Obtención de direcciones IP por DHCP
- Ping al gateway de cada VLAN
- Ping entre Oeste y Este
- Acceso al servidor WEB
- Resolución DNS
- Prueba de correo electrónico
- Prueba de acceso FTP
- Acceso SSH a dispositivos de red
- Acceso a `www.google.com`

---

# 4. Resultados

La topología implementada alcanzó conectividad total entre las redes de las zonas Oeste y Este. Se verificó el correcto funcionamiento de las VLANs, el enrutamiento dinámico mediante RIP v2, la asignación automática de direcciones IP con DHCP, la resolución de nombres mediante DNS, el acceso a los servicios WEB, MAIL y FTP, y la salida hacia internet simulado mediante el servidor Google.

También se comprobó el acceso administrativo seguro mediante SSH en routers y switches, consolidando un entorno de red funcional y seguro.

---

# 5. Dificultades presentadas

Durante la implementación surgieron dificultades relacionadas con la interpretación de la topología física, la asociación correcta de VLANs entre las zonas, la configuración de enlaces troncales y la propagación inicial de rutas entre Oeste y Este. Estas situaciones fueron corregidas mediante verificación de conexiones físicas, revisión de puertos, análisis de trunks, validación de subinterfaces y comprobación de tablas de enrutamiento.

---

# 6. Conclusión

El desarrollo de este proyecto permitió integrar conocimientos de subnetting, VLANs, switching, routing dinámico, DHCP, DNS y servicios de red en una topología funcional dentro de Cisco Packet Tracer. La implementación demostró la importancia de la planificación del direccionamiento IP, la identificación correcta de los enlaces físicos y la validación progresiva de cada servicio.

Como resultado final se obtuvo una red estable, segmentada, administrable y completamente operativa, con conectividad interna y externa simulada, cumpliendo con los objetivos planteados.

---

# 7. Recomendaciones

- Guardar la configuración final en todos los equipos.
- Documentar siempre direcciones IP, puertos y enlaces.
- Verificar la topología física antes de configurar interfaces.
- Realizar pruebas por capas: VLAN, routing, DHCP y servicios.
- Mantener capturas de evidencia para sustentar la entrega final.

# CONFIGURACIÓN COMPLETA DE LOS DEMÁS DISPOSITIVOS

Este documento reúne la configuración general de switches, servidores y equipos finales utilizados en el proyecto. Incluye VLANs, trunks, administración, DHCP en servidores, DNS, WEB, MAIL, FTP, GOOGLE_SERVER y parámetros de hosts.

---

## 1. Parámetros generales

- Dominio usado: `redeslannoche.com`
- Usuario SSH para dispositivos administrables: `admin`
- Clave SSH: `Admin123!`
- DNS principal de la red: `192.168.50.20`

---

## 2. Configuración completa de switches

### 2.1 Configuración base para switches administrables

```bash
enable
configure terminal
hostname NOMBRE_DEL_SWITCH
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

---

### 2.2 SW1_OESTE

#### Función
- Switch principal de acceso para VLAN_A y enlace troncal hacia SW2_OESTE y R1_OESTE

```bash
enable
configure terminal
hostname SW1_OESTE
vlan 10
name VLAN_A
exit
vlan 20
name VLAN_B
exit

interface fa0/1
description TRUNK_HACIA_R1_OESTE
switchport mode trunk
exit

interface fa0/5
description TRUNK_HACIA_SW2_OESTE
switchport mode trunk
exit

interface fa0/11
description PC_VLAN_A_OESTE
switchport mode access
switchport access vlan 10
spanning-tree portfast
exit

interface vlan 10
ip address 192.168.20.2 255.255.255.128
no shutdown
exit

ip default-gateway 192.168.20.1
ip domain-name redeslannoche.com
username admin secret Admin123!
crypto key generate rsa
1024
ip ssh version 2
line vty 0 4
login local
transport input ssh
exit
end
write memory
```

---

### 2.3 SW2_OESTE

#### Función
- Switch secundario de acceso para VLAN_B

```bash
enable
configure terminal
hostname SW2_OESTE
vlan 20
name VLAN_B
exit

interface fa0/1
description TRUNK_HACIA_SW1_OESTE
switchport mode trunk
exit

interface fa0/11
description PC_VLAN_B_OESTE
switchport mode access
switchport access vlan 20
spanning-tree portfast
exit

interface vlan 20
ip address 192.168.40.2 255.255.255.224
no shutdown
exit

ip default-gateway 192.168.40.1
ip domain-name redeslannoche.com
username admin secret Admin123!
crypto key generate rsa
1024
ip ssh version 2
line vty 0 4
login local
transport input ssh
exit
end
write memory
```

---

### 2.4 SW3_OESTE

#### Función
- Switch de acceso para VLAN_C

```bash
enable
configure terminal
hostname SW3_OESTE
vlan 30
name VLAN_C
exit

interface fa0/1
description TRUNK_HACIA_SW4_OESTE
switchport mode trunk
exit

interface fa0/11
description PC_VLAN_C_OESTE
switchport mode access
switchport access vlan 30
spanning-tree portfast
exit

interface vlan 30
ip address 192.168.10.2 255.255.255.0
no shutdown
exit

ip default-gateway 192.168.10.1
ip domain-name redeslannoche.com
username admin secret Admin123!
crypto key generate rsa
1024
ip ssh version 2
line vty 0 4
login local
transport input ssh
exit
end
write memory
```

---

### 2.5 SW4_OESTE

#### Función
- Switch principal para VLAN_C y VLAN_D, con enlace troncal hacia R4_OESTE y SW3_OESTE

```bash
enable
configure terminal
hostname SW4_OESTE
vlan 30
name VLAN_C
exit
vlan 40
name VLAN_D
exit

interface fa0/1
description TRUNK_HACIA_R4_OESTE
switchport mode trunk
exit

interface fa0/5
description TRUNK_HACIA_SW3_OESTE
switchport mode trunk
exit

interface fa0/11
description PC_VLAN_D_OESTE
switchport mode access
switchport access vlan 40
spanning-tree portfast
exit

interface vlan 40
ip address 192.168.30.2 255.255.255.192
no shutdown
exit

ip default-gateway 192.168.30.1
ip domain-name redeslannoche.com
username admin secret Admin123!
crypto key generate rsa
1024
ip ssh version 2
line vty 0 4
login local
transport input ssh
exit
end
write memory
```

---

### 2.6 SW1_ESTE

#### Función
- Switch principal para VLAN_M y enlace a SW2_ESTE y R4_ESTE

```bash
enable
configure terminal
hostname SW1_ESTE
vlan 50
name VLAN_M
exit
vlan 60
name VLAN_N
exit

interface fa0/1
description TRUNK_HACIA_R4_ESTE
switchport mode trunk
exit

interface fa0/5
description TRUNK_HACIA_SW2_ESTE
switchport mode trunk
exit

interface fa0/11
description PC_VLAN_M_ESTE
switchport mode access
switchport access vlan 50
spanning-tree portfast
exit

interface vlan 50
ip address 172.16.10.2 255.255.255.0
no shutdown
exit

ip default-gateway 172.16.10.1
ip domain-name redeslannoche.com
username admin secret Admin123!
crypto key generate rsa
1024
ip ssh version 2
line vty 0 4
login local
transport input ssh
exit
end
write memory
```

---

### 2.7 SW2_ESTE

#### Función
- Switch secundario para VLAN_N

```bash
enable
configure terminal
hostname SW2_ESTE
vlan 60
name VLAN_N
exit

interface fa0/1
description TRUNK_HACIA_SW1_ESTE
switchport mode trunk
exit

interface fa0/11
description PC_VLAN_N_ESTE
switchport mode access
switchport access vlan 60
spanning-tree portfast
exit

interface vlan 60
ip address 172.16.20.2 255.255.255.0
no shutdown
exit

ip default-gateway 172.16.20.1
ip domain-name redeslannoche.com
username admin secret Admin123!
crypto key generate rsa
1024
ip ssh version 2
line vty 0 4
login local
transport input ssh
exit
end
write memory
```

---

### 2.8 SW4_ESTE

#### Función
- Switch secundario para VLAN_P

```bash
enable
configure terminal
hostname SW4_ESTE
vlan 80
name VLAN_P
exit

interface fa0/1
description TRUNK_HACIA_SW5_ESTE
switchport mode trunk
exit

interface fa0/11
description PC_VLAN_P_ESTE
switchport mode access
switchport access vlan 80
spanning-tree portfast
exit

interface vlan 80
ip address 172.16.40.2 255.255.255.224
no shutdown
exit

ip default-gateway 172.16.40.1
ip domain-name redeslannoche.com
username admin secret Admin123!
crypto key generate rsa
1024
ip ssh version 2
line vty 0 4
login local
transport input ssh
exit
end
write memory
```

---

### 2.9 SW5_ESTE

#### Función
- Switch principal para VLAN_O y enlace a SW4_ESTE y R2_ESTE

```bash
enable
configure terminal
hostname SW5_ESTE
vlan 70
name VLAN_O
exit
vlan 80
name VLAN_P
exit

interface fa0/1
description TRUNK_HACIA_R2_ESTE
switchport mode trunk
exit

interface fa0/5
description TRUNK_HACIA_SW4_ESTE
switchport mode trunk
exit

interface fa0/11
description PC_VLAN_O_ESTE
switchport mode access
switchport access vlan 70
spanning-tree portfast
exit

interface vlan 70
ip address 172.16.30.2 255.255.255.128
no shutdown
exit

ip default-gateway 172.16.30.1
ip domain-name redeslannoche.com
username admin secret Admin123!
crypto key generate rsa
1024
ip ssh version 2
line vty 0 4
login local
transport input ssh
exit
end
write memory
```

---

### 2.10 SW3_ESTE

#### Función
- Switch de acceso para red administrativa y FTP

```bash
enable
configure terminal
hostname SW3_ESTE

interface fa0/1
description HACIA_R3_ESTE
switchport mode access
spanning-tree portfast
exit

interface fa0/11
description HACIA_FTP
switchport mode access
spanning-tree portfast
exit

interface fa0/12
description HACIA_ADMIN_PC
switchport mode access
spanning-tree portfast
exit

interface vlan 1
ip address 192.168.80.2 255.255.255.0
no shutdown
exit

ip default-gateway 192.168.80.1
ip domain-name redeslannoche.com
username admin secret Admin123!
crypto key generate rsa
1024
ip ssh version 2
line vty 0 4
login local
transport input ssh
exit
end
write memory
```

---

## 3. Configuración de servidores

### 3.1 WEB

- IP: `192.168.50.10`
- Máscara: `255.255.255.0`
- Gateway: `192.168.50.1`
- DNS: `192.168.50.20`
- Servicio: HTTP ON

Contenido sugerido de `index.html`:

```html
<html>
<head>
<title>redeslannoche.com</title>
</head>
<body>
<h1>Proyecto LAN - redeslannoche.com</h1>
<p>Bienvenido al sitio web del proyecto.</p>
<p>Servicios disponibles: DNS, WEB, MAIL y FTP.</p>
</body>
</html>
```

---

### 3.2 DNS

- IP: `192.168.50.20`
- Máscara: `255.255.255.0`
- Gateway: `192.168.50.1`
- DNS: `192.168.50.20`
- Servicio DNS: ON

Registros configurados:
- `www.redeslannoche.com` → `192.168.50.10`
- `mail.redeslannoche.com` → `192.168.50.30`
- `ftp.redeslannoche.com` → `192.168.80.10`
- `www.google.com` → `200.0.0.10`

---

### 3.3 MAIL

- IP: `192.168.50.30`
- Máscara: `255.255.255.0`
- Gateway: `192.168.50.1`
- DNS: `192.168.50.20`
- Servicios: SMTP ON, POP3 ON
- Dominio: `redeslannoche.com`

Usuarios sugeridos:
- user1 / 123
- user2 / 123
- user3 / 123
- user4 / 123
- user5 / 123
- user6 / 123
- user7 / 123
- user8 / 123
- user9 / 123
- user10 / 123

---

### 3.4 DHCP_OESTE

- IP: `192.168.60.10`
- Máscara: `255.255.255.0`
- Gateway: `192.168.60.1`
- DNS: `192.168.50.20`
- Servicio DHCP: ON

Pools:

#### VLAN_C_OESTE
- Default Gateway: `192.168.10.1`
- DNS Server: `192.168.50.20`
- Start IP Address: `192.168.10.11`
- Subnet Mask: `255.255.255.0`
- Maximum Number of Users: `244`

#### VLAN_D_OESTE
- Default Gateway: `192.168.30.1`
- DNS Server: `192.168.50.20`
- Start IP Address: `192.168.30.11`
- Subnet Mask: `255.255.255.192`
- Maximum Number of Users: `52`

---

### 3.5 DHCP_ESTE

- IP: `192.168.70.10`
- Máscara: `255.255.255.0`
- Gateway: `192.168.70.1`
- DNS: `192.168.50.20`
- Servicio DHCP: ON

Pools:

#### VLAN_M_ESTE
- Default Gateway: `172.16.10.1`
- DNS Server: `192.168.50.20`
- Start IP Address: `172.16.10.11`
- Subnet Mask: `255.255.255.0`
- Maximum Number of Users: `244`

#### VLAN_N_ESTE
- Default Gateway: `172.16.20.1`
- DNS Server: `192.168.50.20`
- Start IP Address: `172.16.20.11`
- Subnet Mask: `255.255.255.0`
- Maximum Number of Users: `244`

---

### 3.6 FTP

- IP: `192.168.80.10`
- Máscara: `255.255.255.0`
- Gateway: `192.168.80.1`
- DNS: `192.168.50.20`
- Servicio FTP: ON

Usuario sugerido:
- `adminftp / 123`

---

### 3.7 GOOGLE_SERVER

- IP: `200.0.0.10`
- Máscara: `255.255.255.0`
- Gateway: `200.0.0.1`
- Servicio HTTP: ON

Contenido sugerido:

```html
<html>
<head>
<title>Google</title>
</head>
<body>
<h1>Google Server Simulado</h1>
<p>Acceso a internet correcto.</p>
</body>
</html>
```

---

## 4. Configuración de equipos finales

### 4.1 PCs por DHCP

Las siguientes PCs deben configurarse en modo DHCP:
- VLAN_A_OESTE
- VLAN_B_OESTE
- VLAN_C_OESTE
- VLAN_D_OESTE
- VLAN_M_ESTE
- VLAN_N_ESTE
- VLAN_O_ESTE
- VLAN_P_ESTE

Verificación:

```bash
ipconfig
```

---

### 4.2 Admin_PC

Configuración estática:
- IP: `192.168.80.20`
- Máscara: `255.255.255.0`
- Gateway: `192.168.80.1`
- DNS: `192.168.50.20`

---

## 5. Pruebas recomendadas

### En switches

```bash
show vlan brief
show interfaces trunk
show ip ssh
show running-config
```

### En servidores y PCs
- Validar IP, gateway y DNS
- Probar acceso web
- Probar resolución DNS
- Probar correo
- Probar FTP
- Probar SSH desde Admin_PC

### Ejemplos

```bash
ping 192.168.50.20
ping www.redeslannoche.com
ping www.google.com
ftp 192.168.80.10
ssh -l admin 192.168.80.2
```

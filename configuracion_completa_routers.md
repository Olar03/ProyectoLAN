# CONFIGURACIÓN COMPLETA DE ROUTERS

Este documento recopila la configuración general y los ejemplos completos de configuración aplicados a los routers del proyecto de red LAN. Incluye parámetros básicos, interfaces WAN, subinterfaces LAN, DHCP, RIP v2, SSH y rutas por defecto donde corresponde.

---

## 1. Parámetros generales usados en routers

- Dominio: `redeslannoche.com`
- Usuario SSH: `admin`
- Clave SSH: `Admin123!`
- Protocolo de enrutamiento: `RIP v2`
- Enlaces seriales con máscara `/30`
- Enlaces LAN según subredes definidas por VLAN o servicios

Configuración base aplicada en todos los routers:

```bash
enable
configure terminal
hostname NOMBRE_DEL_ROUTER
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

## 2. Configuración de R1_OESTE

### Función
- Router-on-a-stick para VLAN_A y VLAN_B
- Participación en la WAN OESTE
- Servidor DHCP para VLAN_A y VLAN_B

### Configuración

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

interface s0/0/0
description HACIA_R2_OESTE
ip address 10.0.0.1 255.255.255.252
clock rate 64000
no shutdown
exit

interface s0/0/1
description HACIA_R4_OESTE
ip address 10.0.0.5 255.255.255.252
clock rate 64000
no shutdown
exit

ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp excluded-address 192.168.40.1 192.168.40.10

ip dhcp pool VLAN_A_OESTE
network 192.168.20.0 255.255.255.128
default-router 192.168.20.1
dns-server 192.168.50.20
domain-name redeslannoche.com
exit

ip dhcp pool VLAN_B_OESTE
network 192.168.40.0 255.255.255.224
default-router 192.168.40.1
dns-server 192.168.50.20
domain-name redeslannoche.com
exit

router rip
version 2
no auto-summary
network 10.0.0.0
network 192.168.20.0
network 192.168.40.0
exit

line vty 0 4
login local
transport input ssh
exit

end
write memory
```

---

## 3. Configuración de R2_OESTE

### Función
- Conectividad WAN OESTE
- Acceso a red de servidores OESTE

### Configuración

```bash
enable
configure terminal
hostname R2_OESTE
no ip domain-lookup
ip domain-name redeslannoche.com
username admin secret Admin123!
crypto key generate rsa
1024
ip ssh version 2

interface g0/0
description HACIA_RED_SERVIDORES_OESTE
ip address 192.168.50.1 255.255.255.0
no shutdown
exit

interface s0/0/0
description HACIA_R1_OESTE
ip address 10.0.0.2 255.255.255.252
no shutdown
exit

interface s0/0/1
description HACIA_R3_OESTE
ip address 10.0.0.9 255.255.255.252
clock rate 64000
no shutdown
exit

interface s0/1/0
description HACIA_R4_OESTE
ip address 10.0.0.13 255.255.255.252
clock rate 64000
no shutdown
exit

router rip
version 2
no auto-summary
network 10.0.0.0
network 192.168.50.0
exit

line vty 0 4
login local
transport input ssh
exit

end
write memory
```

---

## 4. Configuración de R3_OESTE

### Función
- Router de tránsito entre OESTE y ESTE
- Participación en WAN OESTE y enlace interzonal

### Configuración

```bash
enable
configure terminal
hostname R3_OESTE
no ip domain-lookup
ip domain-name redeslannoche.com
username admin secret Admin123!
crypto key generate rsa
1024
ip ssh version 2

interface s0/0/0
description HACIA_R2_OESTE
ip address 10.0.0.10 255.255.255.252
no shutdown
exit

interface s0/0/1
description HACIA_R4_OESTE
ip address 10.0.0.17 255.255.255.252
clock rate 64000
no shutdown
exit

interface s0/1/0
description HACIA_R1_ESTE
ip address 10.0.0.21 255.255.255.252
no shutdown
exit

router rip
version 2
no auto-summary
network 10.0.0.0
exit

line vty 0 4
login local
transport input ssh
exit

end
write memory
```

---

## 5. Configuración de R4_OESTE

### Función
- Router-on-a-stick para VLAN_C y VLAN_D
- Conectividad con DHCP_OESTE
- Participación en WAN OESTE

### Configuración

```bash
enable
configure terminal
hostname R4_OESTE
no ip domain-lookup
ip domain-name redeslannoche.com
username admin secret Admin123!
crypto key generate rsa
1024
ip ssh version 2

interface g0/0
description HACIA_DHCP_OESTE
ip address 192.168.60.1 255.255.255.0
no shutdown
exit

interface g0/1
no ip address
no shutdown
exit

interface g0/1.30
encapsulation dot1Q 30
ip address 192.168.10.1 255.255.255.0
ip helper-address 192.168.60.10
exit

interface g0/1.40
encapsulation dot1Q 40
ip address 192.168.30.1 255.255.255.192
ip helper-address 192.168.60.10
exit

interface s0/0/0
description HACIA_R1_OESTE
ip address 10.0.0.6 255.255.255.252
no shutdown
exit

interface s0/0/1
description HACIA_R2_OESTE
ip address 10.0.0.14 255.255.255.252
no shutdown
exit

interface s0/1/0
description HACIA_R3_OESTE
ip address 10.0.0.18 255.255.255.252
no shutdown
exit

router rip
version 2
no auto-summary
network 10.0.0.0
network 192.168.10.0
network 192.168.30.0
network 192.168.60.0
exit

line vty 0 4
login local
transport input ssh
exit

end
write memory
```

---

## 6. Configuración de R1_ESTE

### Función
- Router intermedio del bloque ESTE
- Salida hacia INTERNET
- Distribución de ruta por defecto

### Configuración

```bash
enable
configure terminal
hostname R1_ESTE
no ip domain-lookup
ip domain-name redeslannoche.com
username admin secret Admin123!
crypto key generate rsa
1024
ip ssh version 2

interface s0/0/0
description HACIA_R2_ESTE
ip address 10.0.0.25 255.255.255.252
no shutdown
exit

interface s0/0/1
description HACIA_R4_ESTE
ip address 10.0.0.29 255.255.255.252
no shutdown
exit

interface s0/1/0
description HACIA_INTERNET
ip address 10.0.0.33 255.255.255.252
no shutdown
exit

interface s0/1/1
description HACIA_R3_OESTE
ip address 10.0.0.22 255.255.255.252
clock rate 64000
no shutdown
exit

ip route 0.0.0.0 0.0.0.0 10.0.0.34

router rip
version 2
no auto-summary
network 10.0.0.0
default-information originate
exit

line vty 0 4
login local
transport input ssh
exit

end
write memory
```

---

## 7. Configuración de R2_ESTE

### Función
- Router-on-a-stick para VLAN_O y VLAN_P
- Servidor DHCP para VLAN_O y VLAN_P
- Participación en WAN ESTE

### Configuración

```bash
enable
configure terminal
hostname R2_ESTE
no ip domain-lookup
ip domain-name redeslannoche.com
username admin secret Admin123!
crypto key generate rsa
1024
ip ssh version 2

interface g0/0
no ip address
no shutdown
exit

interface g0/0.70
encapsulation dot1Q 70
ip address 172.16.30.1 255.255.255.128
exit

interface g0/0.80
encapsulation dot1Q 80
ip address 172.16.40.1 255.255.255.224
exit

interface s0/0/0
description HACIA_R1_ESTE
ip address 10.0.0.26 255.255.255.252
no shutdown
exit

interface s0/0/1
description HACIA_R3_ESTE
ip address 10.0.0.37 255.255.255.252
clock rate 64000
no shutdown
exit

interface s0/1/0
description HACIA_R4_ESTE
ip address 10.0.0.41 255.255.255.252
clock rate 64000
no shutdown
exit

ip dhcp excluded-address 172.16.30.1 172.16.30.10
ip dhcp excluded-address 172.16.40.1 172.16.40.10

ip dhcp pool VLAN_O_ESTE
network 172.16.30.0 255.255.255.128
default-router 172.16.30.1
dns-server 192.168.50.20
domain-name redeslannoche.com
exit

ip dhcp pool VLAN_P_ESTE
network 172.16.40.0 255.255.255.224
default-router 172.16.40.1
dns-server 192.168.50.20
domain-name redeslannoche.com
exit

router rip
version 2
no auto-summary
network 10.0.0.0
network 172.16.30.0
network 172.16.40.0
exit

line vty 0 4
login local
transport input ssh
exit

end
write memory
```

---

## 8. Configuración de R3_ESTE

### Función
- Router de acceso a red FTP y administración
- Participación en WAN ESTE

### Configuración

```bash
enable
configure terminal
hostname R3_ESTE
no ip domain-lookup
ip domain-name redeslannoche.com
username admin secret Admin123!
crypto key generate rsa
1024
ip ssh version 2

interface g0/0
description HACIA_RED_ADMIN_FTP
ip address 192.168.80.1 255.255.255.0
no shutdown
exit

interface s0/0/0
description HACIA_R2_ESTE
ip address 10.0.0.38 255.255.255.252
no shutdown
exit

interface s0/0/1
description HACIA_R4_ESTE
ip address 10.0.0.45 255.255.255.252
clock rate 64000
no shutdown
exit

router rip
version 2
no auto-summary
network 10.0.0.0
network 192.168.80.0
exit

line vty 0 4
login local
transport input ssh
exit

end
write memory
```

---

## 9. Configuración de R4_ESTE

### Función
- Router-on-a-stick para VLAN_M y VLAN_N
- Acceso a DHCP_ESTE
- Participación en WAN ESTE

### Configuración

```bash
enable
configure terminal
hostname R4_ESTE
no ip domain-lookup
ip domain-name redeslannoche.com
username admin secret Admin123!
crypto key generate rsa
1024
ip ssh version 2

interface g0/0
no ip address
no shutdown
exit

interface g0/0.50
encapsulation dot1Q 50
ip address 172.16.10.1 255.255.255.0
ip helper-address 192.168.70.10
exit

interface g0/0.60
encapsulation dot1Q 60
ip address 172.16.20.1 255.255.255.0
ip helper-address 192.168.70.10
exit

interface g0/1
description HACIA_DHCP_ESTE
ip address 192.168.70.1 255.255.255.0
no shutdown
exit

interface s0/0/0
description HACIA_R1_ESTE
ip address 10.0.0.30 255.255.255.252
no shutdown
exit

interface s0/0/1
description HACIA_R3_ESTE
ip address 10.0.0.46 255.255.255.252
no shutdown
exit

interface s0/1/0
description HACIA_R2_ESTE
ip address 10.0.0.42 255.255.255.252
no shutdown
exit

router rip
version 2
no auto-summary
network 10.0.0.0
network 172.16.10.0
network 172.16.20.0
network 192.168.70.0
exit

line vty 0 4
login local
transport input ssh
exit

end
write memory
```

---

## 10. Configuración de INTERNET

### Función
- Simulación de salida a Internet
- Conectividad hacia GOOGLE_SERVER

### Configuración

```bash
enable
configure terminal
hostname INTERNET
no ip domain-lookup

interface s0/0/0
description HACIA_R1_ESTE
ip address 10.0.0.34 255.255.255.252
clock rate 64000
no shutdown
exit

interface g0/0
description HACIA_GOOGLE_SERVER
ip address 200.0.0.1 255.255.255.0
no shutdown
exit

ip route 192.168.0.0 255.255.0.0 10.0.0.33
ip route 172.16.0.0 255.255.0.0 10.0.0.33

end
write memory
```

---

## 11. Comandos de verificación recomendados en routers

```bash
show ip interface brief
show ip route
show ip protocols
show running-config
show ip dhcp binding
show ip dhcp pool
show ip ssh
```

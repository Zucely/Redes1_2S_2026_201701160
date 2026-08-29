# Manual Técnico — Tarea 3

## 1. Diseño de la topología

   ![Diseño topologia](capturas/disenioTopologia.png)

- **Switch0**: switch central, servidor VTP. Puertos fa0/1, fa0/2, fa0/3 →
  trunk hacia ADMIN, MERCA y VENTAS respectivamente.
- **ADMIN, MERCA, VENTAS**: switches de acceso 2960. Puerto fa0/1 → trunk
  hacia Switch0. Puertos fa0/2 y fa0/3 → acceso, conectados a 2 PCs cada uno.

## Configuración de IPs

| VLAN | NOMBRE | PC  | IP          |
|------|--------|-----|-------------|
| 10   | ADMIN  | PC0 | 192.168.1.1 |
| 10   | ADMIN  | PC1 | 192.168.1.2 |
| 20   | MERCA  | PC2 | 192.168.2.1 |
| 20   | MERCA  | PC3 | 192.168.2.2 |
| 30   | VENTAS | PC4 | 192.168.3.1 |
| 30   | VENTAS | PC5 | 192.168.3.2 |


**Dominio VTP:** `tarea3`
**Contraseña VTP:** `t3123`

## 3. Configuración de switches

### 3.1 Switch0 (servidor VTP)

```
enable
configure terminal

vtp domain tarea3
vtp password t3123
vtp mode server

vlan 10
 name ADMIN
vlan 20
 name MERCA
vlan 30
 name VENTAS
exit

interface range fastEthernet0/1 - 3
 switchport mode trunk
 switchport trunk allowed vlan all
exit

```

### 3.2 Switch ADMIN (cliente VTP, VLAN 10)

```
enable
configure terminal
hostname ADMIN

vtp domain tarea3
vtp password t3123
vtp mode client

interface fastEthernet0/1
 switchport mode trunk
exit

interface range fastEthernet0/2 - 3
 switchport mode access
 switchport access vlan 10
exit

```

### 3.3 Switch MERCA (cliente VTP, VLAN 20)

```
enable
configure terminal
hostname MERCA

vtp domain tarea3
vtp password t3123
vtp mode client

interface fastEthernet0/1
 switchport mode trunk
exit

interface range fastEthernet0/2 - 3
 switchport mode access
 switchport access vlan 20
exit

```

### 3.4 Switch VENTAS (modo transparente VTP, VLAN 30)

```
enable
configure terminal
hostname VENTAS

vtp domain tarea3
vtp password t3123
vtp mode transparent

vlan 30
 name VENTAS
exit

interface fastEthernet0/1
 switchport mode trunk
exit

interface range fastEthernet0/2 - 3
 switchport mode access
 switchport access vlan 30
exit

```

---

### 4.1 `show vtp status`

**Switch0**

   ![Show vtp status switch0](capturas/vtpStatus0.png)

**ADMIN**

   ![Show vtp status ADMIN](capturas/vtpStatusADMIN.png)

**MERCA**

   ![Show vtp status MERCA](capturas/vtpStatusMERCA.png)

**VENTAS**

   ![Show vtp status VENTAS](capturas/vtpStatusVENTAS.png)


### 4.2 `show vlan brief`

**Switch0**

   ![Show vlan brief switch0](capturas/vlanBrief0.png)

**ADMIN**

   ![Show vlan brief ADMIN](capturas/vlanBriefADMIN.png)

**MERCA**

   ![Show vlan brief MERCA](capturas/vlanBriefMERCA.png)

**VENTAS**

   ![Show vlan brief VENTAS](capturas/vlanBriefVENTASpng.png)




## 6. Pruebas de conectividad (ping)

### 6.1 Ping dentro de la misma VLAN (debe funcionar)

| Origen | Destino | Resultado esperado |
|--------|---------|---------------------|
| PC0  | PC1  | Exitoso |
| PC2  | PC3  | Exitoso |
| PC4  | PC5  | Exitoso |

   ![ping éxitosos](capturas/pingsExitosos.png)


### 6.2 Ping entre VLANs distintas (debe fallar)

| Origen | Destino | Resultado esperado |
|--------|---------|---------------------|
| PC1 (VLAN10) | PC3 (VLAN20) | Falla (Request timed out / Destination unreachable) |
| PC3 (VLAN20) | PC5 (VLAN30) | Falla |
| PC1 (VLAN10) | PC5 (VLAN30) | Falla |

   ![Ping fallidos](capturas/pingFallidos.png)


**Explicación del fallo:** las PCs de VLANs distintas están en subredes IP
diferentes y no hay un dispositivo de capa 3 (router o SVI) que enrute entre
ellas, además de que a nivel de capa 2 el switch mantiene el tráfico de cada
VLAN aislado (broadcast domains separados). Por eso el ping nunca llega,
aunque físicamente todo esté conectado a la misma infraestructura.



# Grupo 3

**Carnet Coordinador:** 202200057


# Comandos Adicionales

```cisco
no ip domain-lookup
```
# Subnetting VLSM (desde 10.57.10.0/24)

| VLAN | Hosts | Máscara | Red | Gateway |
|------|-------|---------|-----|---------|
| 13 Cajas | 45 | /26 | 10.57.10.0 | .1 |
| 23 Asesores | 30 | /26 | 10.57.10.64 | .65 |
| 43 Seguridad | 12 | /28 | 10.57.10.128 | .129 |
| 33 Gerencia | 10 | /28 | 10.57.10.144 | .145 |


# Configuración Completa de los Routers Core



```cisco
                    ----- CORE 1 ------
enable
configure terminal
hostname Core1
no ip domain-lookup
ip routing
line console 0
 logging synchronous
 exit

interface range fa0/23 - 24
 no channel-group
 shutdown
 exit

interface port-channel 1
 no switchport
 ip address 10.57.0.1 255.255.255.252
 exit

interface range fa0/23 - 24
 no switchport
 channel-group 1 mode on
 no shutdown
 exit

interface gi0/1
 no switchport
 ip address 10.57.0.5 255.255.255.252
 no shutdown
 exit
end
write memory

                    ----- CORE 2 ------
enable
configure terminal
hostname Core2
no ip domain-lookup
ip routing
line console 0
 logging synchronous
 exit

interface range fa0/23 - 24
 no channel-group
 shutdown
 exit


interface port-channel 1
 no switchport
 ip address 10.57.0.2 255.255.255.252
 exit

interface range fa0/23 - 24
 no switchport
 channel-group 1 mode on
 no shutdown
 exit

interface gi0/1
 no switchport
 ip address 10.57.0.9 255.255.255.252
 no shutdown
 exit
end
write memory

                    ----- CORE 3 ------
enable
configure terminal
hostname Core3
no ip domain-lookup
line console 0
 logging synchronous
 exit

interface gi0/0
 ip address 10.57.0.6 255.255.255.252
 no shutdown
 exit

interface gi0/1
 ip address 10.57.0.10 255.255.255.252
 no shutdown
 exit
end
write memory

 ```





# Configuración Enlace Serial: Core 3 ↔ Router Occidente


```cisco
                    ----- En Core 3 -----
configure terminal

interface se0/0/0
 ip address 10.57.0.13 255.255.255.252
 clock rate 64000
 no shutdown
 exit

end
write memory

            ----- En Router occidente -----
enable
configure terminal
hostname R-Occidente
no ip domain-lookup
line console 0
 logging synchronous
 exit

interface se0/0/0
 ip address 10.57.0.14 255.255.255.252
 no shutdown
 exit

end
write memory
```


# Configuración de OSPF


```cisco
                    ---- En Core 1 ----
configure terminal
router ospf 1
 router-id 1.1.1.1
 network 10.57.0.0 0.0.0.3 area 0
 network 10.57.0.4 0.0.0.3 area 0
 exit
end
write memory

                    ---- En Core 2 ----
configure terminal
router ospf 1
 router-id 2.2.2.2
 network 10.57.0.0 0.0.0.3 area 0
 network 10.57.0.8 0.0.0.3 area 0
 exit
end
write memory

                    ---- En Core 3 ----
configure terminal
router ospf 1
 router-id 3.3.3.3
 network 10.57.0.4 0.0.0.3 area 0
 network 10.57.0.8 0.0.0.3 area 0
 network 10.57.0.12 0.0.0.3 area 0
 exit
end
write memory

                    ---- En ROUTER-OCCIDENTE ----
configure terminal
router ospf 1
 router-id 4.4.4.4
 network 10.57.0.12 0.0.0.3 area 0
 exit
end
write memory
```

# Comandos de pruebas
```cisco
show ip ospf neighbor   (Prueba realizada en el Core1.)
show ip route ospf      (Prueba realizada en el Core1.)
```


# Configuración de Switches (Occidente)

```cisco
---- SW-Occ-Principal (VTP server) ----

enable
configure terminal
hostname SW-Occ-Principal
no ip domain-lookup
line console 0
 logging synchronous
 exit

vtp domain bantech57
vtp password cisco
vtp mode server

vlan 13
 name Cajas
vlan 23
 name Asesores
vlan 33
 name Gerencia
vlan 43
 name Seguridad
 exit

interface range fa0/1 - 2
 switchport mode trunk
 exit

interface range fa0/3 - 4
 switchport mode access
 switchport access vlan 13
 exit

interface range fa0/5 - 6
 switchport mode access
 switchport access vlan 23
 exit
end
write memory

        ---- SW-Occ-Acceso (VTP client) ---
enable
configure terminal
hostname SW-Occ-Acceso
no ip domain-lookup
line console 0
 logging synchronous
 exit

vtp domain bantech57
vtp password cisco
vtp mode client

interface fa0/1
 switchport mode trunk
 exit
end
```



# Router-on-a-stick en R-Occidente



```cisco
                    ---- R-Occidente ----
configure terminal

interface gi0/0
 no ip address
 no shutdown
 exit

interface gi0/0.13
 encapsulation dot1Q 13
 ip address 10.57.10.1 255.255.255.192
 exit

interface gi0/0.23
 encapsulation dot1Q 23
 ip address 10.57.10.65 255.255.255.192
 exit

interface gi0/0.33
 encapsulation dot1Q 33
 ip address 10.57.10.145 255.255.255.240
 exit

interface gi0/0.43
 encapsulation dot1Q 43
 ip address 10.57.10.129 255.255.255.240
 exit

router ospf 1
 network 10.57.10.0 0.0.0.255 area 0
 exit
end
write memory

```
# Asignacion de Ip estaticas Occidente
| PC | IP | Máscara | Gateway |
|----|----|---------|---------|
| Cajas 1 | 10.57.10.2 | 255.255.255.192 | 10.57.10.1 |
| Cajas 2 | 10.57.10.3 | 255.255.255.192 | 10.57.10.1 |
| Asesores 1 | 10.57.10.66 | 255.255.255.192 | 10.57.10.65 |
| Asesores 2 | 10.57.10.67 | 255.255.255.192 | 10.57.10.65 |
| Gerencia | 10.57.10.146 | 255.255.255.240 | 10.57.10.145 |
| Seguridad 1 | 10.57.10.130 | 255.255.255.240 | 10.57.10.129 |
| Seguridad 2 | 10.57.10.131 | 255.255.255.240 | 10.57.10.129 |

# Pruebas de ping Occidente
Caja1 - Gateway
![Caja1 - Gateway](Capturas/pingCaja1aGateway.png)

Caja1 - Asesores
![Caja1 - Asesores](Capturas/pingCaja1aAsesores.png)

Caja1 - Gerencia
![Caja1 - Gerencia](Capturas/pingCaja1AGerencia.png)

Caja1 - Core1
![Caja1 - Core1](Capturas/pingCaja1ACore1.png)








# Capturas

Topologia del core
![Topologia del core](Capturas/Topologia-Core.png)

ping del core
![Ping del core](Capturas/Ping-Core.png)

Prueba OSPF
![Prueba OSPF](Capturas/PruebaOSPF.png)

Topologia Occidente
![Topologia Occidente](Capturas/TopologiaOccidente.png)

Vlans Occidente
![Vlans Occidente](Capturas/VlansOccidente.png)

Vlans Occidente
![Vlans Occidente](Capturas/VlansOccidente.png)


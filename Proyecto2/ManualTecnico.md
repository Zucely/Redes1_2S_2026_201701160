# Grupo 3

**Carnet Coordinador:** 202200057


# Comandos Adicionales

```cisco
no ip domain-lookup
```



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

## En Core 3

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

# Capturas

Topologia del core
![Topologia del core](Capturas/Topologia-Core.png)

ping del core
![Ping del core](Capturas/Ping-Core.png)

Prueba OSPF
![Prueba OSPF](Capturas/PruebaOSPF.png)
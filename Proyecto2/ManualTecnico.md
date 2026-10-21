# Grupo 3

**Carnet Coordinador:** 202200057


# Comandos Adicionales

```cisco
no ip domain-lookup
```


# Configuración Core1 (Switch 3560)

# Configuración Completa de los Routers Core

## CORE 1

```cisco
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
 ```

# Configuración Core2 (Switch 3560)
```cisco
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
 ```


 # Configuración Core3

```cisco
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


# Capturas

Topologia del core
![Topologia del core](Capturas/Topologia-Core.png)

ping del core
![Ping del core](Capturas/Ping-Core.png)
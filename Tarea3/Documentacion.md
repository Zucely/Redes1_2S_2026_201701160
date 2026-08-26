# Manual Técnico — Tarea 3

## 2. Diseño de la topología

   ![VTP status Switch0](capturas/disenioTopologia.png)

- **Switch0**: switch central, servidor VTP. Puertos fa0/1, fa0/2, fa0/3 →
  trunk hacia ADMIN, MERCA y VENTAS respectivamente.
- **ADMIN, MERCA, VENTAS**: switches de acceso 2960. Puerto fa0/1 → trunk
  hacia Switch0. Puertos fa0/2 y fa0/3 → acceso, conectados a 2 PCs cada uno.
- No hay router en la topología, por lo que **no existe enrutamiento entre
  VLANs**: esto es justamente lo que provoca que el ping entre VLANs distintas
  falle (punto 5c del enunciado).

### 2.1 Plan de direccionamiento IP

| VLAN | Nombre | Red            | Switch de acceso | PC  | IP            |
|------|--------|----------------|-------------------|-----|---------------|
| 10   | ADMIN  | 192.168.10.0/24| ADMIN             | PC1 | 192.168.10.10 |
| 10   | ADMIN  | 192.168.10.0/24| ADMIN             | PC2 | 192.168.10.11 |
| 20   | MERCA  | 192.168.20.0/24| MERCA             | PC3 | 192.168.20.10 |
| 20   | MERCA  | 192.168.20.0/24| MERCA             | PC4 | 192.168.20.11 |
| 30   | VENTAS | 192.168.30.0/24| VENTAS            | PC5 | 192.168.30.10 |
| 30   | VENTAS | 192.168.30.0/24| VENTAS            | PC6 | 192.168.30.11 |

Máscara: `255.255.255.0` en todas las PCs. No es necesario configurar
gateway (no hay comunicación inter-VLAN prevista), pero podés dejar el
campo con la IP `.1` de cada red por prolijidad (ej. `192.168.10.1`).

**Dominio VTP:** `REDES2026`
**Contraseña VTP:** `cisco123`
**Número de revisión:** dejar que Switch0 lo maneje automáticamente (arranca
en 0 y sube con cada cambio); no tocar VLANs en los switches cliente para
evitar conflictos de revisión.

---

## 3. Configuración de switches

### 3.1 Switch0 (servidor VTP)

```
enable
configure terminal
hostname Switch0

vtp domain REDES2026
vtp password cisco123
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

end
write memory
```

### 3.2 Switch ADMIN (cliente VTP, VLAN 10)

```
enable
configure terminal
hostname ADMIN

vtp domain REDES2026
vtp password cisco123
vtp mode client

interface fastEthernet0/1
 switchport mode trunk
exit

interface range fastEthernet0/2 - 3
 switchport mode access
 switchport access vlan 10
exit

end
write memory
```

### 3.3 Switch MERCA (cliente VTP, VLAN 20)

```
enable
configure terminal
hostname MERCA

vtp domain REDES2026
vtp password cisco123
vtp mode client

interface fastEthernet0/1
 switchport mode trunk
exit

interface range fastEthernet0/2 - 3
 switchport mode access
 switchport access vlan 20
exit

end
write memory
```

### 3.4 Switch VENTAS (modo transparente VTP, VLAN 30)

> En modo **transparente**, el switch NO aprende VLANs vía VTP: hay que
> crear la VLAN localmente. Sí reenvía los anuncios VTP que recibe hacia
> otros switches, pero no aplica los cambios a su propia base de datos.

```
enable
configure terminal
hostname VENTAS

vtp domain REDES2026
vtp password cisco123
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

end
write memory
```

---

## 4. Verificación

Ejecutar en **cada switch** y pegar la captura correspondiente debajo de
cada comando.

### 4.1 `show vtp status`

**Switch0**
```
(pegar aquí la captura/salida)
```

**ADMIN**
```
(pegar aquí la captura/salida)
```

**MERCA**
```
(pegar aquí la captura/salida)
```

**VENTAS**
```
(pegar aquí la captura/salida)
```

Puntos a verificar en la salida:
- `VTP Operating Mode` correcto por switch (Server / Client / Transparent).
- `VTP Domain Name` = `REDES2026` en los cuatro switches.
- `Configuration Revision` igual entre Switch0, ADMIN y MERCA (VENTAS no
  sincroniza porque está en modo transparente).

### 4.2 `show vlan brief`

**Switch0**
```
(pegar aquí la captura/salida)
```

**ADMIN** — debe mostrar VLAN 10, 20 y 30 propagadas desde el servidor
```
(pegar aquí la captura/salida)
```

**MERCA** — debe mostrar VLAN 10, 20 y 30 propagadas desde el servidor
```
(pegar aquí la captura/salida)
```

**VENTAS** — solo debe mostrar la VLAN 30 creada localmente (no recibe las
demás porque está en modo transparente)
```
(pegar aquí la captura/salida)
```

---

## 5. Direccionamiento de PCs

Configurar en cada PC (pestaña Desktop → IP Configuration) la IP fija según
la tabla de la sección 2.1.

---

## 6. Pruebas de conectividad (ping)

### 6.1 Ping dentro de la misma VLAN (debe funcionar)

| Origen | Destino | Resultado esperado |
|--------|---------|---------------------|
| PC1 (192.168.10.10) | PC2 (192.168.10.11) | Exitoso |
| PC3 (192.168.20.10) | PC4 (192.168.20.11) | Exitoso |
| PC5 (192.168.30.10) | PC6 (192.168.30.11) | Exitoso |

```
(pegar aquí captura de ping PC1 -> PC2)
```

### 6.2 Ping entre VLANs distintas (debe fallar)

| Origen | Destino | Resultado esperado |
|--------|---------|---------------------|
| PC1 (VLAN10) | PC3 (VLAN20) | Falla (Request timed out / Destination unreachable) |
| PC3 (VLAN20) | PC5 (VLAN30) | Falla |
| PC1 (VLAN10) | PC5 (VLAN30) | Falla |

```
(pegar aquí captura de ping PC1 -> PC3)
```

**Explicación del fallo:** las PCs de VLANs distintas están en subredes IP
diferentes y no hay un dispositivo de capa 3 (router o SVI) que enrute entre
ellas, además de que a nivel de capa 2 el switch mantiene el tráfico de cada
VLAN aislado (broadcast domains separados). Por eso el ping nunca llega,
aunque físicamente todo esté conectado a la misma infraestructura.

---

## 7. Conclusiones

_(completar con 3-5 líneas: qué se aprendió sobre VLANs, VTP, aislamiento de
tráfico y diferencias entre modo server/client/transparent)_

---

## 8. Anexo — Checklist de entrega

- [ ] Archivo `.pkt` con la red completa y funcional
- [ ] Este manual `.md` con todas las capturas insertadas
- [ ] Capturas de `show vtp status` (4 switches)
- [ ] Capturas de `show vlan brief` (4 switches)
- [ ] Capturas de ping exitoso (misma VLAN)
- [ ] Capturas de ping fallido (VLANs distintas)
- [ ] Todo subido a la carpeta `Tarea 3` del repo `Redes1_2S_2026_CARNET`
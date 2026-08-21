# Manual Técnico --- Práctica 1: QuetzalDev S.A.

**María Zucely Hernández García**\
**Carné: 201701160**

## 1. Inventario de equipos

  Ítem                              Cantidad Ubicación
  ------------------------- ---------------- ----------------------
  Switch principal (core)                  1 MDF
  Switch de departamento                   8 Uno por departamento
  Patch panel                 2 × 24 puertos MDF
  Rack de piso                             1 MDF
  UPS                                      1 MDF
  Tomas de red unitarias                   2 Backend
  Tomas de red dobles                     22 Áreas de trabajo
  Tomas de red triples                     1 Data Center
  Puntos de red                           48 Todo el edificio

## 2. Ubicación y justificación del MDF

El MDF se ubica en la zona central del edificio, adyacente al Hall
Central. Esta ubicación reduce la distancia promedio hacia los
departamentos y facilita la distribución del cableado troncal hacia
Recepción, Recursos Humanos, Legal, Sala de Capacitación, Diseño e
Innovación, Dirección General, Backend y Data Center.

## 3. Distribución de equipos por departamento

  Depto                    Escritorio   Laptop   Servidor   Total puntos
  ---------------------- ------------ -------- ---------- --------------
  Recepción                         3        0          1              4
  Recursos Humanos                  6        2          0              8
  Legal                             3        1          0              4
  Sala de Capacitación             10        0          0             10
  Diseño e Innovación               4        3          1              8
  Dirección General                 1        3          0              4
  Backend                           3        3          1              7
  Data Center                       0        0          3              3
  **Total**                    **30**   **12**      **6**         **48**

## 4. Justificación de topología por departamento

La topología general es una **estrella extendida/jerárquica**: el switch
principal del MDF conecta con los ocho switches departamentales mediante
enlaces troncales, y cada switch departamental conecta sus hosts
mediante estrella.

  -----------------------------------------------------------------------
  Depto                   Topología               Justificación
  ----------------------- ----------------------- -----------------------
  Recepción               Estrella                4 puntos; facilita
                                                  administración,
                                                  aislamiento de fallas y
                                                  crecimiento.

  Recursos Humanos        Estrella                8 puntos; centraliza la
                                                  conexión y facilita
                                                  futuras ampliaciones.

  Legal                   Estrella                4 puntos; solución
                                                  sencilla, económica y
                                                  fácil de mantener.

  Sala de Capacitación    Estrella                10 puntos; permite
                                                  administrar
                                                  individualmente los
                                                  puestos y crecer.

  Diseño e Innovación     Estrella                8 puntos; facilita
                                                  mantenimiento y
                                                  administración del
                                                  servidor y estaciones.

  Dirección General       Estrella                4 puntos; facilita
                                                  disponibilidad y
                                                  detección de fallas.

  Backend                 Estrella                7 puntos; facilita
                                                  administración del
                                                  segmento y crecimiento.

  Data Center             Estrella                3 servidores; conexión
                                                  centralizada y
                                                  administración
                                                  independiente.
  -----------------------------------------------------------------------

## 5. Tipo y categoría de cable por segmento

  -----------------------------------------------------------------------
  Segmento          Tipo              Categoría         Justificación
  ----------------- ----------------- ----------------- -----------------
  Horizontal        UTP de cobre      Cat 6A            Adecuado para
                                                        distancias
                                                        cortas, altas
                                                        velocidades y
                                                        cableado
                                                        estructurado.

  Troncal           Fibra óptica      OM3               Mayor capacidad,
                    multimodo                           inmunidad a
                                                        interferencias y
                                                        escalabilidad
                                                        para uplinks.
  -----------------------------------------------------------------------

## 6. Distancias estimadas y cálculo de bobinas

  --------------------------------------------------------------------------
  Depto                 Troncal     Horizontal         Puntos          Cable
                       estimado       promedio                    horizontal
  -------------- -------------- -------------- -------------- --------------
  Recepción              14.5 m            3 m              4           12 m

  Recursos                7.5 m            3 m              8           24 m
  Humanos                                                     

  Legal                  15.5 m            3 m              4           12 m

  Sala de                  14 m            3 m             10           30 m
  Capacitación                                                

  Diseño e                  9 m            3 m              8           24 m
  Innovación                                                  

  Dirección                17 m            3 m              4           12 m
  General                                                     

  Backend                20.5 m            3 m              7           21 m

  Data Center            19.5 m            3 m              3            9 m

  **Total**                                            **48**      **144 m**
  --------------------------------------------------------------------------

Se agrega 15 % de margen: **144 × 1.15 ≈ 166 m**.

Una bobina estándar de 305 m es suficiente para el cálculo base del
cableado horizontal:

**166 ÷ 305 ≈ 0.54 → 1 bobina de 305 m.**

Las distancias son estimaciones y deben ajustarse si se realiza una
medición exacta sobre la escala del plano.

## 7. Cableado troncal

  -----------------------------------------------------------------------
  Enlace                  Medio                   Justificación
  ----------------------- ----------------------- -----------------------
  MDF -- Recepción        Fibra OM3               Alta capacidad e
                                                  inmunidad a
                                                  interferencias.

  MDF -- RRHH             Fibra OM3               Capacidad y
                                                  escalabilidad.

  MDF -- Legal            Fibra OM3               Enlace troncal de alta
                                                  capacidad.

  MDF -- Capacitación     Fibra OM3               Área con mayor cantidad
                                                  de hosts; se prioriza
                                                  capacidad.

  MDF -- Diseño           Fibra OM3               Área con servidor y
                                                  múltiples estaciones.

  MDF -- Dirección        Fibra OM3               Enlace troncal estable
                                                  y escalable.

  MDF -- Backend          Fibra OM3               Área con servidor y
                                                  múltiples estaciones.

  MDF -- Data Center      Fibra OM3               Enlace crítico hacia
                                                  servidores principales.
  -----------------------------------------------------------------------

Etiquetas: `MDF-Recepcion`, `MDF-RRHH`, `MDF-Legal`, `MDF-Capacitacion`,
`MDF-Diseno`, `MDF-Direccion`, `MDF-Backend`, `MDF-DataCenter`.

## 8. Equipo activo: patch panel y switch

-   Total de puntos: **48**
-   Patch panels: **2 de 24 puertos**
-   Switch principal: **48 puertos como mínimo**
-   ODF: **1**, para organizar las terminaciones de fibra.

El patch panel organiza las terminaciones del cableado estructurado; el
switch interconecta los dispositivos y concentra los enlaces
departamentales; el ODF organiza los enlaces de fibra del troncal.

## 9. Canalización

Se propone **escalerilla metálica cerrada** para la ruta principal y
**canaleta para cableado estructurado** para las derivaciones hacia los
puntos de red. La escalerilla protege y organiza los recorridos
principales; la canaleta facilita las bajadas y ampliaciones.

## 10. Rack o gabinete

Se propone un **rack de piso de 42U** en el MDF. Es preferible a un
gabinete de pared porque el MDF concentra el switch principal, dos patch
panels, ODF, organizadores, UPS y espacio para crecimiento.

## 11. Respaldo de energía (UPS)

  Equipo                       Cantidad   Consumo estimado       Total
  -------------------------- ---------- ------------------ -----------
  Switch principal                    1              100 W       100 W
  Switches departamentales            8               60 W       480 W
  Equipamiento auxiliar               1               20 W        20 W
  **Total**                                                  **600 W**

Con 25 % de margen: **600 × 1.25 = 750 W**.

Se recomienda un **UPS de aproximadamente 1500 VA**, con capacidad real
de al menos 900 W. El valor final debe verificarse contra las
especificaciones eléctricas de los modelos seleccionados.

## 12. Estándares T568A/T568B

  -----------------------------------------------------------------------
  Enlace                  Tipo                    Justificación
  ----------------------- ----------------------- -----------------------
  Switch -- PC/laptop     Straight-through        Dispositivo final a
                                                  switch.

  Switch -- Servidor      Straight-through        Servidor a switch.

  MDF -- Switch depto     Fibra                   El troncal propuesto es
                                                  fibra, no UTP.

  Switch -- Switch        Según uplink/equipo     No se utiliza cobre
                                                  directo entre switches
                                                  departamentales.
  -----------------------------------------------------------------------

### Straight-through --- T568B en ambos extremos

    Pin Color
  ----- ----------------
      1 Blanco/Naranja
      2 Naranja
      3 Blanco/Verde
      4 Azul
      5 Blanco/Azul
      6 Verde
      7 Blanco/Marrón
      8 Marrón

### Crossover --- T568A / T568B

    Pin Extremo A T568A   Extremo B T568B
  ----- ----------------- -----------------
      1 Blanco/Verde      Blanco/Naranja
      2 Verde             Naranja
      3 Blanco/Naranja    Blanco/Verde
      4 Azul              Azul
      5 Blanco/Azul       Blanco/Azul
      6 Naranja           Verde
      7 Blanco/Marrón     Blanco/Marrón
      8 Marrón            Marrón

El cableado horizontal se poncha con el mismo estándar en ambos
extremos: T568B en la toma y T568B en el patch panel.

## 13. Etiquetado de cableado

### Horizontal

Formato: `[Departamento]-[Número de Punto de Red]`

  Departamento   Etiquetas
  -------------- -------------------------------------------
  Recepción      `Recepcion-PR01` a `Recepcion-PR04`
  RRHH           `RRHH-PR01` a `RRHH-PR08`
  Legal          `Legal-PR01` a `Legal-PR04`
  Capacitación   `Capacitacion-PR01` a `Capacitacion-PR10`
  Diseño         `Diseno-PR01` a `Diseno-PR08`
  Dirección      `Direccion-PR01` a `Direccion-PR04`
  Backend        `Backend-PR01` a `Backend-PR07`
  Data Center    `DataCenter-PR01` a `DataCenter-PR03`

### Troncal

Formato: `MDF-[Departamento]`.

Se utilizarán: `MDF-Recepcion`, `MDF-RRHH`, `MDF-Legal`,
`MDF-Capacitacion`, `MDF-Diseno`, `MDF-Direccion`, `MDF-Backend` y
`MDF-DataCenter`.

### Comparación con TIA/EIA-606

El esquema utilizado en esta práctica es simplificado: identifica el
área y el número de punto. En una implementación real basada en
TIA/EIA-606 se requiere una administración más completa de
identificadores y registros de infraestructura.

Primera diferencia: el esquema de la práctica identifica principalmente
puntos y enlaces, mientras que una administración real contempla
identificadores únicos para distintos elementos de infraestructura.

Segunda diferencia: el esquema de la práctica no registra toda la
información de espacios, racks, puertos, rutas y conexiones. En un
entorno real se documentarían estos elementos para facilitar
mantenimiento, cambios y localización de fallas.

## 14. Flujo de conexión end-to-end

``` text
Dispositivo final
      ↓
Toma de red
      ↓
Cableado horizontal Cat 6A
      ↓
Patch panel
      ↓
Switch departamental
      ↓
Cableado troncal de fibra
      ↓
ODF del MDF
      ↓
Switch principal
      ↓
Red corporativa
```

Ejemplo:

``` text
PC
 ↓
RRHH-PR01
 ↓
UTP Cat 6A
 ↓
Patch panel
 ↓
SW-RRHH
 ↓
MDF-RRHH
 ↓
Fibra OM3
 ↓
ODF
 ↓
Switch principal
```

## 15. Presupuesto estimado

Los precios siguientes son **estimaciones académicas de referencia**, no
cotizaciones de Guatemala. Deben sustituirse por el precio del producto
concreto si se dispone de una cotización.

  Ítem                                  Cantidad   Precio unitario estimado (Q)   Subtotal (Q)
  ----------------------------------- ---------- ------------------------------ --------------
  Bobina UTP Cat 6A, 305 m                     1                          2,500          2,500
  Switch de departamento                       8                          1,500         12,000
  Switch principal 48 puertos                  1                          7,000          7,000
  Patch panel 24 puertos                       2                            900          1,800
  Rack de piso 42U                             1                          4,500          4,500
  ODF                                          1                          1,200          1,200
  Enlaces de fibra OM3 y accesorios            8                            500          4,000
  Tomas dobles                                22                            100          2,200
  Tomas unitarias                              2                             70            140
  Toma triple                                  1                            150            150
  Organizadores                                4                            250          1,000
  Canalización y accesorios               1 lote                          2,000          2,000
  UPS 1500 VA                                  1                          2,500          2,500
  Patch cords y accesorios                1 lote                          2,000          2,000
  **Total estimado**                                                              **Q 42,990**

## 16. Consideraciones de escalabilidad futura

El diseño deja capacidad para crecimiento mediante espacio adicional en
el rack, puertos disponibles, reserva de cable, canalización con
capacidad adicional, UPS con margen y enlaces troncales de fibra.

## 17. Informe de desarrollo

El diseño de QuetzalDev S.A. se realizó a partir del plano
arquitectónico proporcionado. El MDF se ubicó en la zona central,
adyacente al Hall Central, con el objetivo de reducir la distancia
promedio hacia los departamentos y facilitar el tendido troncal.

Se seleccionó una topología jerárquica de estrella extendida. El switch
principal del MDF concentra los enlaces troncales hacia los ocho
switches departamentales, mientras cada departamento utiliza una
topología en estrella para sus dispositivos finales.

Para el cableado horizontal se seleccionó UTP Cat 6A por sus
características de capacidad y facilidad de instalación. Para el troncal
se propuso fibra OM3 por su capacidad, inmunidad a interferencias y
escalabilidad.

El diseño también contempla patch panels, rack de piso, ODF, UPS,
canalización y etiquetado. Los puntos horizontales se identifican
mediante `[Departamento]-PR##` y los enlaces troncales mediante
`MDF-[Departamento]`.

Finalmente, se consideró crecimiento futuro mediante reserva de espacio,
puertos, canalización, cable y capacidad eléctrica.

## 18. Resumen de decisiones

  Elemento                      Decisión
  ----------------------------- -------------------------------------
  Topología general             Estrella extendida / jerárquica
  Topología departamental       Estrella
  MDF                           Zona central, junto al Hall Central
  Switches                      1 principal + 8 departamentales
  Puntos de red                 48
  Cable horizontal              UTP Cat 6A
  Cable troncal                 Fibra multimodo OM3
  Patch panels                  2 × 24 puertos
  Rack                          Piso, 42U
  Canalización                  Escalerilla cerrada + canaleta
  UPS                           Aproximadamente 1500 VA
  Estándar horizontal           T568B
  Conexiones de cobre a hosts   Straight-through
  Etiquetado horizontal         `[Depto]-PR##`
  Etiquetado troncal            `MDF-[Depto]`
  Bobinas UTP                   1 × 305 m, cálculo base

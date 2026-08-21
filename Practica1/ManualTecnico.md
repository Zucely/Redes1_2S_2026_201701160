# Manual Técnico — Práctica 1: QuetzalDev S.A.
María Zucely Hernández García
201701160


## 1. Inventario de equipos

| Ítem | Cantidad | Ubicación |
|---|---|---|
| Switch principal (core) | 1 | MDF |
| Switch de departamento | 7 | Uno por depto |
| Patch panel | _[completar en sección 5]_ | MDF |
| Rack/gabinete | 1 | MDF |
| UPS | _[completar en sección 6]_ | MDF |
| Tomas de red unitarias | _[completar]_ | Por punto |
| Tomas de red dobles/triples | _[completar]_ | Por punto |

## 2. Ubicación y justificación del MDF

El cuarto de telecomunicaciones (MDF) se ubica en el **Hall Central** (26m x 2m), en el punto medio del pasillo que conecta ambas filas de departamentos.

**Justificación:** el Hall Central atraviesa todo el ancho del edificio (26m) y separa las dos filas de oficinas (Recepción/RRHH/Legal arriba, Capacitación/Diseño/Dirección abajo), por lo que minimiza la distancia promedio hacia todos los puntos de red sin ocupar espacio productivo de ningún departamento. Además facilita el tendido de cableado troncal en ambas direcciones desde un único punto central.

## 3. Distribución de equipos por departamento

| Depto | Escritorio | Laptop | Servidor | Total puntos de red |
|---|---|---|---|---|
| Recepción | 3 | 0 | 1 | 4 |
| Recursos Humanos | 6 | 2 | 0 | 8 |
| Legal | 3 | 1 | 0 | 4 |
| Sala de Capacitación | 10 | 0 | 0 | 10 |
| Diseño e Innovación | 4 | 3 | 1 | 8 |
| Dirección General | 1 | 3 | 0 | 4 |
| Backend | 3 | 3 | 1 | 7 |
| Data Center | 0 | 0 | 3 | 3 |
| **Total** | **30** | **12** | **6** | **48** |

## 4. Justificación de topología por departamento

_[completar — para cada depto: tipo de topología (usualmente estrella extendida desde su switch hacia el MDF), y por qué según cantidad de hosts / criticidad / costo-escalabilidad-tolerancia a fallos]_

| Depto | Topología | Justificación |
|---|---|---|
| Recepción | | |
| Recursos Humanos | | |
| Legal | | |
| Sala de Capacitación | | |
| Diseño e Innovación | | |
| Dirección General | | |
| Backend | | |
| Data Center | | |

## 5. Tipo y categoría de cable por segmento

_[completar — ej. Cat6 UTP para horizontal, fibra o Cat6A para troncal si la distancia/ancho de banda lo justifica]_

| Segmento | Tipo de cable | Categoría | Justificación |
|---|---|---|---|
| Horizontal (switch↔hosts) | | | |
| Troncal (MDF↔switch depto) | | | |

## 6. Distancias estimadas y cálculo de bobinas

| Depto | Distancia troncal (MDF↔switch) | Horizontal interno (prom.) | Puntos de red | Metros totales |
|---|---|---|---|---|
| Recepción | 14.5 m | 3 m | 4 | 70 m |
| Recursos Humanos | 7.5 m | 3 m | 8 | 84 m |
| Legal | 15.5 m | 3 m | 4 | 74 m |
| Sala de Capacitación | 14 m | 3 m | 10 | 170 m |
| Diseño e Innovación | 9 m | 3 m | 8 | 96 m |
| Dirección General | 17 m | 3 m | 4 | 80 m |
| Backend | 20.5 m | 3 m | 7 | 164.5 m |
| Data Center | 19.5 m | 3 m | 3 | 67.5 m |
| **Total** | | | | **≈806 m** |

**Cálculo de bobinas:** 806 m ÷ 305 m (bobina estándar) = 2.64 → **se requieren 3 bobinas**, dejando margen para desperdicio de poncheo y crecimiento futuro.

_Nota metodológica: el enunciado pide medir "distancia hasta el cuarto de telecomunicaciones" para dimensionar la compra de cable. Aquí se sumó el tramo troncal (MDF↔switch de depto) más un promedio de cableado horizontal interno, ya que la compra de material cubre ambos tramos del proyecto completo._

## 7. Cableado troncal

_[completar — ruta de cada enlace troncal y medio de transmisión elegido, considerando velocidad de uplink, distancia y costo/escalabilidad]_

| Enlace | Medio de transmisión | Justificación |
|---|---|---|
| MDF – Recepción | | |
| MDF – RRHH | | |
| MDF – Legal | | |
| MDF – Capacitación | | |
| MDF – Diseño e Innovación | | |
| MDF – Dirección General | | |
| MDF – Backend | | |
| MDF – Data Center | | |

## 8. Equipo activo: patch panel y switch

- Total de puntos de red del edificio: **48**
- Patch panel del edificio: _[completar, ≥ 48 puertos]_
- Switch principal (core): _[completar, puertos ≥ patch panel]_

_[Justificar función de cada elemento: switch, patch panel, ODF si aplica]_

## 9. Canalización

_[completar — tipo de canalización para troncal y horizontal: escalerilla metálica abierta/cerrada u otro medio, justificando por segmento]_

## 10. Rack o gabinete

_[completar — rack de piso vs gabinete de pared para el MDF, justificando según cantidad de equipo a alojar]_

## 11. Respaldo de energía (UPS)

_[completar — consumo eléctrico estimado del switch central + switches de depto, y capacidad de UPS recomendada]_

## 12. Estándares T568A/T568B

| Enlace | Straight-through / Crossover | Justificación técnica |
|---|---|---|
| Switch depto – PC/laptop | | |
| Switch depto – Servidor | | |
| MDF – Switch depto | | |
| Switch – Switch (si aplica) | | |

### Disposición de pines

**Cable Straight-through (T568B ambos extremos):**

| Pin | Color |
|---|---|
| 1 | |
| 2 | |
| 3 | |
| 4 | |
| 5 | |
| 6 | |
| 7 | |
| 8 | |

**Cable Crossover (T568A un extremo, T568B el otro):**

| Pin | Extremo A | Extremo B |
|---|---|---|
| 1 | | |
| 2 | | |
| 3 | | |
| 4 | | |
| 5 | | |
| 6 | | |
| 7 | | |
| 8 | | |

## 13. Etiquetado de cableado

**Formato horizontal:** `[Depto]-PR##` — ej. `Recepcion-PR01`
**Formato troncal:** `MDF-[Depto]` — ej. `MDF-Recepcion`

| Cable | Etiqueta |
|---|---|
| _[completar tabla completa]_ | |

### Comparación con TIA/EIA-606

_[completar — investigar el estándar y comparar contra el formato simplificado usado aquí; señalar mínimo 2 diferencias concretas (ej. identificadores únicos, codificación por colores, registro de espacios/rutas) y una razón de por qué en un entorno real se usaría el estándar completo]_

## 14. Flujo de conexión end-to-end

_[completar — ejemplo: PC → toma de red → cableado horizontal → patch panel de depto → switch de depto → cableado troncal → patch panel MDF → switch principal]_

## 15. Presupuesto estimado

| Ítem | Cantidad | Precio unitario | Subtotal |
|---|---|---|---|
| Bobina UTP Cat6 (305m) | 3 | | |
| Switch de departamento | 7 | | |
| Switch principal | 1 | | |
| Patch panel | 1 | | |
| Rack/gabinete | 1 | | |
| UPS | 1 | | |
| Tomas de red | 48 | | |
| **Total** | | | |

# Invariantes y escenarios de aceptación — Actividad 3

## Invariantes

Reglas que siempre deben cumplirse. Las protege el dominio (la entidad o el objeto de valor), no la base de datos ni la interfaz.

| ID | Invariante | Quién la protege |
|---|---|---|
| INV-01 | Una entrega tiene como máximo un repartidor activo | `Delivery` |
| INV-02 | Solo una entrega en estado *Pendiente* puede recibir repartidor | `Delivery` |
| INV-03 | No se asigna un repartidor que no está disponible | `Delivery` (con `CourierAvailability`) |
| INV-04 | No se asigna un repartidor saturado (ruta llena) | `Delivery` (con `CourierAvailability`) |
| INV-05 | Toda entrega tiene dirección no vacía y una ventana con inicio anterior al fin | `DeliveryAddress` y `DeliveryWindow` |
| INV-06 | Una entrega no puede estar en dos estados a la vez ni saltarse las transiciones permitidas | `Delivery` |
| INV-07 | Toda asignación exitosa queda registrada y genera el evento `RepartidorAsignado` | `Delivery` y caso de uso |

## Escenarios de aceptación (Dado-Cuando-Entonces)

### EA-01 — Asignación exitosa (INV-01, INV-02, INV-07)
- **Dado** una entrega *Pendiente* con dirección y ventana válidas y un repartidor disponible con carga 3 de capacidad 8
- **Cuando** el operador asigna al repartidor a la entrega
- **Entonces** la entrega queda *Asignada* con ese repartidor, se guarda, se publica `RepartidorAsignado` y el cambio queda registrado

### EA-02 — Entrega ya asignada (INV-01, INV-02)
- **Dado** una entrega *Asignada* al repartidor A
- **Cuando** se intenta asignar al repartidor B
- **Entonces** se rechaza con el mensaje "entrega ya asignada", la entrega sigue con el repartidor A y no se publica ningún evento

### EA-03 — Repartidor saturado (INV-04)
- **Dado** un repartidor con carga 8 de capacidad 8 y una entrega *Pendiente*
- **Cuando** se intenta asignar la entrega a ese repartidor
- **Entonces** se rechaza con el mensaje "repartidor saturado" y la entrega sigue *Pendiente*

### EA-04 — Repartidor no disponible (INV-03)
- **Dado** un repartidor fuera de turno y una entrega *Pendiente*
- **Cuando** se intenta asignar la entrega a ese repartidor
- **Entonces** se rechaza con el mensaje "repartidor no disponible" y la entrega sigue *Pendiente*

### EA-05 — Entrega inexistente
- **Dado** un identificador de entrega que no existe
- **Cuando** se intenta asignar un repartidor
- **Entonces** se rechaza con el mensaje "entrega no encontrada" y no se guarda nada

### EA-06 — Entrega en estado no asignable (INV-02, INV-06)
- **Dado** una entrega *Cancelada* (o *Entregada*)
- **Cuando** se intenta asignar un repartidor
- **Entonces** se rechaza y la entrega conserva su estado

### EA-07 — Falla el servicio de disponibilidad (ES-04 de la Actividad 2)
- **Dado** que el servicio que informa la disponibilidad no responde
- **Cuando** se intenta asignar un repartidor
- **Entonces** la entrega sigue *Pendiente*, no se guarda nada y se informa un error temporal que permite reintentar

### EA-08 — Falla la notificación (ES-04, RNF-02)
- **Dado** una asignación ya guardada y un sistema de notificaciones que no responde
- **Cuando** se intenta publicar `RepartidorAsignado`
- **Entonces** la asignación se mantiene y el evento queda pendiente de reenvío

### EA-09 — Dirección inválida (INV-05)
- **Dado** una dirección vacía
- **Cuando** se intenta crear la entrega
- **Entonces** se rechaza con el mensaje "la dirección es obligatoria"

### EA-10 — Ventana inválida (INV-05)
- **Dado** una ventana cuyo inicio es posterior a su fin
- **Cuando** se intenta crear la entrega
- **Entonces** se rechaza con el mensaje "la ventana de entrega es inválida"

## Cobertura

| Invariante | Escenarios que la comprueban |
|---|---|
| INV-01 | EA-01, EA-02 |
| INV-02 | EA-01, EA-02, EA-06 |
| INV-03 | EA-04 |
| INV-04 | EA-03 |
| INV-05 | EA-09, EA-10 |
| INV-06 | EA-06 |
| INV-07 | EA-01, EA-08 |

# Flujo de negocio seleccionado — Actividad 3

## Flujo: asignar un repartidor a una entrega

**Por qué este flujo:** es el punto donde se cruzan varios riesgos de la Actividad 1 (mala coordinación entre almacén, rutas y soporte; entregas retrasadas; pérdida de trazabilidad) y los requisitos RF-06, RNF-04 y RNF-06 de la Actividad 2. Además es un flujo acotado, adecuado para diseñar sin implementar todavía.

## Propósito

Que cada entrega confirmada quede a cargo de **exactamente un repartidor disponible y con capacidad**, y que las demás partes del sistema se enteren de ese resultado.

## Actores

| Actor | Papel en el flujo |
|---|---|
| Operador de tienda / Administrador de almacén | Inicia la asignación |
| Repartidor | Recibe la entrega asignada |
| Cliente | Es informado de que su pedido ya tiene repartidor |
| Servicio de geolocalización (externo) | Informa la disponibilidad y la carga del repartidor |
| Sistema de notificaciones (externo) | Avisa a repartidor y cliente |

## Disparador y precondiciones

- **Disparador:** existe una entrega en estado *Pendiente* (se crea cuando un pedido es confirmado en el contexto de Pedidos).
- **Precondición 1:** la entrega tiene dirección y ventana de entrega válidas.
- **Precondición 2:** el repartidor propuesto existe en el sistema.

## Flujo principal

1. El operador elige una entrega pendiente y un repartidor.
2. El sistema carga la entrega.
3. El sistema verifica que la entrega exista y no tenga repartidor activo.
4. El sistema consulta la disponibilidad y la carga del repartidor.
5. La entrega valida sus propias reglas (invariantes) y queda *Asignada*.
6. El sistema guarda la entrega.
7. El sistema publica el resultado (`RepartidorAsignado`) para notificar y registrar.

## Flujos alternativos y de error

| Situación | Resultado |
|---|---|
| La entrega no existe | Se rechaza; no se guarda nada |
| La entrega ya tiene repartidor | Se rechaza; la entrega no cambia |
| La entrega no está *Pendiente* (cancelada, entregada...) | Se rechaza |
| El repartidor no está disponible | Se rechaza; la entrega sigue *Pendiente* |
| El repartidor está saturado (ruta llena) | Se rechaza; la entrega sigue *Pendiente* |
| El servicio de disponibilidad no responde | No se asigna; se informa un error temporal para reintentar |
| Falla el envío de la notificación | La asignación se mantiene; el aviso se reintenta después |

## Resultado esperado

- La entrega queda *Asignada* con un solo repartidor.
- El cambio queda guardado y registrado (trazabilidad).
- Se publica el evento `RepartidorAsignado`.

## Fuera de alcance de esta actividad

Crear la entrega, optimizar rutas, reasignar repartidores, seguimiento en vivo, persistencia real e integraciones reales (corresponden a la Actividad 4).

## Relación con actividades anteriores

- **Actividad 1:** riesgos de mala coordinación, entregas retrasadas y pérdida de trazabilidad.
- **Actividad 2:** RF-06 (asignar rutas y repartidores), RNF-04 (trazabilidad), RNF-06 (evolución), escenarios ES-04 y ES-06, y la decisión del monolito modular (ADR-001).

# Escenarios de calidad — Actividad 2

Un escenario de calidad describe una situación concreta y cómo debe responder el sistema, con una **medida** que se pueda comprobar. Las medidas son valores iniciales supuestos que deben confirmarse con el negocio.

## ES-01 — Rendimiento (RNF-01)

- **Fuente:** un cliente.
- **Estímulo:** consulta el estado de su pedido.
- **Entorno:** operación normal, con 1.000 pedidos por hora.
- **Respuesta:** el sistema muestra el estado actualizado del pedido.
- **Medida:** en menos de 2 segundos para el 95 % de las consultas.

## ES-02 — Consistencia de inventario (RNF-05)

- **Fuente:** dos canales de venta distintos.
- **Estímulo:** piden al mismo tiempo la última unidad de un producto.
- **Entorno:** operación normal, con inventario distribuido en varios almacenes.
- **Respuesta:** el sistema confirma solo un pedido e informa al otro que no hay stock.
- **Medida:** 0 pedidos confirmados sobre stock inexistente.

## ES-03 — Pedidos duplicados (RF-02)

- **Fuente:** un canal de venta.
- **Estímulo:** envía el mismo pedido dos veces (por ejemplo, por un reintento o un doble clic).
- **Entorno:** operación normal.
- **Respuesta:** el sistema detecta el duplicado y conserva un solo pedido.
- **Medida:** al menos el 99 % de los duplicados con el mismo identificador se detectan.

## ES-04 — Disponibilidad ante fallas externas (RNF-02)

- **Fuente:** el sistema de notificaciones externo.
- **Estímulo:** deja de responder.
- **Entorno:** horario operativo, con entregas en curso.
- **Respuesta:** el sistema sigue registrando pedidos y asignando rutas; las notificaciones quedan en espera y se reenvían cuando el servicio vuelve.
- **Medida:** 0 pedidos perdidos; notificaciones pendientes enviadas en menos de 15 minutos tras la recuperación.

## ES-05 — Seguridad (RNF-03)

- **Fuente:** un repartidor.
- **Estímulo:** intenta ver pedidos de otra ruta o datos de pago de un cliente.
- **Entorno:** operación normal.
- **Respuesta:** el sistema niega el acceso y registra el intento.
- **Medida:** 100 % de los accesos no autorizados bloqueados y registrados; datos sensibles cifrados.

## ES-06 — Facilidad de evolución (RNF-06)

- **Fuente:** el negocio.
- **Estímulo:** pide cambiar la regla de asignación de rutas (por ejemplo, priorizar entregas urgentes).
- **Entorno:** el sistema ya está en producción.
- **Respuesta:** el cambio se hace sin modificar pedidos ni inventario.
- **Medida:** el cambio afecta a un solo módulo y se completa, con pruebas, en 2 días o menos.

## ES-07 — Trazabilidad (RNF-04)

- **Fuente:** un agente de soporte.
- **Estímulo:** consulta el historial completo de un pedido reclamado.
- **Entorno:** operación normal.
- **Respuesta:** el sistema muestra todos los cambios de estado, con fecha y responsable.
- **Medida:** historial completo en menos de 3 segundos; 100 % de los cambios registrados.

## ES-08 — Escalabilidad (RNF-07)

- **Fuente:** una campaña comercial.
- **Estímulo:** el volumen sube de 1.000 a 3.000 pedidos por hora.
- **Entorno:** operación con alta demanda.
- **Respuesta:** el sistema absorbe la carga aumentando recursos, sin reescribirse.
- **Medida:** se mantiene la consulta de estado en menos de 2 segundos para el 95 % de los casos (ES-01).

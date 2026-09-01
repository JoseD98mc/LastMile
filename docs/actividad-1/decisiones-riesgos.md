# Registro de decisiones iniciales y riesgos — Actividad 1

## Decisión inicial

Se propone una plataforma centralizada que permita registrar pedidos, validar inventario, asignar rutas, rastrear entregas y gestionar incidencias desde un único sistema.

## Supuestos y decisiones

### Qué asumimos

- Los canales de venta ya existen y solo necesitan enviar el pedido al sistema (no se construye la tienda en línea desde cero).
- Los repartidores cuentan con un dispositivo móvil para reportar su ubicación.

### Qué decisiones ya se toman

- Habrá un sistema central único que registra el pedido desde que se crea hasta que se entrega.
- La información de inventario, rutas y entregas se consulta en un solo lugar, no en archivos separados.

### Qué falta confirmar

- Cuántos almacenes o bodegas maneja la empresa hoy.
- Qué tan seguido cambia el inventario, para definir qué tan "en tiempo real" debe ser la validación.
- Si el ERP/CRM interno ya existe o también hay que integrarlo desde cero.

## Riesgos identificados

| Riesgo | Descripción |
|---|---|
| Duplicación de pedidos | Un mismo pedido se registra más de una vez por venir de canales distintos. |
| Inconsistencias de inventario | El inventario mostrado no coincide con el disponible en almacén. |
| Entregas fallidas o retrasadas | Fallas en la asignación o seguimiento de rutas. |
| Pérdida de trazabilidad | No es posible reconstruir el estado histórico de un pedido. |
| Mala coordinación entre almacén, rutas y soporte | Falta de información compartida entre áreas. |
| Crecimiento operativo sin control | El sistema no soporta el aumento de pedidos, rutas o usuarios. |

# Catálogo de requisitos — Actividad 2

Este catálogo parte del diagnóstico de la Actividad 1: la empresa recibe pedidos por varios canales, tiene inventario distribuido, coordina rutas y repartidores, y no tiene una vista centralizada del estado real de cada pedido.

## Requisitos funcionales (qué debe hacer el sistema)

| ID | Requisito | Actor principal | Riesgo de la Actividad 1 que atiende |
|---|---|---|---|
| RF-01 | Registrar pedidos de todos los canales en un solo lugar | Operador de tienda / Cliente | Duplicación de pedidos, pérdida de trazabilidad |
| RF-02 | Detectar pedidos duplicados antes de guardarlos | Sistema | Duplicación de pedidos |
| RF-03 | Validar la disponibilidad de inventario al confirmar un pedido | Sistema / Administrador de almacén | Inconsistencias de inventario |
| RF-04 | Consultar y actualizar el inventario por almacén | Administrador de almacén | Inconsistencias de inventario |
| RF-05 | Confirmar el pago con el sistema de pagos externo | Cliente / Sistema de pagos | Errores operativos |
| RF-06 | Asignar pedidos a rutas y repartidores | Operador / Administrador de almacén | Entregas retrasadas, mala coordinación |
| RF-07 | Rastrear la entrega (estado y ubicación) | Cliente / Repartidor | Pérdida de trazabilidad |
| RF-08 | Notificar al cliente cada cambio de estado de su pedido | Sistema de notificaciones | Mala experiencia del cliente |
| RF-09 | Registrar y gestionar incidencias (entrega fallida, reclamos) | Soporte / Repartidor | Mala coordinación entre áreas |
| RF-10 | Consultar el historial completo de un pedido | Soporte / Operador | Pérdida de trazabilidad |
| RF-11 | Sincronizar datos con el ERP/CRM interno | ERP/CRM | Duplicación de información |
| RF-12 | Gestionar usuarios y permisos según su rol | Administrador | Baja seguridad |

## Requisitos no funcionales (qué tan bien debe hacerlo)

Las metas son valores iniciales **supuestos**; deben confirmarse con el negocio.

| ID | Atributo de calidad | Descripción | Meta inicial |
|---|---|---|---|
| RNF-01 | Rendimiento | Consultar el estado de un pedido debe ser rápido | Menos de 2 s en el 95 % de las consultas |
| RNF-02 | Disponibilidad | El sistema debe funcionar mientras hay entregas en curso | 99,5 % en horario operativo (06:00–22:00) |
| RNF-03 | Seguridad | Proteger datos personales y de pago; acceso según rol | Datos sensibles cifrados; 0 accesos sin permiso |
| RNF-04 | Trazabilidad | Cada cambio de estado de un pedido queda registrado | 100 % de cambios con fecha y responsable |
| RNF-05 | Consistencia de inventario | No confirmar pedidos sobre stock que no existe | 0 pedidos confirmados sin stock |
| RNF-06 | Facilidad de evolución | Cambiar reglas de negocio sin reescribir todo el sistema | Un cambio de regla afecta a un solo módulo |
| RNF-07 | Escalabilidad | Soportar el crecimiento de pedidos | 1.000 pedidos/hora hoy; hasta 3.000 sin rediseñar |
| RNF-08 | Interoperabilidad | Integrar sistemas externos con contratos claros | Cada integración con contrato (API) documentado |
| RNF-09 | Costo operativo | Infraestructura y equipo manejables | Un solo despliegue inicial |
| RNF-10 | Usabilidad | El repartidor actualiza el estado de una entrega rápido | 3 pasos o menos por actualización |

## Restricciones

- Esta actividad decide la **estructura**, no las tecnologías: no se eligen lenguajes ni frameworks todavía.
- El sistema debe integrarse con sistemas que ya existen (pagos, geolocalización, notificaciones, ERP/CRM), cuya disponibilidad no controlamos.
- Se asume un equipo y un presupuesto limitados.
- Se asume una operación con crecimiento moderado, no explosivo.
- El alcance es el definido en la Actividad 1 (gestión de pedidos y entregas de última milla).

## Supuestos a confirmar

- Los números de las metas (2 s, 99,5 %, 1.000 y 3.000 pedidos/hora) son estimaciones iniciales.
- Cuántos almacenes y repartidores maneja la empresa hoy.
- Si el ERP/CRM interno ofrece una forma estándar de integración.

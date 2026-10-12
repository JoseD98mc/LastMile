# Glosario de lenguaje ubicuo — Actividad 3

El lenguaje ubicuo es el vocabulario común entre negocio y equipo técnico. Cada término se usa igual en las conversaciones, los documentos y el código.

## Términos del flujo

| Término (negocio) | Nombre en código | Definición | Tipo |
|---|---|---|---|
| Entrega | `Delivery` | Traslado de un pedido confirmado hasta la dirección del cliente | Entidad (raíz) |
| Repartidor | `Courier` | Persona que realiza entregas | Entidad |
| Pedido | `Order` | Compra confirmada del cliente; en este contexto solo se conoce por su identificador (`OrderId`) | Referencia externa |
| Asignación | `Assignment` | Acto de dejar una entrega a cargo de un repartidor | Acción del dominio |
| Dirección de entrega | `DeliveryAddress` | Lugar donde se entrega; no puede estar vacía | Objeto de valor |
| Ventana de entrega | `DeliveryWindow` | Rango de tiempo en que debe entregarse; el inicio es anterior al fin | Objeto de valor |
| Prioridad | `Priority` | Nivel de importancia de la entrega (normal o urgente) | Objeto de valor |
| Disponibilidad del repartidor | `CourierAvailability` | Si el repartidor está disponible y cuántas entregas lleva frente a su máximo | Objeto de valor |
| Carga | `CurrentLoad` | Número de entregas activas que lleva un repartidor | Dato de la disponibilidad |
| Capacidad | `MaxLoad` | Número máximo de entregas activas que puede llevar | Dato de la disponibilidad |
| Saturado | `IsSaturated` | Carga igual o mayor a la capacidad | Regla del dominio |
| Estado de entrega | `DeliveryStatus` | Pendiente, Asignada, En ruta, Entregada, Fallida, Cancelada | Objeto de valor (enumeración) |
| Entrega activa | — | Entrega en estado Asignada o En ruta | Concepto |
| Caso de uso | `UseCase` | Intención del usuario o del sistema (por ejemplo, asignar repartidor) | Capa de aplicación |
| Puerto | `Port` | Interfaz que expresa lo que el caso de uso necesita | Capa de aplicación |
| Adaptador | `Adapter` | Implementación concreta de un puerto (base de datos, API, notificaciones) | Infraestructura |
| Invariante | — | Regla que siempre debe cumplirse | Dominio |

## Términos ambiguos que deben confirmarse

| Término | Duda | Supuesto actual | A quién confirmar |
|---|---|---|---|
| Disponible | ¿Está en turno, no tiene entregas activas o tiene la ubicación activa? | En turno y con ubicación activa; lo informa el servicio de disponibilidad | Operaciones |
| Ruta saturada / capacidad | ¿Se mide por número de entregas, peso, volumen o tiempo? | Número máximo de entregas activas por repartidor (valor supuesto: 8) | Operaciones |
| Entrega activa | ¿Cuenta desde que se asigna o desde que sale a ruta? | Desde que queda Asignada | Operaciones |
| Pedido vs. entrega | ¿Un pedido puede generar varias entregas (parciales)? | Un pedido genera una entrega | Negocio |
| Urgente | ¿Quién define la prioridad y con qué criterio? | La define el operador al crear la entrega | Negocio |
| Reasignación | ¿Se permite cambiar de repartidor? ¿Quién la autoriza y hasta qué estado? | No se modela en esta actividad | Operaciones |
| Fallida | ¿Una entrega fallida se reprograma como la misma entrega o como una nueva? | Estado final por ahora | Negocio |
| Repartidor | ¿Es empleado propio o tercero? Afecta los datos y la seguridad | Se modela solo su identidad | Negocio / Legal |

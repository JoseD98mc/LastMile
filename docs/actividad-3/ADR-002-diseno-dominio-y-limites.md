# ADR-002: Diseño del dominio y límites del flujo "asignar repartidor"

- **Estado:** Aceptada
- **Fecha:** 2026-10-12
- **Proyecto:** LogiCentral — plataforma de gestión logística de última milla
- **Relacionada con:** ADR-001 (monolito modular)

## Contexto

Con el monolito modular decidido (ADR-001), hay que diseñar el modelo de dominio y los límites de un flujo concreto antes de construir integraciones: asignar un repartidor a una entrega. El diseño debe proteger las reglas del negocio, separarlas de la infraestructura y poder cambiar sin depender de una base de datos, una API externa o un framework.

## Decisiones

### D1. Tres capas con dependencias hacia adentro

Se usan tres capas: Dominio, Aplicación e Infraestructura. La infraestructura depende de la aplicación y esta del dominio.

- **Alternativa descartada:** arquitectura por capas tradicional (interfaz → negocio → datos), donde el negocio depende de la capa de datos.
- **Trade-off:** hay más interfaces y archivos, a cambio de que el negocio sea independiente y fácil de probar.

### D2. La entidad `Delivery` protege sus invariantes

Las reglas de la asignación (un solo repartidor, estado *Pendiente*, repartidor disponible y no saturado) están dentro de la entidad.

- **Alternativa descartada:** modelo "anémico", con reglas en el caso de uso o en la base de datos.
- **Trade-off:** la entidad recibe más responsabilidad, pero las reglas no se pueden saltar desde otro punto del sistema.

### D3. La disponibilidad entra por un puerto como objeto de valor

El caso de uso consulta `ICourierAvailabilityPort` y entrega a la entidad un `CourierAvailability`. El dominio decide si el repartidor está saturado o no disponible.

- **Alternativa descartada:** llamar a la API de geolocalización desde el caso de uso, o dejar la regla de saturación en el adaptador.
- **Trade-off:** el dato puede cambiar entre la consulta y el guardado, por lo que dos asignaciones simultáneas podrían saturar al mismo repartidor. Se mitiga más adelante con control de concurrencia al guardar (Actividad 4).

### D4. Los contextos se comunican por eventos y se referencian por identificador

Pedidos publica `PedidoConfirmado`; Entregas publica `RepartidorAsignado`. Las referencias entre contextos son solo `OrderId` y `CourierId`.

- **Alternativa descartada:** compartir clases o tablas entre módulos.
- **Trade-off:** se duplica un poco de información y se definen contratos, pero cada módulo puede cambiar sin romper a los demás y quedan listos para separarse si el crecimiento lo exige.

### D5. Publicar el resultado después de guardar; un fallo no revierte la asignación

La asignación se guarda primero y luego se publica el evento. Si la notificación falla, la asignación se mantiene y el aviso se reintenta.

- **Alternativa descartada:** una transacción que incluya guardar y notificar, que obligaría a deshacer asignaciones válidas por un fallo externo.
- **Trade-off:** una notificación puede llegar tarde (consistencia eventual), a cambio de cumplir ES-04 y RNF-02.

### D6. No se usa máquina de estados de interfaz

- **Alternativa descartada:** introducir una máquina de estados de front-end (video 25).
- **Justificación:** este flujo no tiene estados de pantalla complejos. Los estados de la entrega son del dominio y los controla la entidad.

### D7. Sin persistencia ni integraciones reales por ahora

Esta actividad define puertos y contratos; los adaptadores reales se construyen en la Actividad 4. Para probar el diseño se pueden usar adaptadores en memoria.

- **Trade-off:** no hay un flujo funcionando todavía, pero se evita elegir tecnologías antes de validar el diseño.

## Justificación general

- Protege las reglas críticas en un solo lugar (dominio).
- Mantiene el negocio independiente de la tecnología.
- Es coherente con el monolito modular y con los riesgos de las Actividades 1 y 2.
- Permite comprobar cada invariante con los escenarios EA-01 a EA-10.

## Consecuencias

**Positivas**
- Reglas de negocio claras, probadas sin base de datos ni red.
- Cambios de tecnología sin tocar el dominio.
- Contratos explícitos entre módulos.

**Negativas / trade-offs aceptados**
- Más código de estructura (interfaces y adaptadores).
- Posible concurrencia al asignar (mitigación pendiente).
- Notificaciones con consistencia eventual.

## Riesgos y mitigaciones

| Riesgo | Mitigación |
|---|---|
| Dos asignaciones simultáneas saturan a un repartidor | Control de concurrencia al guardar (Actividad 4) |
| Reglas de negocio filtradas a los adaptadores | Revisar que los adaptadores solo traduzcan datos |
| Términos ambiguos mal interpretados | Confirmar el glosario con el negocio (`02-glosario-lenguaje-ubicuo.md`) |
| Sobrediseño para un flujo pequeño | Aplicar este patrón solo a los flujos con reglas críticas |

## Referencias

- `01-flujo-negocio.md` a `07-diagrama-c4.md`
- `ADR-001-estructura-plataforma.md` (Actividad 2)
- `Architecture.md` en la raíz del repositorio

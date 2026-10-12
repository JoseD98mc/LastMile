# Matriz de priorización — Actividad 2

## Cómo se priorizó

Cada requisito se evaluó con cuatro preguntas:

- **Prioridad:** qué tan necesario es para que el negocio funcione (Alta / Media / Baja).
- **Impacto en negocio:** qué pasa en el negocio si falla (Alto / Medio / Bajo).
- **Impacto en arquitectura:** cuánto influye en cómo se organiza el sistema (Alto / Medio / Bajo).
- **Complejidad:** qué tan difícil es lograrlo (Alta / Media / Baja).

Regla usada: los requisitos con **prioridad Alta e impacto Alto en la arquitectura** son los que más pesan en la decisión estructural.

## Matriz

| ID | Requisito | Prioridad | Impacto negocio | Impacto arquitectura | Complejidad |
|---|---|---|---|---|---|
| RF-01 | Registrar pedidos de todos los canales | Alta | Alto | Alto | Media |
| RF-02 | Detectar pedidos duplicados | Alta | Alto | Medio | Media |
| RF-03 | Validar disponibilidad de inventario | Alta | Alto | Alto | Alta |
| RF-04 | Consultar y actualizar inventario por almacén | Alta | Alto | Alto | Media |
| RF-05 | Confirmar pago con sistema externo | Alta | Alto | Medio | Media |
| RF-06 | Asignar pedidos a rutas y repartidores | Alta | Alto | Alto | Alta |
| RF-07 | Rastrear la entrega | Alta | Alto | Medio | Media |
| RF-08 | Notificar cambios de estado | Media | Medio | Bajo | Baja |
| RF-09 | Gestionar incidencias | Media | Medio | Medio | Media |
| RF-10 | Consultar historial del pedido | Alta | Alto | Medio | Baja |
| RF-11 | Sincronizar con ERP/CRM | Media | Medio | Medio | Media |
| RF-12 | Gestionar usuarios y roles | Alta | Alto | Medio | Media |
| RNF-01 | Rendimiento | Alta | Alto | Medio | Media |
| RNF-02 | Disponibilidad | Alta | Alto | Alto | Alta |
| RNF-03 | Seguridad | Alta | Alto | Alto | Media |
| RNF-04 | Trazabilidad | Alta | Alto | Alto | Media |
| RNF-05 | Consistencia de inventario | Alta | Alto | Alto | Alta |
| RNF-06 | Facilidad de evolución | Alta | Medio | Alto | Media |
| RNF-07 | Escalabilidad | Media | Medio | Alto | Alta |
| RNF-08 | Interoperabilidad | Media | Medio | Alto | Media |
| RNF-09 | Costo operativo | Media | Alto | Medio | Baja |
| RNF-10 | Usabilidad | Media | Medio | Bajo | Baja |

## Requisitos que más influyen en la decisión estructural

1. **RNF-05 Consistencia de inventario:** si dos canales venden la misma unidad, el negocio pierde dinero y confianza.
2. **RNF-04 Trazabilidad:** es el problema central de la Actividad 1 (no hay vista única del pedido).
3. **RNF-06 Facilidad de evolución:** las reglas de rutas e inventario van a cambiar con el tiempo.
4. **RNF-02 Disponibilidad:** una caída detiene entregas en curso.
5. **RNF-03 Seguridad:** el sistema maneja datos personales y de pago.

Funcionalmente, **RF-03 (validar inventario)** y **RF-06 (asignar rutas)** son los más complejos y los que más conectan a distintas áreas.

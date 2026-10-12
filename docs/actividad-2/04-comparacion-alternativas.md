# Comparación de alternativas estructurales — Actividad 2

Se comparan dos formas de organizar la plataforma. La pregunta no es cuál es "la mejor arquitectura", sino **cuál es la más apropiada para este caso**: operación con crecimiento moderado, equipo y presupuesto limitados, y un problema central de trazabilidad y consistencia.

## Dominios identificados (comunes a ambas opciones)

- **Pedidos:** registro, detección de duplicados, estado e historial.
- **Inventario:** stock por almacén y validación de disponibilidad.
- **Entregas y rutas:** asignación de rutas y repartidores, seguimiento.
- **Incidencias y soporte:** entregas fallidas, reclamos.
- **Integraciones y notificaciones:** pagos, geolocalización, notificaciones, ERP/CRM.

## Opción A: monolito modular con límites claros

Un solo sistema que se despliega como una unidad, pero organizado internamente en módulos separados (uno por dominio). Los módulos solo se comunican por interfaces definidas, no accediendo a los datos de los demás.

**Ventajas**
- Más rápido y barato de construir para un crecimiento moderado.
- Los datos de pedidos e inventario viven juntos, lo que facilita mantener el inventario consistente y la trazabilidad.
- Menor complejidad operativa: un solo despliegue que monitorear.
- Si los límites están bien definidos, se puede separar un módulo más adelante.

**Desventajas**
- Puede volverse una base de código enredada si los límites no se respetan.
- Si el volumen crece mucho, hay que escalar todo el sistema junto.
- Una falla grave puede afectar a todos los módulos a la vez.

## Opción B: arquitectura distribuida por dominios y servicios

Cada dominio es un servicio independiente, con su propio despliegue y sus propios datos, que se comunican por red.

**Ventajas**
- Separa mejor inventario, entregas y soporte.
- Permite escalar cada servicio por separado y trabajar por equipos.
- Aísla fallas: si un servicio cae, los otros pueden seguir.

**Desventajas**
- Mayor complejidad operativa: varios despliegues, monitoreo y comunicación entre servicios.
- Mantener el inventario consistente entre servicios es más difícil (los datos están repartidos).
- Más infraestructura y coordinación técnica, con mayor costo.
- Riesgo de sobrediseñar antes de que el negocio lo necesite.

## Evaluación con criterios

Escala de 1 a 5, donde **5 es lo más favorable** (por ejemplo, 5 en costo significa el menor costo). Los pesos reflejan la prioridad del negocio en este caso. Los puntajes son un juicio razonado, no una medición exacta.

| Criterio | Peso | Opción A: monolito modular | Opción B: servicios distribuidos |
|---|---|---|---|
| Costo | 20 % | 5 | 2 |
| Complejidad | 20 % | 4 | 2 |
| Rapidez de implementación | 15 % | 5 | 2 |
| Facilidad de evolución | 15 % | 3 | 5 |
| Riesgo operativo | 15 % | 4 | 2 |
| Alineación con el negocio | 15 % | 4 | 3 |
| **Puntaje ponderado** | 100 % | **4,20** | **2,60** |

Cálculo de A: 5×0,20 + 4×0,20 + 5×0,15 + 3×0,15 + 4×0,15 + 4×0,15 = 4,20.
Cálculo de B: 2×0,20 + 2×0,20 + 2×0,15 + 5×0,15 + 2×0,15 + 3×0,15 = 2,60.

## Cómo responde cada opción a los escenarios clave

| Escenario | Opción A | Opción B |
|---|---|---|
| ES-02 Consistencia de inventario | Pedidos e inventario comparten datos, por lo que se valida de forma directa y segura | Requiere coordinar varios servicios; mayor riesgo de inconsistencias |
| ES-04 Falla de un sistema externo | Se aísla con una cola de reintentos dentro del módulo de integraciones | Se aísla de forma natural, aunque también necesita reintentos |
| ES-06 Cambio de reglas | Se cumple si los módulos respetan sus límites | Se cumple por diseño |
| ES-07 Trazabilidad | El historial está en un solo lugar | Hay que reunir información de varios servicios |
| ES-08 Escalabilidad | Se escala el sistema completo; suficiente hasta 3.000 pedidos/hora | Se escala cada servicio por separado; mejor a volúmenes muy altos |

## Conclusión de la comparación

La Opción B gana en evolución y aislamiento de fallas, pero esas ventajas importan sobre todo con volúmenes y equipos mucho mayores que los del caso. La Opción A resuelve mejor lo que hoy es más importante: **consistencia de inventario, trazabilidad, costo y rapidez**, y deja abierta la puerta a separar módulos en el futuro.

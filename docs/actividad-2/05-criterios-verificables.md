# Criterios verificables — Actividad 2

Estos criterios permiten comprobar si la decisión del ADR-001 (monolito modular) responde a las necesidades del sistema. Cada criterio indica cómo se verifica y a qué requisito o escenario responde.

## Criterios de estructura

| # | Criterio | Cómo se verifica | Relacionado con |
|---|---|---|---|
| C-01 | Existen cinco módulos separados: Pedidos, Inventario, Entregas y rutas, Incidencias y soporte, Integraciones y notificaciones | Revisar la estructura del código | RNF-06 |
| C-02 | Ningún módulo lee ni escribe directamente en los datos de otro; solo usa interfaces públicas | Revisión de dependencias entre módulos | RNF-06, ES-06 |
| C-03 | Las cuatro integraciones externas (pagos, geolocalización, notificaciones, ERP/CRM) pasan por adaptadores con contrato documentado | Revisar el módulo de Integraciones y su documentación | RNF-08 |
| C-04 | El sistema se despliega como una sola unidad | Revisar el proceso de despliegue | RNF-09 |

## Criterios de comportamiento (se prueban con los escenarios)

| # | Criterio | Cómo se verifica | Escenario |
|---|---|---|---|
| C-05 | Con 1.000 pedidos/hora, el 95 % de las consultas de estado responde en menos de 2 s | Prueba de carga | ES-01 |
| C-06 | Si dos pedidos piden la última unidad al mismo tiempo, solo uno se confirma | Prueba de concurrencia | ES-02 |
| C-07 | Un pedido enviado dos veces se guarda una sola vez | Prueba con envíos repetidos | ES-03 |
| C-08 | Si el sistema de notificaciones falla, los pedidos siguen registrándose y las notificaciones se reenvían al recuperarse | Simular la caída del servicio | ES-04 |
| C-09 | Un repartidor no puede ver pedidos de otra ruta ni datos de pago; el intento queda registrado | Prueba de permisos por rol | ES-05 |
| C-10 | Cambiar la regla de asignación de rutas modifica solo el módulo de Entregas y rutas | Revisar qué archivos cambian | ES-06 |
| C-11 | El historial completo de un pedido se muestra en menos de 3 s, con fecha y responsable por cambio | Prueba de consulta | ES-07 |
| C-12 | Con 3.000 pedidos/hora, se mantiene la respuesta de consulta en menos de 2 s para el 95 % | Prueba de carga aumentando recursos | ES-08 |

## Criterios para reevaluar la decisión

La decisión deja de ser válida y debe revisarse si:

- el volumen sostenido supera los 3.000 pedidos por hora,
- un módulo necesita escalar por separado,
- los equipos se bloquean entre sí al trabajar en paralelo,
- los despliegues de un módulo afectan seguido a los demás,
- los límites entre módulos se rompen repetidamente (falla C-02).

## Cómo usar esta lista

Si los criterios C-01 a C-04 se cumplen y las pruebas C-05 a C-12 pasan, la decisión responde a las necesidades. Si alguna falla, se registra y se decide si se corrige la implementación o se revisa el ADR.

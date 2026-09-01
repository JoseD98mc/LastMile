# Análisis del problema — Actividad 1

## Contexto del sistema

La plataforma es una solución de gestión logística de última milla para una empresa de retail digital. La empresa recibe pedidos desde varios canales, maneja inventarios distribuidos, coordina entregas con diferentes rutas y repartidores, y no cuenta con una vista centralizada del estado real de cada pedido.

## Actores

- Cliente
- Operador de comercio o tienda
- Administrador de almacén
- Repartidor
- Soporte al cliente
- Sistema de pagos (externo)
- Servicio de geolocalización (externo)
- Sistema de notificaciones (externo)
- ERP o CRM interno (externo)

## ¿Qué ocurre hoy?

Los pedidos entran por distintos canales (web, tienda física, entre otros) y cada área — almacén, rutas, soporte — maneja su propia información sin conectarse con las demás.

## ¿Por qué es un problema?

Porque nadie tiene una foto completa y actualizada del pedido. El almacén no sabe si ya se asignó una ruta, el repartidor no sabe si hubo cambios y el cliente no cuenta con información confiable.

## ¿Qué consecuencias tiene?

Pedidos duplicados, entregas retrasadas, clientes insatisfechos y un equipo de soporte que pierde tiempo tratando de reconstruir qué pasó con cada pedido.

## ¿Qué variables importan?

El volumen de pedidos por canal, la disponibilidad real de inventario, la cantidad de repartidores y rutas activas, y el tiempo que toma cada etapa (pedido → validación → ruta → entrega).

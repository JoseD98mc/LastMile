# Diagrama C4 actualizado — Actividad 3

El modelo C4 muestra la arquitectura en niveles: contexto, contenedores y componentes. Se actualiza respecto a la Actividad 1 (contexto) y la Actividad 2 (monolito modular).

## Nivel 1: contexto del sistema

```mermaid
flowchart LR
  OP["Operador / Admin. almacén"] --> SYS["LogiCentral<br/>Sistema de gestión logística"]
  CL["Cliente"] --> SYS
  RE["Repartidor"] --> SYS
  SO["Soporte al cliente"] --> SYS
  SYS --> PAG["Sistema de pagos"]
  SYS --> GEO["Geolocalización"]
  SYS --> NOT["Notificaciones"]
  SYS --> ERP["ERP / CRM"]
```

## Nivel 2: contenedores

```mermaid
flowchart TB
  USR["Operador, repartidor, cliente y soporte"] --> UI
  subgraph SYS["LogiCentral"]
    UI["Interfaz de usuario<br/>(web y móvil, tecnología por definir)"]
    APP["Aplicación modular (monolito modular)<br/>Pedidos · Inventario · Entregas y rutas · Incidencias · Integraciones"]
    DB[("Base de datos<br/>(tecnología por definir)")]
  end
  UI --> APP
  APP --> DB
  APP --> PAG["Sistema de pagos"]
  APP --> GEO["Geolocalización"]
  APP --> NOT["Notificaciones"]
  APP --> ERP["ERP / CRM"]
```

Se mantiene un solo despliegue (ADR-001). Las tecnologías se eligen en las siguientes actividades.

## Nivel 3: componentes del módulo Entregas y rutas

```mermaid
flowchart TB
  subgraph ENT["Módulo Entregas y rutas"]
    API["Controlador / API<br/>(adaptador de entrada)"]
    UC["AssignCourierUseCase<br/>(Application)"]
    DOM["Delivery · DeliveryAddress · DeliveryWindow · CourierAvailability<br/>(Domain)"]
    P1["IDeliveryRepository"]
    P2["ICourierAvailabilityPort"]
    P3["IDeliveryEventPublisher"]
    A1["Adaptador de base de datos"]
    A2["Adaptador de geolocalización"]
    A3["Adaptador de notificaciones"]
  end
  API --> UC
  UC --> DOM
  UC --> P1
  UC --> P2
  UC --> P3
  A1 -. implementa .-> P1
  A2 -. implementa .-> P2
  A3 -. implementa .-> P3
  A1 --> DB[("Base de datos")]
  A2 --> GEO["Servicio de geolocalización"]
  A3 --> NOT["Sistema de notificaciones"]
```

Además de estos puertos, el caso de uso usa un puerto de reloj (`IClock`) para poder probar el tiempo sin depender del reloj real.

## Cómo leer los diagramas

- **Nivel 1:** quién usa el sistema y con qué sistemas externos se conecta.
- **Nivel 2:** de qué piezas desplegables se compone el sistema.
- **Nivel 3:** cómo está organizado por dentro el módulo central del flujo, con las dependencias dirigidas hacia el dominio.

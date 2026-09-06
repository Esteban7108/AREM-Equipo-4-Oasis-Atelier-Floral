# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
Taller 3 - Arquitectura Actual del Sistema con el Modelo C4

## 👥 Integrantes del equipo
- Esteban Díaz Vargas
- Katherin Juliana Moreno Carvajal

## 🧠 Descripción general del trabajo
El objetivo de esta Parte 2 fue representar, mediante las vistas C1 (Contexto) y C2 (Contenedores) del modelo C4, la arquitectura del sistema real del cliente: **Oasis Atelier Floral**, una floristería personalizada de Sopó (Cundinamarca) gestionada por dos personas (el propietario y su mamá, encargada de atención).

A diferencia del caso base RedExpress, Oasis no cuenta actualmente con ningún sistema de información propio: toda la gestión de clientes y pedidos se realiza de forma manual mediante Instagram y WhatsApp. Por esta razón, el C1 y el C2 aquí documentados representan la arquitectura del **sistema objetivo (TO-BE)** que el equipo diseñó en los talleres previos de BPMN y modelo de información, y que se formalizó tecnológicamente en la sección de Arquitectura Tecnológica del informe de BPMN (contenedor web, API REST y base de datos relacional).

## 🔧 Proceso de desarrollo

**C1 - Vista de Contexto:** se identificaron los tres actores humanos que interactúan directamente con el sistema — **Cliente** (15-25 años, solicita arreglos y consulta el estado de su pedido), **Propietario** (gestiona catálogo, precios y elaboración) y **Encargada de Atención** ("Mamá", revisa el panel de solicitudes y el contacto con clientes) —, tomados directamente de la ficha de caracterización y del BPMN del proceso. Se definió **Sistema Oasis** como el sistema en alcance y se identificó un único sistema externo con integración digital real: **WhatsApp Business API**, prevista en la Fase 3 del plan de migración para notificaciones automáticas de cambio de estado.

Se decidió **no incluir** a Instagram, las pasarelas de pago (Nequi/Daviplata) ni a los proveedores de flores como sistemas externos del C1, porque el plan de cambio del cliente es explícito en que "Instagram y WhatsApp continuarán siendo los canales de comunicación con los clientes" de forma independiente al sistema, y ninguno de esos tres tiene hoy una integración de datos prevista con el Sistema Oasis.

**C2 - Vista de Contenedores:** se descompuso el Sistema Oasis en los tres elementos ya definidos en la sección de Arquitectura Tecnológica del Taller de BPMN: una **App Web Oasis** (aplicación web ligera, acceso desde el celular vía navegador, sin instalación), una **API REST Oasis** (gestiona solicitudes, cotizaciones, pedidos, estados y notificaciones, y actúa como punto central de acceso a los datos) y una **Base de Datos Oasis** (SQL relacional, con las ocho entidades ya modeladas en el Taller de Modelo de Información: Cliente, Solicitud, Cotización, Pedido, Arreglo, Materia Prima, Proveedor y Entrega). Se conectaron los tres actores del C1 con la App Web (HTTPS), la App Web con la API (REST/JSON), la API con la base de datos (SQL) y la API con WhatsApp Business API (REST, para el envío de notificaciones).

## 🧩 Análisis del modelo propuesto

### Cómo se estructura el modelo
El modelo es deliberadamente minimalista: un único sistema en alcance con un solo sistema externo en el C1, y tres elementos (dos contenedores más la base de datos) en el C2, sin balanceador de carga ni separación geográfica.

### Cómo representa las necesidades del cliente
Esta simplicidad no es una limitación del ejercicio sino una decisión de modelado que refleja directamente la restricción declarada por el cliente en la ficha de caracterización: *"se busca evitar infraestructura costosa o soluciones demasiado complejas"* y *"se priorizarían tecnologías gratuitas o de bajo costo"*. Un negocio de dos personas que atiende entre 15 y 20 pedidos mensuales no requiere balanceo de carga, múltiples instancias ni separación por zona — al contrario de RedExpress, que sí necesita esa complejidad por operar a escala nacional con picos de demanda en campañas.

### Diferencias explícitas con el caso base (RedExpress)

| Aspecto | RedExpress (caso base) | Oasis (cliente real) |
|---|---|---|
| Punto de partida | Sistema ya en producción, con arquitectura existente que se documenta | No existe sistema propio; el C1/C2 documenta la arquitectura objetivo (TO-BE) aún no construida |
| Contenedores en C2 | 6 contenedores especializados (App Móvil, Portal Web, Gestión de Paquetes, Motor de Rutas, GPS, Alertas) | 3 elementos (App Web, API REST, Base de Datos) — un solo backend monolítico |
| Infraestructura de soporte | Balanceador de carga + base de datos distribuida por región | Sin balanceador; una única base de datos relacional, sin necesidad de distribución geográfica |
| Sistemas externos (C1) | 2 integraciones críticas en tiempo real (Notificaciones, Geolocalización) | 1 integración externa (WhatsApp Business API), y prevista para una fase posterior del plan de migración, no desde el lanzamiento |
| Escala y usuarios | Operación nacional, múltiples regiones, alta concurrencia | 2 usuarios internos del sistema, ~15-20 clientes atendidos al mes |

### Supuestos tomados
- Se asumió que el "Sistema Oasis" corresponde a la arquitectura objetivo definida en los talleres de BPMN (TO-BE) y Modelo de Información, ya que el cliente actualmente no opera ningún sistema propio.
- Se asumió que Instagram, las pasarelas de pago y los proveedores de flores permanecen fuera del sistema por decisión explícita del plan de cambio del cliente, y por tanto no se modelan como sistemas externos del C1.
- Se asumió que la integración con WhatsApp Business API corresponde a la Fase 3 del plan de migración (0 a 6+ meses), por lo que se representa en el C1/C2 como la arquitectura de destino, no como parte del MVP inicial (Fase 1, sin desarrollo).
- No se incluyó el proveedor de hosting cloud (Railway/Render/Supabase) como contenedor del C2, ya que corresponde a una decisión de despliegue/infraestructura que se documentará en el Taller 4.

## 📈 Diagrama final entregado
- `c1-contexto-final.drawio` — Vista de Contexto del Sistema Oasis.
- `c2-contenedores-final.drawio` — Vista de Contenedores del Sistema Oasis.

## 📋 Tabla de actores, entidades o componentes

| Nombre del elemento | Tipo | Descripción | Responsable |
|---|---|---|---|
| Cliente | Actor | Solicita arreglos florales personalizados y consulta el estado de su pedido | Cliente |
| Propietario | Actor | Elabora los arreglos, gestiona el catálogo, precios y compras de materia prima | Equipo Oasis |
| Encargada de Atención | Actor | Revisa el panel de solicitudes pendientes y gestiona el contacto con los clientes | Equipo Oasis |
| Sistema Oasis | Sistema en alcance | Plataforma de gestión de clientes y pedidos a construir para el negocio | Equipo Oasis |
| WhatsApp Business API | Sistema externo | Envío automático de notificaciones de cambio de estado del pedido (Fase 3) | Proveedor externo (Meta) |
| App Web Oasis | Contenedor | Aplicación web ligera de acceso vía navegador, sin instalación | Equipo Oasis |
| API REST Oasis | Contenedor | Gestiona solicitudes, cotizaciones, pedidos, estados y notificaciones | Equipo Oasis |
| Base de Datos Oasis | Infraestructura | Almacenamiento SQL relacional de las 8 entidades del negocio | Equipo Oasis |

## 🔍 Investigación complementaria

### Tema investigado:
Aplicación del modelo C4 en arquitecturas de comercio minorista y e-commerce a pequeña escala.

### Resumen:
La documentación técnica sobre C4 en comercio electrónico coincide en que el nivel de detalle del modelo debe escalar con la complejidad real del sistema, no con lo que "podría" construirse: un caso guiado de una tienda de sombreros a pequeña escala muestra que, incluso en un contexto de e-commerce, un contexto (C1) con dos sistemas y un contenedor (C2) monolítico simple es una representación completa y honesta cuando el negocio no requiere más [2]. Esto respalda la decisión de modelar Oasis con solo tres elementos en el C2, en lugar de forzar una descomposición en microservicios que no se justifica para su escala actual.

Por otro lado, comparaciones de arquitecturas de e-commerce documentadas con C4 muestran que la decisión entre un backend monolítico (una sola API que centraliza toda la lógica) y una arquitectura de microservicios (un contenedor separado por dominio: inventario, pedidos, pagos) depende directamente del volumen de operaciones y del tamaño del equipo que debe mantener el sistema [1][3]. Para un equipo de una sola persona desarrollando el sistema y una operación de menos de 20 pedidos mensuales, el patrón monolítico (App Web + API REST + Base de Datos) es la opción documentada como apropiada, reservando la separación en microservicios para cuando el crecimiento del negocio lo amerite — un camino de evolución coherente con el plan de migración por fases ya definido para Oasis.

## 📚 Referencias

1. Miro / Visual Paradigm. *What is C4 Model? Complete Guide for Software Architecture — caso de plataforma de e-commerce*. Disponible en https://miro.com/diagramming/c4-model-for-software-architecture/ . Fecha de consulta: 05/09/2026.
2. Hayes, Matt. *Diagrams with C4 Model — Architectural Kata: Horrendous Hats Inc. (e-commerce a pequeña escala)*. Disponible en https://mattjhayes.com/2020/05/10/diagrams-with-c4-model/ . Fecha de consulta: 05/09/2026.
3. IcePanel. *How to create common architecture diagrams with the C4 model — ejemplo monolito vs. microservicios en e-commerce (Online Boutique)*. Disponible en https://icepanel.io/blog/2025-01-30-how-to-create-common-architecture-diagrams-with-the-c4-model . Fecha de consulta: 05/09/2026.
4. Brown, Simon. *The C4 Model for Visualising Software Architecture*. Disponible en https://c4model.com/ . Fecha de consulta: 05/09/2026.
5. Universidad de La Sabana. *Guía Paso a Paso: Arquitectura Actual del Sistema con el Modelo C4* - Taller 3, curso Arquitectura Empresarial. 2026.

---

_Este documento hace parte de la entrega del Taller 3 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._

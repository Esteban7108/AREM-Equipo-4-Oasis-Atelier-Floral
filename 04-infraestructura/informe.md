# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
Taller 4 - Mapa de Infraestructura y Diagnóstico Técnico

## 👥 Integrantes del equipo
- Esteban Díaz Vargas
- Katherin Juliana Moreno Carvajal

## 🧠 Descripción general del trabajo
El objetivo de esta Parte 2 fue construir el mapa de infraestructura del sistema real del cliente — **Oasis Atelier Floral** — y realizar el diagnóstico técnico priorizado de sus debilidades, cuellos de botella y límites de escalabilidad, siguiendo la misma metodología de 5 pasos usada en clase sobre RedExpress.

El punto de partida es la arquitectura de contenedores definida en el Taller 3 (C2): **App Web Oasis**, **API REST Oasis** y **Base de Datos Oasis**, más la integración externa con **WhatsApp Business API**. El mapa de infraestructura sitúa estos mismos elementos sobre proveedores cloud de bajo costo (Render/Railway para la aplicación, Supabase para los datos), consistentes con la restricción del cliente de evitar infraestructura costosa.

## 🔧 Proceso de desarrollo

1. **Identificación de componentes**: se retomaron los tres contenedores del C2 (App Web, API REST, Base de Datos) y se añadió la integración con WhatsApp Business API como componente de infraestructura externa.
2. **Agrupación por zona/capa**: al ser un sistema monolítico sin necesidad de separación geográfica, se agrupó por **capa y proveedor cloud** en lugar de por región: Clientes/Dispositivos, Aplicación (Proveedor Cloud A), Datos (Proveedor Cloud B) e Integraciones Externas.
3. **Conexión de componentes**: se trazó el tráfico real — los tres actores acceden a la App Web (HTTPS), la App Web consume la API REST (REST/JSON), la API REST consulta la Base de Datos (SQL) y envía notificaciones a WhatsApp Business API (REST).
4. **Marcado de redundancia y capacidad**: se identificó que **App Web, API REST y Base de Datos operan como instancia única**, sobre planes gratuitos o de entrada (starter) de los proveedores cloud propuestos, sin redundancia ni balanceo de carga.
5. **Diagnóstico y priorización**: se documentaron cuatro riesgos en la tabla de la sección siguiente, cubriendo las tres categorías de la guía (disponibilidad, rendimiento, escalabilidad).

## 🧩 Análisis del modelo propuesto

### Cómo se estructura el modelo
El mapa agrupa los componentes en cuatro zonas por capa/proveedor, no por región geográfica: Clientes, Aplicación (Proveedor Cloud A), Datos (Proveedor Cloud B) e Integraciones Externas. Esta es la decisión de modelado más importante y la principal diferencia estructural frente al caso base.

### Cómo representa las necesidades del cliente
La investigación complementaria confirma que los planes gratuitos de los proveedores cloud de bajo costo considerados (Railway, Render, Supabase) tienen limitaciones concretas que encajan exactamente con el perfil de riesgo de un negocio de dos personas: sin presupuesto para redundancia, pero también sin el volumen de tráfico que la justificaría a corto plazo.

### Diferencias explícitas con el caso base (RedExpress)

| Aspecto | RedExpress (caso base) | Oasis (cliente real) |
|---|---|---|
| Agrupación (Paso 2) | Por zona geográfica (Bogotá, Medellín) | Por capa/proveedor cloud (Aplicación, Datos), sin necesidad geográfica |
| Componente de riesgo principal | Balanceador de carga e infraestructura compartida entre regiones | Ausencia total de redundancia: cada componente (App Web, API, BD) es una instancia única sobre un plan gratuito |
| Naturaleza del riesgo de disponibilidad | Un componente compartido cae y afecta a todas las regiones | El proveedor cloud gratuito puede pausar o detener el servicio por inactividad o agotamiento de créditos, no solo por una falla técnica |
| Riesgo de datos | No se diagnostica explícitamente pérdida de datos | Riesgo diagnosticado de pérdida de datos por ausencia de backups automáticos en el plan gratuito de base de datos |
| Escalabilidad | Limitación por falta de infraestructura propia en una región | Limitación por los topes de cómputo/conexiones de los planes gratuitos, relevante solo en fechas pico (San Valentín, Día de la Madre) |

### Supuestos tomados
- Se asumió una arquitectura de despliegue en dos proveedores cloud distintos: uno para la aplicación (App Web + API REST, ej. Render o Railway) y otro para los datos (Base de Datos, ej. Supabase), por ser la combinación más común documentada para este tipo de arquitectura de bajo costo.
- Se asumió que, dado el volumen actual (15-20 clientes/mes), la arquitectura seguirá sobre planes gratuitos o de entrada durante la Fase 1-2 del plan de migración, lo que hace válido diagnosticar los riesgos de esos planes específicamente (y no de un plan empresarial).
- Se asumió que la integración con WhatsApp Business API (Fase 3) usará una cuenta básica sujeta a límites de mensajería, consistente con el plan de migración por fases ya definido para el cliente.

## 📈 Diagrama final entregado
- `mapa-final.drawio` — Mapa de infraestructura del Sistema Oasis, con el diagnóstico priorizado ya incorporado (componentes marcados en rojo).

## 📊 Diagnóstico priorizado

| Componente | Riesgo diagnosticado | Categoría | Impacto si ocurre | Prioridad |
|---|---|---|---|---|
| App Web Oasis + API REST Oasis (instancia única, plan gratuito/starter) | Punto único de falla: el proveedor cloud puede pausar o detener el servicio por inactividad o agotamiento de créditos gratuitos | Disponibilidad | El sistema completo queda inaccesible para el propietario y la encargada de atención, forzando el regreso al manejo manual por WhatsApp | Alta |
| Base de Datos Oasis (instancia única, sin backup automático en plan gratuito) | Pérdida de datos ante fallo, eliminación accidental o expiración del proyecto gratuito | Disponibilidad | Se perdería el historial completo de clientes, solicitudes, cotizaciones y pedidos, sin posibilidad de recuperación | Alta |
| Infraestructura de bajo costo (límites de cómputo y conexiones de los planes gratuitos/starter) | Límite de escalabilidad ante picos de demanda | Escalabilidad | Lentitud o caídas del sistema en fechas de alta demanda (San Valentín, Día de la Madre) | Media |
| Integración con WhatsApp Business API (cuenta básica, límites de mensajería) | Cuello de botella en el envío de notificaciones automáticas | Rendimiento | Los clientes no reciben la notificación automática de cambio de estado; se mitiga con el contacto manual que ya existe como respaldo | Baja |

## 📋 Tabla de actores, entidades o componentes

| Nombre del elemento | Tipo | Descripción | Responsable |
|---|---|---|---|
| Cliente / Propietario / Encargada de Atención | Cliente (dispositivo) | Acceden al sistema desde el navegador móvil | Actores del sistema |
| App Web Oasis | Servicio | Aplicación web ligera, instancia única sobre proveedor cloud de bajo costo | Infraestructura - Aplicación |
| API REST Oasis | Servicio | Gestiona solicitudes, cotizaciones, pedidos y notificaciones; instancia única | Infraestructura - Aplicación |
| Base de Datos Oasis | Base de datos | Almacenamiento SQL relacional; instancia única sin backup automático | Infraestructura - Datos |
| WhatsApp Business API | Servicio externo | Envío de notificaciones automáticas de estado del pedido (Fase 3) | Integración externa |

## 🔍 Investigación complementaria

### Tema investigado:
Buenas prácticas y limitaciones reales de infraestructura cloud de bajo costo (planes gratuitos/starter) para sistemas de pequeña escala, aplicadas al diagnóstico de Oasis.

### Resumen:
La documentación técnica actualizada de los proveedores considerados confirma que el riesgo de disponibilidad diagnosticado no es hipotético: el plan gratuito de Supabase **no incluye backups automáticos ni SLA**, y pausa automáticamente los proyectos tras una semana sin actividad, lo que puede dejar el sistema fuera de línea sin que medie ninguna falla técnica real [1][3]. De forma similar, el plan gratuito de Railway funciona por créditos: cuando se agotan, el servicio se detiene sin escalado automático a un plan pago, lo que puede provocar caídas abruptas si no se monitorea activamente el consumo [2].

Estos hallazgos respaldan directamente la prioridad "Alta" asignada tanto al riesgo de disponibilidad de la capa de aplicación como al riesgo de pérdida de datos de la base de datos: en ambos casos, la causa del riesgo no es una falla de hardware sino una condición comercial del plan gratuito (inactividad, créditos agotados, ausencia de backups) que un negocio de dos personas puede fácilmente pasar por alto. La recomendación estándar de la industria para mitigar el riesgo de datos en estos planes —automatizar un respaldo periódico hacia un almacenamiento externo de bajo costo— es consistente con lo que el propio informe de BPMN de Oasis ya identificaba como necesidad de gobernanza ("se deben realizar copias de seguridad periódicas… para evitar depender de un único dispositivo"), y confirma que ese punto de gobernanza debe traducirse en una acción técnica concreta antes de pasar a producción.

## 📚 Referencias

Las referencias utilizadas y la información de investigación complementaria se encuentran registradas en `referencias.md`.

---

_Este documento hace parte de la entrega del Taller 4 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._

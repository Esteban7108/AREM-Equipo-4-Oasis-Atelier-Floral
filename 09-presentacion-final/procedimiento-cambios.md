# Procedimiento de Actualización y Revisión de la Arquitectura

## Cliente
Oasis Atelier Floral — Sopó, Cundinamarca.

## Integrantes del equipo
- Esteban Díaz Vargas
- Katherin Juliana Moreno Carvajal

## Propósito de este documento

A diferencia del Plan de Implementación (que ejecuta un cambio ya decidido) y del Plan de Gobernanza (que supervisa esa ejecución), este documento cubre qué pasa **después**: cómo se mantiene actualizada la arquitectura de Oasis cuando surge una necesidad de cambio nueva y cómo queda registrada cada decisión.

## Disparadores de cambio

| Disparador | Ejemplo concreto para este cliente |
|---|---|
| Nueva regulación | Cambio en la normativa de protección de datos (Ley 1581 de 2012) o de comercio electrónico (Ley 1480 de 2011) que exija nuevos controles, o una instrucción de la Superintendencia de Industria y Comercio |
| Cambio de proveedor tecnológico | Supabase, Render o Railway modifican precios, límites o backups de sus planes; Meta cambia costos, límites o aprobación de WhatsApp Business API |
| Incidente de seguridad | Un cliente reporta que alguien vio o cambió su pedido; se pierde información; el sistema queda caído más de 24 horas |
| Crecimiento inesperado de demanda | Más de 50 solicitudes al mes (umbral de reevaluación de ADR-001 y ADR-002); saturación en San Valentín o Día de la Madre; apertura de un punto físico (evaluada en la Fase 4 del cliente) |
| Cambio en el equipo | Se suma una tercera persona (ayuda temporal en temporada alta): activa el RBAC antes de darle acceso |

## Proceso de cambio

1. **Quién propone:** cualquier integrante del comité (Propietario, Encargada de Atención o Responsable técnico) registra la necesidad.
2. **Registro:** el Responsable técnico abre una ADR en estado *Propuesta* con fecha, contexto, problema y alternativas.
3. **Evaluación (3 días hábiles):** el Responsable técnico mide el cambio contra los objetivos de la Ficha (Taller 0), los cuatro principios del plan de gobernanza, la matriz de riesgos y el costo mensual; la Encargada indica el efecto en la operación.
4. **Dictamen (máximo 5 días hábiles desde la propuesta):** el Propietario acepta, rechaza o pide ajustes y se anota la fecha de dictamen (alimenta la métrica M3).
5. **Actualización:** si se acepta, se actualizan la matriz de riesgos, el tablero integrado y, si aplica, el plan de implementación.
6. **Respuesta esperada:** 5 días hábiles; en incidentes de seguridad, el mismo día se contiene y el registro se completa después.

## Registro de decisiones (ADR)

Las tres decisiones ya fueron tomadas por el equipo durante el curso; su estado es *Propuesta* hasta que el Propietario las dictamine en la sustentación del 14/11/2026. Las ADR no se editan para cambiarles el sentido ni se borran: se reemplazan por otra ADR.

| Campo | ADR-001 |
|---|---|
| Título | Base de datos en Supabase Pro en lugar del plan gratuito con script propio de backup |
| Estado | Propuesta (dictamen del Propietario pendiente) |
| Fecha de propuesta | 27/09/2026 (matriz de decisión del Taller 7) |
| Fecha de dictamen | Pendiente: sustentación del 14/11/2026 |
| Contexto | El sistema objetivo del Taller 3 usa una base de datos SQL relacional. Los Talleres 4, 5 y 6 diagnosticaron, cada uno por su lado, el mismo riesgo Alto: el plan gratuito no incluye backups automáticos, pausa el proyecto tras una semana de inactividad y no hay cifrado en reposo confirmado. |
| Problema | Si la base se corrompe, se borra por error o el proveedor la pausa, se pierde el historial de clientes, solicitudes y pedidos sin recuperación, y los datos personales quedan sin cifrado garantizado, lo que expone al negocio frente a la Ley 1581. |
| Decisión | Aprovisionar la base de datos directamente en Supabase Pro (≈USD 25/mes) al desplegar el sistema propio (Corto plazo del plan). |
| Alternativas consideradas | **B · Script propio de backup semanal hacia Backblaze B2 (≈USD 1–2/mes):** descartada, obtuvo Confiabilidad 2 (menor que el mínimo de 3): backup solo semanal, sin cifrado adicional garantizado y dependiente de que alguien mantenga el script, algo que el equipo no puede hacer. **C · Aceptar el riesgo en el plan gratuito:** descartada, Confiabilidad 1; aunque su total ponderado bruto era el más alto (3,80 frente a 3,70), mantiene intacto el riesgo Alto. La Opción A ganó en los tres escenarios de pesos probados. |
| Consecuencias | (+) Backup diario con retención de 7 días y cifrado en reposo sin mantenimiento técnico; el proyecto de base de datos no se pausa por inactividad. (−) Costo fijo de ≈USD 25/mes para un negocio de 15 a 20 pedidos mensuales; dependencia de un proveedor (PostgreSQL estándar, exportable); la retención de 7 días limita la recuperación a esa ventana. Pendiente: que el Propietario valide los pesos (40/30/20/10) y que Supabase confirme por escrito el cifrado en reposo y el precio vigente. **Trade-off aceptado:** se paga un costo mensual a cambio de confiabilidad sin operación técnica. Reevaluar si hay más de 50 solicitudes al mes o si cambian las condiciones del plan. |

| Campo | ADR-002 |
|---|---|
| Título | Backend monolítico de tres elementos (App Web + API REST + Base de Datos) en lugar de microservicios |
| Estado | Propuesta (dictamen del Propietario pendiente) |
| Fecha de propuesta | Septiembre de 2026 (Taller 3, vista C2) |
| Fecha de dictamen | Pendiente: sustentación del 14/11/2026 |
| Contexto | Oasis es un negocio de 2 personas que atiende entre 15 y 20 clientes al mes, con la restricción explícita de evitar infraestructura costosa o soluciones demasiado complejas. |
| Problema | Definir en cuántos contenedores se descompone el sistema, sabiendo que lo desarrollará y mantendrá un equipo técnico mínimo y que cada contenedor adicional suma costo de hosting y de operación. |
| Decisión | Un único backend: App Web Oasis, API REST Oasis y Base de Datos Oasis, más una sola integración externa (WhatsApp Business API). |
| Alternativas consideradas | **Microservicios por dominio (pedidos, inventario, notificaciones):** descartada, el volumen no la justifica y multiplica costo y operación. **Balanceador y API gateway como en RedExpress:** descartada, resuelven una escala nacional que Oasis no tiene. |
| Consecuencias | (+) Menor costo, despliegue simple, fácil de explicar y de mantener. (−) Un solo componente concentra todo (ver ADR-003) y, si el negocio crece (inventario, punto físico), habrá que separar módulos: deuda técnica aceptada. **Trade-off aceptado:** se sacrifica aislamiento y escalabilidad por simplicidad y costo. Reevaluar con más de 50 solicitudes al mes o nuevas capacidades de negocio. |

| Campo | ADR-003 |
|---|---|
| Título | Mantener una instancia única de App Web + API REST, sin redundancia, y diferir las mejoras de disponibilidad |
| Estado | Propuesta (dictamen del Propietario pendiente) |
| Fecha de propuesta | 27/09/2026 (Taller 7, secciones 2.2 y 3.2) |
| Fecha de dictamen | Pendiente: sustentación del 14/11/2026 |
| Contexto | El Taller 4 diagnosticó como riesgo Alto que la capa de aplicación corre en una sola instancia de un plan gratuito o starter, que puede detenerse por inactividad o agotamiento de créditos. El presupuesto del cliente es limitado. |
| Problema | Con recursos para una sola intervención grande, ¿se invierte primero en redundancia de la aplicación o en cerrar los riesgos que comprometen datos personales (backup, cifrado, identidad, consentimiento)? |
| Decisión | No implementar redundancia. En Largo plazo: plan pago estable para la capa de aplicación, monitoreo de disponibilidad y alerta de consumo de créditos. Mientras tanto, el registro y el contacto manual por WhatsApp son el respaldo operativo. |
| Alternativas consideradas | **Segunda instancia con balanceador activo-pasivo:** descartada por costo recurrente y complejidad para un equipo sin perfil técnico, frente a 15–20 pedidos al mes y a un respaldo manual que ya existe. **Plan alto sin redundancia:** descartada, costo desproporcionado. **No hacer nada:** descartada, la pausa por inactividad o créditos es una condición comercial del plan y pasa inadvertida sin monitoreo. |
| Consecuencias | (+) Bajo costo y foco en los riesgos de datos personales. (−) Si la instancia cae, el sistema queda inaccesible y el negocio vuelve temporalmente a modo manual; el riesgo R6 queda abierto hasta Largo plazo. **Trade-off aceptado:** se sacrifica disponibilidad por costo y simplicidad. Reevaluar tras una caída de 24 horas o más, con más de 50 solicitudes al mes o si se pierde un pedido en un pico. |

## Revisión periódica completa

Cada 3 meses mientras dure el roadmap, alineada con el cierre de cada fase: se revisan el tablero integrado, la matriz de riesgos y las métricas M1 a M3 contra la situación real del negocio. Después, cada 6 meses. Además, una revisión previa a cada pico de demanda (San Valentín y Día de la Madre) para confirmar el congelamiento de cambios.

---

_Este documento hace parte de la entrega final del Taller 9 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._

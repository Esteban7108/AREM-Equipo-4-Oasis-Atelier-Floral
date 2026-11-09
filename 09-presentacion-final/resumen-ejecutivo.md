# Resumen Ejecutivo del Proyecto Arquitectónico

## Nombre del Cliente
Oasis Atelier Floral — floristería personalizada de Sopó, Cundinamarca (Equipo 4: Esteban Díaz Vargas y Katherin Juliana Moreno Carvajal).

## Objetivo General del Proyecto
Oasis atiende entre 15 y 20 clientes al mes con dos personas, y toda su gestión de pedidos ocurre en chats de Instagram y WhatsApp. El proyecto diseñó, con la metodología de arquitectura empresarial del curso, un sistema sencillo y de bajo costo que centraliza clientes, solicitudes, cotizaciones y estados de pedido, y que cierra de forma ordenada los riesgos de datos personales, seguridad y disponibilidad que los talleres fueron encontrando, respetando que el cliente siga atendiendo por WhatsApp e Instagram.

## Vistas Arquitectónicas Cubiertas

| Vista | Alcance de la Solución |
|---|---|
| Procesos de Negocio | BPMN del ciclo de gestión de clientes y pedidos (solicitud, cotización, confirmación, elaboración y entrega) y plan de migración en 4 fases del cliente |
| Información / Datos | Modelo entidad-relación y relacional de 8 entidades (Cliente, Solicitud, Cotización, Pedido, Arreglo, Materia Prima, Proveedor, Entrega) |
| Aplicaciones / Sistemas | C4 C1 y C2: App Web Oasis, API REST Oasis, Base de Datos Oasis y WhatsApp Business API |
| Infraestructura | Mapa sobre Render/Railway y Supabase, con 4 riesgos diagnosticados (instancia única y ausencia de backup en prioridad Alta) |
| Seguridad | STRIDE con 6 amenazas; la exposición de datos personales (T4) es la de mayor nivel |
| Cumplimiento Normativo | Checklist de 13 ítems (Ley 1581, ISO/IEC 27001, Ley 1480): 1 en «Cumple» y 12 en «Parcial» |
| Mejora de arquitectura (TO-BE) | Matriz de brechas, matriz de decisión ponderada para backup y cifrado, y paquetes de trabajo |
| Integración de vistas | Tablero de 5 capas con el AS-IS y el TO-BE marcados |

## Hallazgos Clave

- El riesgo crítico del negocio es la gestión manual de pedidos en chats (Nivel 9 de 12): sin registro central, el estado de cada pedido se reconstruye de memoria.
- Un mismo hueco apareció de forma independiente en tres talleres: la base de datos no tiene backup automático ni cifrado en reposo confirmado (Talleres 4, 5 y 6), con riesgo de perder el historial de clientes y de incumplir la Ley 1581.
- Solo 1 de 13 ítems del checklist normativo cumple hoy; faltan autorización informada de tratamiento de datos y un canal para derechos ARCO.
- La matriz de riesgos consolidada tiene 14 riesgos que cubren los 7 dominios; el plan se ordena por riesgo, esfuerzo y dependencias, no solo por prioridad.

## Recomendaciones Principales

- Empezar de inmediato con un registro digital básico y el aviso de privacidad, que son de esfuerzo bajo y atacan el riesgo crítico y el incumplimiento normativo.
- Desplegar el sistema propio directamente sobre Supabase Pro (≈USD 25/mes, ADR-001), con cifrado en reposo, backup diario y HTTPS.
- Incorporar la verificación de identidad por código de WhatsApp y las notificaciones automáticas, y dejar para el final la redundancia de la aplicación, que se cubre con monitoreo y respaldo manual (ADR-003).
- Gobernar con un comité de 3 roles que se reúne 30 minutos al mes, 4 principios, 3 métricas y un registro de decisiones (ADR).

## Plan de Implementación (resumen)

| Fase | Qué se implementa | Duración estimada |
|---|---|---|
| Quick win | Registro digital básico, aviso de privacidad y canal ARCO | 4 semanas (11 ene – 8 feb 2027) |
| Corto plazo | Sistema propio sobre Supabase Pro, HTTPS y casilla de autorización | 8 semanas (8 feb – 5 abr 2027) |
| Mediano plazo | WhatsApp Business API, Autenticación Ligera y rate limiting | 9 semanas (5 abr – 7 jun 2027) |
| Largo plazo | Auditoría, roles, monitoreo de disponibilidad y catálogo en línea | 6 meses (7 jun – 4 dic 2027) |

_Ver el detalle completo, con la relación de cada fase a los riesgos identificados en el curso, en [`plan-implementacion.md`](plan-implementacion.md)._


## Reflexión Final

El ejercicio mostró que, para un negocio pequeño, la arquitectura útil no es la más completa sino la que se puede pagar, operar y ejecutar en el orden correcto. Integrar los talleres en una sola cadena —riesgo, brecha, decisión y fase— permitió ver que lo que más pesa es poco y se repite: proteger los datos de los clientes y ordenar los pedidos.

---

_Este resumen ejecutivo forma parte de la entrega final del curso AREM - Arquitectura Empresarial - Universidad de La Sabana._

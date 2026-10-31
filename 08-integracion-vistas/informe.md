# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
Taller 8 - Integración de Vistas de Arquitectura

## 👥 Integrantes del equipo
- Esteban Díaz Vargas
- Katherin Juliana Moreno Carvajal

## 🧠 Descripción general del trabajo
El objetivo de esta Parte 2 fue integrar, en un solo tablero coherente, todas las vistas arquitectónicas ya construidas para **Oasis Atelier Floral** a lo largo del curso: negocio (procesos del ciclo de pedido), información (modelo de datos del Taller 3), aplicaciones e infraestructura AS-IS (Talleres 3 y 4), seguridad (STRIDE del Taller 5 y normatividad del Taller 6), y la arquitectura objetivo TO-BE propuesta en el Taller 7 (Opportunities & Solutions), marcando explícitamente los elementos nuevos o mejorados con las etiquetas **NUEVO** y **MEJORADO**.

## 🔧 Proceso de desarrollo
Se siguió la metodología de 5 pasos de la guía del taller:

1. **Organización en capas:** se definieron las 5 capas (negocio, aplicaciones, información, infraestructura, seguridad) como contenedores horizontales del tablero.
2. **Ubicación de elementos:** se transcribió el inventario completo ya definido en los Talleres 3-7, sin inventar elementos nuevos — cada caja del tablero corresponde a un elemento ya documentado en un entregable previo.
3. **Trazado de relaciones verticales:** a diferencia del caso FarmApp (que tiene tres procesos de negocio independientes: Compra Online, Prescripción, Despacho, y traza solo uno para no saturar el tablero), Oasis es un negocio de 2 personas con un único proceso de negocio real — el ciclo completo de un pedido (Solicitud → Cotización/Confirmación → Elaboración → Entrega) — por lo que se trazó ese ciclo de punta a punta, atravesando las 5 capas.
4. **Narrativa de coherencia:** se documentó el *por qué* de cada conexión, no solo su existencia (ver sección de análisis).
5. **Revisión de consistencia:** se verificó que cada elemento relevante tuviera al menos una conexión hacia arriba y hacia abajo, y que los 8 elementos TO-BE del Taller 7 (3 en Aplicaciones/Infraestructura, 3 en Seguridad) estuvieran presentes y marcados.

Herramienta usada: draw.io, manteniendo la paleta de colores ya establecida desde el Taller 5 (naranja `#FAC775` para riesgo/pendiente, verde `#C6E0B4` para mejorado/mitigado) para que el tablero se lea de forma consistente con los entregables anteriores.

## 🧩 Análisis del modelo propuesto

### Cómo se estructura el modelo
El tablero tiene 5 capas horizontales con un único hilo de negocio conectado de punta a punta, en lugar de 5 diagramas sueltos. Las conexiones siguen tres patrones: negocio→aplicaciones (quién soporta qué proceso), aplicaciones→información/infraestructura (dónde vive y dónde corre cada dato), y aplicaciones/infraestructura→seguridad (qué control protege o mitiga cada punto, con flechas punteadas etiquetadas "protege"/"implementa", replicando la convención de C1/C2 del Taller 3 para relaciones no estructurales).

### Cómo representa las necesidades del cliente
La integración confirma algo que ya era visible taller por taller pero que solo se hace evidente al verlo conectado: **los tres gaps TO-BE del Taller 7 (autenticación, consentimiento, cifrado/backup) no son mejoras aisladas — las tres cierran huecos de seguridad identificados de forma independiente en los Talleres 4, 5 y 6 sobre el mismo hilo de negocio** (la confirmación y el pago/entrega de un pedido). Esto respalda la decisión, tomada en el Taller 7, de priorizar esas tres brechas sobre las otras seis ideas del brainstorming: no son parches puntuales, sino los tres puntos donde el hilo de negocio completo queda expuesto.

También se hace visible una asimetría relevante: el componente de infraestructura con mayor riesgo (**App Web + API REST en instancia única**, Taller 4) **no tiene todavía una mejora TO-BE conectada** — el Taller 7 priorizó el gap de base de datos (corroborado en tres talleres) sobre este, que solo aparece en el diagnóstico del Taller 4. El tablero integrado deja ese vacío explícito en vez de ocultarlo, lo cual es información útil para el plan de implementación del Taller 9.

### Supuestos tomados
- Se asumió que el "proceso de negocio" de Oasis, para efectos de este tablero, es el ciclo completo de un pedido (Solicitud → Cotización → Elaboración → Entrega), ya que el negocio no tiene procesos paralelos independientes como FarmApp (Compra Online, Prescripción, Despacho son canales/casos de uso distintos de una cadena nacional; Oasis atiende un único flujo con 2 personas).
- Se asumió que "Materia Prima" y "Proveedor" pertenecen a la capa de Información y no a la de Negocio, siguiendo la clasificación ya fijada en el modelo de datos del Taller 3 (son entidades, no procesos ni actores).
- Se mantuvieron los nombres exactos de los elementos ya definidos en cada taller anterior (p. ej. "API REST Oasis", "STRIDE T1/T4") para que el tablero sea trazable sin ambigüedad hacia su taller de origen, en vez de renombrarlos por legibilidad.
- No se incluyó el proveedor de hosting (Render/Railway/Supabase) como un elemento adicional de la capa de Infraestructura más allá de los ya nombrados en el Taller 4 — solo se anotó entre paréntesis, igual que en los diagramas de ese taller.

## 📈 Diagrama final entregado
- `entrega/tablero-integrado-cliente.drawio` — Tablero de Integración de Vistas del Sistema Oasis (5 capas, AS-IS + TO-BE marcado).

## 📋 Tabla de actores, entidades o componentes

| Nombre del elemento | Capa | Tipo | Taller de origen |
|---|---|---|---|
| Solicitud de Arreglo / Cotización y Confirmación / Elaboración / Entrega | Negocio | Proceso | Taller 0/1 (BPMN) |
| App Web Oasis (MEJORADO) | Aplicaciones | Contenedor | Taller 3 / Taller 7 |
| API REST Oasis | Aplicaciones | Contenedor | Taller 3 |
| Módulo de Autenticación Ligera (NUEVO) | Aplicaciones | Contenedor | Taller 7 |
| Cliente, Solicitud, Cotización, Pedido, Arreglo, Materia Prima, Proveedor, Entrega | Información | Entidad | Taller 3 (modelo de datos) |
| App Web + API REST - instancia única | Infraestructura | Riesgo diagnosticado (Alto) | Taller 4 |
| Base de Datos Oasis - Supabase Pro (MEJORADO) | Infraestructura | Infraestructura | Taller 4 / Taller 7 |
| WhatsApp Business API (MEJORADO) | Infraestructura | Sistema externo | Taller 3 / Taller 7 |
| HTTPS en tránsito | Seguridad | Control (Cumple) | Taller 6 |
| Verificación de Identidad (NUEVO) | Seguridad | Mitigación | Taller 5 (STRIDE T1) / Taller 7 |
| Consentimiento Explícito (NUEVO) | Seguridad | Mitigación | Taller 6 / Taller 7 |
| Cifrado en reposo + Backup diario (MEJORADO) | Seguridad | Mitigación | Taller 5 (STRIDE T4) / Taller 7 |

## 🔍 Investigación complementaria

### Tema investigado:
Documentación de arquitecturas integradas multicapa (negocio-aplicación-tecnología) en negocios pequeños, con notación ArchiMate.

### Resumen:
El caso guiado "Coffee Around The Corner" [1] documenta exactamente el mismo problema que aborda este taller a escala de un negocio de 2-3 personas: cómo mantener una sola vista coherente entre negocio, aplicaciones y tecnología sin recurrir a diagramas sueltos. El caso muestra una cafetería que depende de una única solución SaaS de inventario y contabilidad —un paralelo directo con la dependencia de Oasis en un backend monolítico (App Web + API REST en instancia única)— y advierte que ese único punto de integración es también un único punto de falla, el mismo hallazgo que ya tenía Oasis desde el Taller 4 y que el tablero integrado de este taller vuelve a confirmar visualmente.

El caso también respalda la decisión metodológica tomada aquí de usar *viewpoints* (vistas filtradas) en vez de un único diagrama que lo muestre todo: para negocios pequeños recomienda empezar con 2 o 3 vistas específicas (p. ej. una vista de riesgos tecnológicos) en lugar de un modelo completo desde el día uno — coherente con la decisión de la guía del taller de trazar un solo hilo de negocio a la vez en lugar de conectar las 5 capas completas, que saturaría el tablero sin aportar más claridad.

## 📚 Referencias

Las referencias utilizadas y la información de investigación complementaria se encuentran registradas en `referencias.md`.

---

_Este documento hace parte de la entrega del Taller 8 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._

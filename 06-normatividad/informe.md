# Informe Técnico del Taller

## Nombre del Taller
Taller 6 - Checklist de Cumplimiento Normativo

## Integrantes del equipo
- Esteban Díaz Vargas
- Katherin Juliana Moreno Carvajal

## Descripción general del trabajo

El objetivo de esta Parte 2 fue verificar el cumplimiento normativo del sistema real del cliente — **Oasis Atelier Floral** — frente a los mismos marcos evaluados en clase sobre GobData: Habeas Data (Ley 1581 de 2012), ISO/IEC 27001, protección contra fugas de datos, y consentimiento/auditoría/roles de acceso, siguiendo la misma metodología de 5 pasos.

La diferencia de partida es fundamental: GobData es un sistema estatal ya en producción, mientras que Oasis **todavía no tiene ningún sistema desplegado** — el "Sistema Oasis" evaluado aquí es la arquitectura objetivo ya definida en los Talleres 3 y 4. Este checklist, por tanto, no audita un sistema en funcionamiento sino que diagnostica **qué tan preparado está el diseño actual** frente a las obligaciones legales que aplicarán desde el día en que ese sistema entre en operación y empiece a procesar datos reales de clientes.

## Proceso de desarrollo

Se reutilizó la estructura de 6 categorías de GobData (Consentimiento, Seguridad, Protección de Datos, Prevención de Fugas, Retención, Roles y Responsabilidades), pero **no se copió ni un solo ítem**: cada criterio se redactó y evaluó contra lo que efectivamente existe en el diseño de Oasis, documentado en los Talleres 3, 4 y 5.

De los 13 criterios evaluados, solo 1 quedó en **Cumple** (cifrado en tránsito vía HTTPS, ya definido explícitamente en la arquitectura del Taller 3) y **12 en Parcial**. Esto no es un error de evaluación ni una tabla "genérica" — es el reflejo honesto de que Oasis está en fase de diseño, no de operación: casi ningún control de cumplimiento se ha formalizado todavía, porque el sistema que los necesitaría aún no existe.

Los 12 ítems Parcial se documentaron como filas en la hoja **Brechas Identificadas**, con su riesgo y prioridad. Varias de estas brechas **no son hallazgos nuevos**, sino la traducción a lenguaje normativo de riesgos ya diagnosticados en talleres anteriores:
- La brecha de cifrado en reposo y ausencia de backups retoma directamente el riesgo de infraestructura de bajo costo del **Taller 4**.
- La brecha de logs de acceso retoma la amenaza T3 (Repudiation) del **Taller 5**.
- La brecha de roles y permisos no diferenciados retoma la amenaza T6 (Elevation of Privilege) del **Taller 5**.

## Análisis del modelo propuesto

### Cómo se estructura el modelo
El checklist mantiene las 6 categorías de GobData por comparabilidad, pero el peso de la evaluación es inverso: en GobData 7 de 12 ítems están en Cumple; en Oasis, 12 de 13 están en Parcial. La hoja de Brechas Identificadas pasó de 5 filas (GobData) a 12 filas (Oasis).

### Cómo representa las necesidades del cliente
Este resultado es coherente con la realidad de Oasis: es una empresa colombiana de 2 personas que, sin importar su tamaño, queda sujeta a la Ley 1581 de 2012 desde el primer cliente registrado — el mismo hallazgo que ya se estableció en la investigación complementaria del Taller 5 — pero que, a diferencia de GobData, no tiene los recursos ni la urgencia regulatoria de una entidad estatal para haber formalizado controles antes de tener siquiera un sistema construido.

### Diferencias explícitas con el caso base (GobData)

| Aspecto | GobData (caso base) | Oasis (cliente real) |
|---|---|---|
| Estado del sistema evaluado | En producción, con datos reales de ciudadanos | En diseño; el checklist evalúa preparación normativa, no auditoría de un sistema operando |
| Proporción Cumple / Parcial | 7 Cumple / 5 Parcial (de 12) | 1 Cumple / 12 Parcial (de 13) |
| Brechas identificadas | 5 | 12 |
| Brecha de mayor riesgo | Ausencia de plan de continuidad (BCP/DRP) y de controles DLP | Cifrado en reposo no confirmado y ausencia de backups — ambas ya señaladas como riesgo Alto en el Taller 4 |
| Figura de responsable de datos | DPO formal asignado (exigible por el volumen y sensibilidad de datos que maneja un ente estatal) | Sin DPO formal — no exigido para una microempresa, pero tampoco documentado quién asume el rol de facto |
| Marco regulatorio adicional relevante | MinSalud/SuperSalud (por el historial clínico que procesa) | Ley 1480 de 2011 (Estatuto del Consumidor) y registro mercantil ante Cámara de Comercio, por ser un comercio electrónico dirigido a consumidor final |

### Supuestos tomados
- Se asumió que, al no existir un sistema desplegado, "Cumple" solo aplica a decisiones ya documentadas explícitamente en la arquitectura de los Talleres 3-4 (no a intenciones no formalizadas); todo lo demás se marcó Parcial, incluso cuando existe una práctica informal razonable (ej. el Propietario como responsable de facto del tratamiento).
- Se asumió que, por ser una microempresa, no aplica exigir una figura formal de DPO (Ley 1581 no lo exige por tamaño), pero sí exige que exista un responsable identificable del tratamiento — de ahí que el ítem 7 se evalúe distinto a como se evaluaría en una entidad grande.
- Se asumió que Oasis, al vender directamente a consumidores finales por canales digitales (o planear hacerlo), queda cobijada por el Estatuto del Consumidor (Ley 1480 de 2011), no solo por la Ley 1581.

## Tabla de actores, entidades o componentes

| Nombre del elemento | Tipo | Descripción | Responsable |
|---|---|---|---|
| Cliente | Actor | Titular de los datos personales tratados por el sistema | Cliente |
| Propietario | Actor | Asume de facto el rol de responsable del tratamiento de datos | Equipo Oasis |
| Sistema Oasis | Componente de aplicación | Sistema objetivo (App Web + API + Base de Datos) que procesará los datos personales | Equipo Oasis |
| Mecanismo de revocatoria / derechos ARCO | Constraint (ArchiMate) | Restricción derivada de la Ley 1581 sobre el registro de solicitud del cliente | Equipo del taller |

## Investigación complementaria

### Tema investigado:
Normativas sectoriales aplicables a un comercio electrónico de venta directa al consumidor en Colombia, adicionales a la Ley 1581.

### Resumen:
A diferencia de GobData, cuya normativa sectorial relevante gira en torno a la salud (por el historial clínico que procesa), Oasis es un comercio electrónico de venta directa al consumidor, lo que la sujeta al **Estatuto del Consumidor (Ley 1480 de 2011)**. El artículo 50 de esta ley establece responsabilidades específicas para los proveedores de comercio electrónico, y la Superintendencia de Industria y Comercio (SIC) — la misma autoridad que vigila la Ley 1581 — es también quien vigila su cumplimiento, lo que convierte a la SIC en el punto de control regulatorio único para Oasis en ambos frentes.

Además de la protección al consumidor, un comercio electrónico formal en Colombia debe contar con Registro Único Tributario (RUT) y registro mercantil ante la Cámara de Comercio, y dar cumplimiento a las obligaciones de IVA correspondientes — requisitos administrativos que no tienen relación con STRIDE ni con los talleres de arquitectura técnica previos, pero que sí son parte del "cumplimiento normativo" que pide este taller, y que conviene que el equipo tenga presente antes de que Oasis empiece a vender formalmente por un canal digital propio.

## Referencias

Las referencias utilizadas y la información de investigación complementaria se encuentran registradas en `referencias.md`.

---

_Este documento hace parte de la entrega del Taller 6 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._

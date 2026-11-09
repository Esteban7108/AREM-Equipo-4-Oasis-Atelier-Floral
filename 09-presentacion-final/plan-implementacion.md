# Plan de Implementación (Roadmap de Transición)

## Cliente
Oasis Atelier Floral — floristería personalizada de Sopó, Cundinamarca (contacto y representante legal: Juan David Cetina Gómez).

## Integrantes del equipo
- Esteban Díaz Vargas
- Katherin Juliana Moreno Carvajal

## Propósito de este documento

La solución arquitectónica (Acto 3) es un sistema de gestión de clientes y pedidos —App Web + API REST + base de datos— con tres controles ya diseñados en el Taller 7 (verificación de identidad por WhatsApp, autorización de tratamiento de datos, base de datos con cifrado y backup) y la integración con WhatsApp Business API. Un diseño sin ruta de ejecución deja a un negocio de 2 personas, sin perfil técnico y con presupuesto limitado, sin saber qué hacer primero, qué depende de qué y cuándo **no** debe tocar nada (San Valentín y Día de la Madre). Este plan ordena el trabajo en 4 fases, cada una trazada a riesgos de `matriz-riesgos.xlsx`, y se alinea con el plan de migración en 4 fases que el propio cliente definió en el informe de BPMN.

**Supuestos de calendario (a confirmar con el Propietario):** el plan arranca el lunes 11/01/2027. Se ejecuta el desarrollo con un responsable técnico por definir (el equipo AREM entrega el diseño; no mantiene el sistema después del curso).

## Fases de implementación

| Fase | Qué se implementa | Riesgo / brecha que corrige | Esfuerzo | Duración estimada | Responsable sugerido |
|---|---|---|---|---|---|
| **Quick win** (11 ene – 8 feb 2027) | (1) Registro digital básico en Google Sheets o Airtable con los 7 estados del pedido y panel de pendientes (Fase 1 del plan del cliente). (2) Aviso de privacidad y autorización de tratamiento en el primer mensaje a cada cliente, política de datos publicada y canal ARCO por WhatsApp. (3) Propietario documentado como Responsable del Tratamiento. (4) Exportación semanal del registro a un segundo lugar (respaldo provisional). | R1 (gestión manual), R7 (información dispersa), R5 (Ley 1581), reduce R2 · Gap G3 y G4 | Bajo | 4 semanas | Propietario y Encargada de Atención, con apoyo del equipo AREM |
| **Corto plazo** (8 feb – 5 abr 2027) | Sistema propio (Fase 2 del cliente) desplegado desde el primer día sobre **Supabase Pro** (ADR-001): cifrado en reposo y backup diario; HTTPS/HSTS; validación en servidor de que cada pedido pertenece a quien lo consulta; casilla de autorización en el formulario de solicitudes; migración de los datos del registro. | R2 (pérdida de datos), R3 (exposición de datos), R12 (alteración en tránsito); cierra R1, R5 y R7 de forma definitiva · Gap G1, G3, G4, G5 (parte) | Alto | 8 semanas | Responsable técnico (por definir); el Propietario aprueba el costo (≈USD 25/mes) |
| **Mediano plazo** (5 abr – 7 jun 2027) | Integración con WhatsApp Business API (notificación automática de cada cambio de estado, Fase 3 del cliente); Módulo de Autenticación Ligera (código de verificación por WhatsApp); rate limiting en la API y cola de reintentos hacia WhatsApp. | R9 (suplantación), R10 (dependencia de WhatsApp), R8 (saturación en picos) · Gap G2, G5 | Medio | 9 semanas | Responsable técnico |
| **Largo plazo** (7 jun – 4 dic 2027) | Registro de auditoría (usuario + fecha) y roles diferenciados (RBAC); monitoreo de disponibilidad y alerta de créditos, con la capa de aplicación en plan pago estable sin redundancia (ADR-003); catálogo digital y solicitud de pedido en línea (Fase 4 del cliente); política de retención e inducción. | R6 (instancia única), R13 (sin trazabilidad ni roles), R11 (dependencia de redes), apoya R4 y R14 · Gap G6 | Medio | 6 meses | Responsable técnico y Propietario |

> El esfuerzo es relativo (Bajo / Medio / Alto), estimado por el equipo según el alcance de cada fase; el responsable técnico lo convierte en horas y costo antes del 8 de febrero.

**Por qué el orden no sigue la prioridad del riesgo al pie de la letra.** El único riesgo crítico (R1, gestión manual, Nivel 9) tiene como solución definitiva el sistema propio, que es la fase de mayor esfuerzo. Por eso la primera fase no es esa construcción sino su versión provisional de esfuerzo bajo (registro en Sheets/Airtable), junto con el consentimiento y el aviso de privacidad (R5, bajo esfuerzo). Backup y cifrado (R2, R3) tienen prioridad 2 pero necesitan que exista la base de datos, así que llegan con el sistema. La verificación de identidad (R9) se etiquetó *quick win* en el Taller 7 por su esfuerzo, pero depende de WhatsApp Business API (Fase 3 del cliente, dependencia ya registrada en la hoja «Riesgos y Dependencias» del Taller 7), por lo que aquí cae en Mediano plazo. Del mismo modo, la «migración» a Supabase Pro del Taller 7 se vuelve «aprovisionar directamente en Pro» al desplegar el sistema: no hay base de datos previa que migrar.

## Cronograma de actividades

Cada fase se descompone en las actividades que la ejecutan. Filas = actividades; columnas = meses de 2027 (■ = mes con trabajo en esa actividad). El Quick win ocupa del 11/01 al 08/02.

![Cronograma de Work Packages 2027](vistas-finales/archimate-migracion.png)

| Actividad (Work Package) | Fechas 2027 | Riesgos | Ene | Feb | Mar | Abr | May | Jun | Jul | Ago | Sep | Oct | Nov | Dic |
|---|---|---|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| **WP-A · Quick win** | **11/01 – 08/02** | **R1, R5, R7** | ■ | ■ |  |  |  |  |  |  |  |  |  |  |
| A1 · Registro digital en Sheets/Airtable: 7 estados y pendientes | 11/01 – 24/01 | R1, R7 | ■ |  |  |  |  |  |  |  |  |  |  |  |
| A2 · Sesión práctica con la Encargada y guía de 1 página | 11/01 – 17/01 | R1, R4 | ■ |  |  |  |  |  |  |  |  |  |  |  |
| A3 · Aviso de privacidad, autorización y política de datos | 18/01 – 31/01 | R5 | ■ |  |  |  |  |  |  |  |  |  |  |  |
| A4 · Canal ARCO por WhatsApp y Responsable del Tratamiento | 25/01 – 07/02 | R5, R14 | ■ | ■ |  |  |  |  |  |  |  |  |  |  |
| A5 · Exportación semanal del registro (respaldo provisional) | 25/01 – 07/02 | R2 | ■ | ■ |  |  |  |  |  |  |  |  |  |  |
| **WP-B · Corto plazo** | **08/02 – 05/04** | **R2, R3, R12** |  | ■ | ■ | ■ |  |  |  |  |  |  |  |  |
| B1 · Aprovisionar Supabase Pro: cifrado en reposo y backup diario | 08/02 – 21/02 | R2, R3 |  | ■ |  |  |  |  |  |  |  |  |  |  |
| B2 · Desarrollo de la App Web y la API REST | 15/02 – 21/03 | R1, R7 |  | ■ | ■ |  |  |  |  |  |  |  |  |  |
| B3 · HTTPS/HSTS y validación de pertenencia del pedido | 08/03 – 21/03 | R3, R12 |  |  | ■ |  |  |  |  |  |  |  |  |  |
| B4 · Casilla de autorización en el formulario de solicitudes | 15/03 – 21/03 | R5 |  |  | ■ |  |  |  |  |  |  |  |  |  |
| B5 · Migración de datos, capacitación y salida a producción | 22/03 – 04/04 | R1, R7 |  |  | ■ | ■ |  |  |  |  |  |  |  |  |
| **WP-C · Mediano plazo** | **05/04 – 07/06** | **R8, R9, R10** |  |  |  | ■ | ■ | ■ |  |  |  |  |  |  |
| C1 · Cuenta de WhatsApp Business API (aprobación de Meta) | 05/04 – 18/04 | R10 |  |  |  | ■ |  |  |  |  |  |  |  |  |
| C2 · Notificación automática de estados y cola de reintentos | 19/04 – 01/05 | R8, R10 |  |  |  | ■ | ■ |  |  |  |  |  |  |  |
| C3 · Autenticación Ligera: código de verificación por WhatsApp | 10/05 – 30/05 | R9 |  |  |  |  | ■ |  |  |  |  |  |  |  |
| C4 · Rate limiting (límite de solicitudes) en la API | 24/05 – 06/06 | R8 |  |  |  |  | ■ | ■ |  |  |  |  |  |  |
| **WP-D · Largo plazo** | **07/06 – 04/12** | **R6, R11, R13** |  |  |  |  |  | ■ | ■ | ■ | ■ | ■ | ■ | ■ |
| D1 · Registro de auditoría y roles diferenciados (RBAC) | 07/06 – 18/07 | R4, R13 |  |  |  |  |  | ■ | ■ |  |  |  |  |  |
| D2 · Plan pago estable, monitoreo y alerta de créditos | 19/07 – 15/08 | R6 |  |  |  |  |  |  | ■ | ■ |  |  |  |  |
| D3 · Catálogo digital y solicitud de pedido en línea | 16/08 – 24/10 | R11 |  |  |  |  |  |  |  | ■ | ■ | ■ |  |  |
| D4 · Política de retención de datos e inducción | 25/10 – 03/12 | R4, R14 |  |  |  |  |  |  |  |  |  | ■ | ■ | ■ |

Las fechas de cada actividad son una propuesta del equipo dentro de las fechas de cada fase; el responsable técnico las ajusta antes del 8 de febrero. El mismo cronograma, con los Gaps que cierra cada fase y los Plateaus intermedios, es la vista ArchiMate `vistas-finales/archimate-migracion.drawio`.

**Picos de demanda que condicionan el calendario:** San Valentín (14/02/2027) y Día de la Madre (09/05/2027). Regla del plan: no se pasa a producción ni se cambia nada crítico durante la semana previa a cada pico. Para San Valentín opera el registro básico; el sistema propio entra en producción después. Los módulos de WhatsApp se despliegan antes del 2 de mayo (notificaciones, actividad C2) o desde el 10 de mayo (Autenticación Ligera, actividad C3).

## Dependencias entre fases

1. **Corto plazo depende del Quick win:** el registro es la fuente de los datos que se cargan al sistema, y la decisión de costo de Supabase Pro (ADR-001) debe estar dictaminada por el Propietario antes del 8 de febrero.
2. **Mediano plazo depende de Corto plazo** (necesita API y base de datos en producción) y de que Meta apruebe la cuenta de WhatsApp Business API, un plazo externo que el equipo no controla. Si se retrasa, la Encargada verifica la identidad manualmente por chat y las notificaciones siguen siendo manuales.
3. **El RBAC de Largo plazo debe estar listo antes de sumar una tercera persona** (ayuda temporal en temporada alta); contratar esa ayuda antes dispara el procedimiento de cambios.
4. **Punto abierto:** quién construye y mantiene el sistema no está definido. Sin responsable técnico antes del 8 de febrero, solo el Quick win es ejecutable.

## Gestión del cambio con las personas

Oasis tiene 2 personas y un grupo de clientes; el plan se limita a los grupos reales.

| Grupo | Qué pierde o teme | Resistencia esperada | Poder / interés | Comunicación y capacitación | Cuándo | Responsable |
|---|---|---|---|---|---|---|
| Encargada de Atención (usuaria diaria del panel) | Perder su forma ágil de responder por chat; que el panel la haga responsable de cada solicitud sin atender; no dominar una herramienta nueva | Seguir atendiendo solo por chat y registrar después —o no registrar—, dejando el registro desactualizado | Medio / Alto | Sesión práctica de 1 hora con pedidos reales; guía de 1 página con los 7 estados; la alerta de «solicitudes sin atender» se presenta como apoyo, no como control (BPMN, Plan del Cambio) | Semana 1 del Quick win y semana previa al arranque del sistema propio | Propietario, con apoyo del equipo AREM o del responsable técnico |
| Propietario (Administrador del sistema) | Un costo mensual fijo (≈USD 25 de Supabase Pro más el hosting) y el tiempo diario para mantener estados, catálogo y precios al día | Posponer la aprobación del costo o dejar de actualizar estados en semanas de mucha demanda y volver al chat | Alto / Alto | Una página por fase con el resultado visible y el costo; sesión de 30 min para validar los pesos y el precio de la matriz del Taller 7 | Antes del 8/02/2027 (decisión de costo) y al cierre de cada fase | Equipo AREM hasta la entrega; luego, el responsable técnico |
| Clientes (15 a 25 años) | Un paso extra para pedir o consultar (autorización, código); desconfianza al compartir datos | Dejar de consultar su pedido o seguir pidiendo solo por Instagram sin autorización | Bajo / Medio | Mensaje corto en el mismo chat de WhatsApp que explica para qué se pide la autorización y el código, con enlace a la política; sin instalar nada ni crear cuentas (principio P3) | Al activar el aviso (semana 2) y al activar el código de verificación (Mediano plazo) | Encargada de Atención |

### Resultado visible por fase

| Fase | Resultado visible | Quién lo comprueba (fuera de TI) |
|---|---|---|
| Quick win | Todos los pedidos activos aparecen en un solo registro con su estado y cada chat nuevo recibe el aviso de privacidad | Propietario (contrasta el registro con sus chats) |
| Corto plazo | Desde el celular, la Encargada ve «solicitudes sin atender» y «próximas entregas»; el Propietario ve en el panel del proveedor el plan Pro activo y el último backup diario | Encargada de Atención y Propietario |
| Mediano plazo | El cliente recibe en WhatsApp el aviso de cada cambio de estado y solo ve su pedido tras ingresar el código; bajan los mensajes «¿cómo va mi pedido?» que la Encargada responde a mano | Encargada de Atención (cuenta esos mensajes) |
| Largo plazo | Una persona de apoyo temporal toma pedidos con permisos limitados en la temporada alta siguiente, el sistema no sufre caídas y el catálogo en línea recibe solicitudes | Propietario |

## Referencias

- The Open Group. *TOGAF Standard, 10th Edition — ADM, fases E y F (Opportunities & Solutions; Migration Planning)*. https://www.opengroup.org/togaf
- The Open Group. *ArchiMate 3.2 Specification — Implementation and Migration Layer (Plateau, Gap, Work Package)*.
- Kotter, J. P. (1996). *Leading Change*. Harvard Business School Press (resultados visibles tempranos).
- Equipo 4 AREM. Plan de Migración y Plan del Cambio, `01-bpmn/informe.md`; matriz de brechas y matriz de decisión del Taller 7; `matriz-riesgos.xlsx` de este taller.
- Fuente asistida por IA: Claude (Anthropic), octubre 2026, para el borrador de secuenciación y redacción; el equipo debe validar fechas, esfuerzo y responsables.

---

_Este documento hace parte de la entrega final del Taller 9 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._

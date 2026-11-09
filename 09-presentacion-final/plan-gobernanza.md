# Plan de Gobernanza de la Implementación

## Cliente
Oasis Atelier Floral — Sopó, Cundinamarca.

## Integrantes del equipo
- Esteban Díaz Vargas
- Katherin Juliana Moreno Carvajal

## Propósito de este documento

Define quién supervisa la ejecución de `plan-implementacion.md`, con qué reglas se diseñó la arquitectura y qué pasa si una fase necesita desviarse de lo aprobado en el Taller 7. Está dimensionado para un negocio de 2 personas: un comité de tres roles que se reúne 30 minutos al mes, no un organismo corporativo.

## Principios Arquitectónicos

Cada principio permite decir «no» a algo concreto y se sostiene en un objetivo de la Ficha de Caracterización.

| Principio | Explicación | Justificación en el proyecto |
|---|---|---|
| **P1 · Simplicidad y bajo costo primero** | Toda decisión debe poder pagarla y operarla un negocio de 2 personas; se prefiere un servicio gestionado de bajo costo sobre infraestructura propia. **Dice «no» a:** microservicios, balanceadores y múltiples regiones, scripts propios de backup. | La Ficha fija como restricción «evitar infraestructura costosa o soluciones demasiado complejas». Sostiene el backend monolítico (ADR-002) y el descarte del script propio de backup (ADR-001). Atiende el objetivo 3. |
| **P2 · Privacidad y seguridad desde el diseño** | Ningún componente que toque datos personales entra en producción sin cifrado, autorización del titular y control de acceso. **Dice «no» a:** usar el plan gratuito con datos reales de clientes y publicar el formulario sin casilla de autorización. | El Taller 5 (T4) y el Taller 6 (ítems 1, 2, 5 y 6) diagnosticaron el mismo hueco en tres talleres; la Ley 1581 de 2012 es una restricción legal. Atiende el objetivo 2 (información centralizada y confiable). |
| **P3 · El cliente sigue en WhatsApp e Instagram; el sistema es soporte interno** | El sistema se adapta al cliente, no al revés: sin apps que instalar, sin cuentas ni contraseñas. **Dice «no» a:** portales de acceso obligatorios para pedir. | El plan de cambio del cliente establece que «Instagram y WhatsApp continuarán siendo los canales» y que no se modifica la experiencia del cliente final. Por eso la verificación de identidad es un código por WhatsApp. Atiende el objetivo 1. |
| **P4 · Crecimiento incremental guiado por riesgo** | Se agrega una pieza solo cuando un riesgo medido la justifica; lo que no tiene brecha diagnosticada va al backlog. **Dice «no» a:** construir inventario, reportes o redundancia antes de cerrar los riesgos de nivel 6 o más. | El Taller 7 dejó en backlog las ideas #3, #4, #5 y #7 por esta misma regla, y el plan ordena las fases por riesgo y esfuerzo. Atiende el objetivo 3 («herramientas que puedan crecer junto con la empresa»). |

## Estándares Tecnológicos

Un estándar tiene puerta: las excepciones se aprueban mediante una ADR (ver `procedimiento-cambios.md`).

| Categoría | Estándar definido | Por qué |
|---|---|---|
| Lenguaje / framework | TypeScript sobre Node.js para la API REST y para la App Web responsive (un solo lenguaje de punta a punta). *Supuesto del equipo: ajustar si ya se eligió otro stack.* | Un solo lenguaje reduce lo que debe saber un único responsable técnico y cuenta con librerías para Supabase y para WhatsApp Business API. Se desprende de P1. |
| Tipo de comunicación | REST/JSON síncrono sobre HTTPS entre App Web, API REST y WhatsApp Business API; sin brokers de eventos. | 15 a 20 pedidos al mes no justifican mensajería asíncrona; el C2 del Taller 3 ya usa REST/JSON. Excepción prevista vía ADR: cola de reintentos hacia WhatsApp (backlog #7). P1 y P4. |
| Infraestructura | Aplicación (App Web + API) en Render o Railway, una sola instancia en plan pago al pasar a producción; datos en Supabase Pro (PostgreSQL gestionado con cifrado en reposo y backup diario); sin balanceador ni multi-región. | Continúa el mapa del Taller 4 y el TO-BE del Taller 7; ADR-001 y ADR-003. P1 y P2. |
| Seguridad y datos personales | HTTPS con HSTS obligatorio; verificación por código de WhatsApp para consultar o modificar un pedido; autorización de tratamiento y política de datos antes de guardar datos; dos roles internos con auditoría de usuario y fecha. | Trazado a las mitigaciones de los Talleres 5 y 6. P2 y P3. |

**Prueba de la escalera:** *estándar → principio → objetivo de la Ficha.* TypeScript → P1 → objetivo 3. REST/JSON → P1 y P4 → objetivo 3. Supabase Pro → P2 → objetivo 2. Seguridad y datos → P2 → objetivo 2. Código por WhatsApp → P3 → objetivo 1. Ningún estándar queda sin principio.

## Mecanismo de gobierno

Comité de Arquitectura de Oasis (3 roles, mismo foro de la «evaluación mensual» definida en el informe de BPMN).

| Rol | Responsabilidad | Persona / equipo sugerido |
|---|---|---|
| Propietario | Dictamina ADR y excepciones, aprueba costos y es Responsable del Tratamiento de datos personales | Juan David Cetina Gómez |
| Encargada de Atención | Valida que cada fase sea operable en el día a día (resultado visible) y reporta fricciones | Encargada de Atención |
| Responsable técnico | Propone ADR y excepciones, verifica la conformidad contra el TO-BE y mantiene el registro de ADR | Por designar antes del 08/02/2027; hasta la entrega, el equipo AREM |

Quórum: Propietario y Responsable técnico.

## Cadencia de seguimiento

Reunión mensual de 30 minutos mientras dure el roadmap (ene–dic 2027) y trimestral después. Al cierre de cada fase, un informe de una página con el resultado visible, el costo real y las métricas de gobierno.

## Criterio de conformidad

Una fase se cierra solo si cumple todas sus verificaciones contra el TO-BE (`to-be-aplicaciones-final.drawio`, `to-be-tecnologia-final.drawio` del Taller 7 y el tablero del Taller 8). Cada verificación es una prueba, no una opinión.

| Fase | Verificaciones |
|---|---|
| Quick win | Todos los chats nuevos reciben aviso de privacidad y queda registro de la autorización; todos los pedidos activos están en el registro; la política de datos está publicada; hay una exportación semanal con fecha. |
| Corto plazo | El plan Pro de Supabase está activo (captura del panel); existe confirmación escrita del proveedor sobre el cifrado en reposo; se restauró con éxito un backup de prueba; el certificado TLS es válido y HSTS está activo (revisión con SSL Labs); al cambiar el identificador de un pedido por el de otro cliente, el sistema niega el acceso; la casilla de autorización es obligatoria. |
| Mediano plazo | Sin código de verificación no se muestra el pedido; el código llega al WhatsApp del cliente; cada cambio de estado genera una notificación; un mensaje fallido se reintenta; las solicitudes por encima del límite configurado se rechazan. |
| Largo plazo | Cada cambio de estado deja usuario y fecha; un usuario con rol limitado no puede editar precios; la alerta de disponibilidad se activa en un simulacro; el catálogo en línea recibe una solicitud con autorización. |

## Escalamiento

1. El Responsable técnico registra la desviación por escrito como ADR en estado *Propuesta* (qué cambia, por qué, efecto en riesgo y costo).
2. La Encargada indica si afecta la operación diaria.
3. El Propietario dictamina en máximo 5 días hábiles, por mensaje o reunión extraordinaria. Sin dictamen, la desviación no se asume aprobada y esa parte de la fase no avanza.
4. Si la desviación toca datos personales o seguridad, debe documentarse un control equivalente antes de aprobarla.

## Métricas de gobierno

| Métrica | Qué mide (principio) | Fórmula | Fuente que ya existe | Línea base | Umbral |
|---|---|---|---|---|---|
| M1 · Cobertura normativa | Si se cumple **P2 Privacidad y seguridad desde el diseño** | Ítems del checklist normativo en «Cumple» ÷ 13 ítems × 100 | `checklist-cliente.xlsx` (Taller 6) | 1 de 13 = 7,7 % (solo HTTPS en tránsito) | Menos de 7 de 13 (54 %) al cerrar Corto plazo (05/04/2027) |
| M2 · Riesgos de nivel ≥ 6 mitigados | Si se cumple **P4 Crecimiento guiado por riesgo** | Riesgos de nivel ≥ 6 con mitigación implementada y verificada ÷ 7 riesgos de nivel ≥ 6 × 100 | `matriz-riesgos.xlsx` (R1 a R7) | 0 de 7 = 0 % | Menos de 5 de 7 (71 %) al cerrar Mediano plazo (07/06/2027) |
| M3 · Tiempo de dictamen del comité (mide al comité) | Si el **gobierno** estorba (**P1 Simplicidad**) | Días hábiles entre la propuesta de una ADR o excepción y el dictamen del Propietario | Registro de ADR de `procedimiento-cambios.md` | Sin línea base: ninguna ADR tiene dictamen todavía y el registro empieza con este plan | Más de 5 días hábiles |

**Cómo se llegó a los umbrales.** M1: con el Quick win cierran los ítems 1, 2, 3 y 7 y con el Corto plazo los ítems 5 y 6; sumados al ítem 4 que ya cumple, son 7 de 13. M2: al cierre de Mediano plazo quedan mitigados R1, R2, R3, R5 y R7; R4 (dependencia de personas) y R6 (instancia única) se atienden en Largo plazo y gobernanza. M3: las ADR-001 a 003 se llevan al Propietario por primera vez en la sustentación del 14/11/2026; ese primer dictamen no cuenta como línea base porque aún no existía el comité.

---

_Este documento hace parte de la entrega final del Taller 9 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._

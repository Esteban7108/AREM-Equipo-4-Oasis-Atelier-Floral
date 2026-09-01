# Arquitectura Empresarial — Oasis

**Floristería personalizada · Sopó y Sabana Norte, Cundinamarca**

Oasis es una floristería dedicada al diseño y elaboración de arreglos florales personalizados por encargo. El proyecto busca transformar el proceso actual de gestión de clientes y pedidos mediante un sistema de información que permita centralizar solicitudes, cotizaciones y seguimiento.

---

## 1. Visión de la Arquitectura

### ¿Quiénes somos?

**Origen**

Oasis nace al identificar que en Sopó predominaban floristerías enfocadas en arreglos básicos. El propietario detectó una oportunidad para ofrecer arreglos estéticos y personalizados, inspirados en tendencias de redes sociales.

**Actividad**

* Diseño y elaboración de arreglos florales personalizados por encargo.
* Oasis no cultiva flores; las adquiere mediante proveedores y las transforma.
* Elaboración de tarjetería propia.
* En fechas especiales se realizan alianzas con emprendimientos de postres.

**Equipo**

El equipo está compuesto por 2 personas:

* **Propietario:** elaboración de arreglos, compras, tarjetería y apoyo en entregas.
* **Mamá:** redes sociales, publicidad, contacto con clientes y apoyo en entregas.

**Clientes y canales**

* Principal segmento: jóvenes entre 15 y 25 años.
* **Instagram:** reconocimiento y llegada de nuevos clientes.
* **WhatsApp:** recepción y gestión de pedidos.
* El crecimiento se ha producido principalmente mediante voz a voz y recomendaciones.

### Misión

Diseñar y elaborar arreglos florales personalizados de alta calidad para clientes en Sopó y la Sabana Norte, ofreciendo una experiencia estética, cercana y a la medida de cada ocasión.

### Visión

Ser reconocida como la floristería líder en personalización y calidad de la Sabana Norte, contando con un punto físico propio que permita llegar a nuevos segmentos de clientes.

### Objetivo del proyecto

Optimizar la gestión de clientes y pedidos de Oasis mediante un sistema de información sencillo que centralice el registro y seguimiento de las solicitudes, con el propósito de reducir tiempos de respuesta y evitar la pérdida de ventas.

### Unidad de negocio seleccionada

El levantamiento identificó cinco procesos:

1. Gestión de pedidos.
2. Abastecimiento.
3. Elaboración del arreglo.
4. Validación y entrega.
5. Marketing y clientes.

El proceso seleccionado fue **Gestión de Clientes y Pedidos**, debido a que:

* Es una de las actividades más repetitivas del día a día.
* Cada arreglo implica una conversación con el cliente.
* Es una de las principales causas de retrasos.
* Algunos mensajes pueden responderse hasta el día siguiente.
* Es viable solucionarlo mediante un sistema de información sencillo.
* Permite mejorar el contacto, la cotización y el seguimiento sin afectar la calidad de elaboración de los arreglos.

---

## 2. Capa de Negocio

### Modelo de negocio actual

**Captación y pedidos**

* Instagram se utiliza para reconocimiento y llegada de nuevos clientes.
* WhatsApp se utiliza para recibir y gestionar pedidos.
* Actualmente el flujo se maneja principalmente mediante conversaciones manuales.

**Producción bajo pedido**

* Las flores se compran a proveedores cerca de la fecha de entrega para garantizar su frescura.
* Una compra semanal permite elaborar aproximadamente hasta 10 arreglos.

**Roles del equipo**

* **Propietario:** elaboración, compra de materia prima, diseño e impresión de tarjetas y apoyo en entregas.
* **Mamá:** publicidad, manejo de redes sociales y contacto con clientes.

**Alianzas**

Durante fechas comerciales como San Valentín y Día de la Madre, Oasis establece alianzas con emprendimientos de productos complementarios, como mini donas y fresas con chocolate.

### Problemas identificados

* Comunicación tardía con los clientes.
* Mensajes que pueden quedar sin responder durante horas o hasta el día siguiente.
* No existe un registro centralizado de solicitudes.
* No existe un registro centralizado de cotizaciones.
* No existe un seguimiento sistemático del estado de cada pedido.
* La información depende de la memoria y del historial de WhatsApp.
* Los proveedores no tienen una relación fija con Oasis.
* Existe variabilidad en la disponibilidad y calidad de las flores.
* Es difícil identificar qué solicitudes quedaron sin atender.

### Consecuencias

* Menor engagement con los clientes.
* Dificultad para captar nuevos clientes.
* Riesgo de pérdida de ventas.
* Falta de control sobre el proceso de venta.

---

## 3. Arquitectura del Sistema de Información

### Información principal del sistema

**Cliente**

* Nombre.
* Teléfono o usuario.
* Canal de contacto.

**Solicitud**

* Descripción de lo solicitado.
* Referencia o imagen.
* Fecha de solicitud.

**Cotización**

* Precio.
* Condiciones ofrecidas.
* Relación con la solicitud aceptada.

**Pedido y estado**

Los pedidos pueden manejar los siguientes estados:

1. Solicitado.
2. Cotizado.
3. Confirmado.
4. En elaboración.
5. Pendiente de aprobación.
6. Listo.
7. Entregado.

**Arreglo y materia prima**

* Tipo de flor.
* Accesorios.
* Tarjetería.
* Flores utilizadas.
* Proveedor asociado.

**Proveedor y entrega**

* Datos del proveedor.
* Dirección de entrega.
* Fecha de entrega.
* Responsable de la entrega.

### Modelo de datos

El sistema contempla un modelo entidad-relación y un modelo relacional basado en las entidades necesarias para gestionar:

* Clientes.
* Solicitudes.
* Cotizaciones.
* Pedidos.
* Arreglos.
* Proveedores.
* Materia prima.
* Entregas.

El modelo relacional utiliza **llaves primarias (PK)** y **llaves foráneas (FK)** para permitir su implementación en una base de datos SQL.

---

## 4. Arquitectura Tecnológica

### Modelo cliente-servidor

Se propone una arquitectura sencilla de acuerdo con el tamaño y las necesidades de Oasis.

### Cliente

**Aplicación web ligera / WhatsApp**

Permite a la propietaria y a su mamá:

* Registrar solicitudes.
* Crear cotizaciones.
* Consultar pedidos.
* Revisar estados.

El acceso se realiza desde el celular mediante un navegador, sin necesidad de instalar software adicional.

### Servidor

**API REST**

Se encarga de:

* Gestionar solicitudes.
* Gestionar cotizaciones.
* Gestionar pedidos.
* Administrar estados.
* Gestionar notificaciones.
* Servir como punto central de acceso a los datos.

### Base de datos

**Base de datos relacional SQL**

Contiene la información de:

* Clientes.
* Solicitudes.
* Cotizaciones.
* Pedidos.
* Arreglos.
* Proveedores.

### Infraestructura

Se plantea utilizar un proveedor cloud de bajo costo, por ejemplo:

* Railway.
* Render.
* Supabase.

### Integración futura

Se contempla una integración con **WhatsApp Business API** para enviar automáticamente notificaciones relacionadas con cambios en el estado de los pedidos.

---

## 5. Oportunidades y Soluciones

Las principales iniciativas propuestas son:

### Solicitudes pendientes

Crear un panel de **solicitudes pendientes de respuesta** para identificar rápidamente los mensajes que aún no han sido atendidos.

### Registro digital

Crear un registro centralizado de:

* Clientes.
* Solicitudes.
* Pedidos.
* Estados.

Esto reemplaza la dependencia de la memoria y del historial disperso de WhatsApp.

### Cotización estándar

Implementar una plantilla de cotización dentro del sistema para agilizar una de las actividades más repetitivas.

### Estados de pedido

Permitir visualizar claramente el estado de cada pedido:

`Solicitado → Cotizado → Confirmado → En elaboración → Pendiente de aprobación → Listo → Entregado`

### Historial por cliente

Permitir consultar pedidos anteriores para facilitar:

* Personalización.
* Seguimiento.
* Fidelización.

### Panel principal

El sistema debe mostrar principalmente:

* Pedidos pendientes.
* Próximas entregas.
* Solicitudes sin atender.

El panel está diseñado para las necesidades del equipo de 2 personas.

---

## 6. Plan de Migración

La implementación será gradual para reducir el impacto sobre la operación.

### Fase 1 — 0 a 1 mes

**MVP sin desarrollo**

* Implementar un registro digital básico.
* Utilizar Google Sheets o Airtable.
* Centralizar solicitudes y estados.

### Fase 2 — 1 a 3 meses

**Sistema propio**

* Desarrollar formulario de solicitudes.
* Desarrollar panel de pedidos.
* Implementar el modelo de datos definido.

### Fase 3 — 3 a 5 meses

**Notificaciones**

* Integrar WhatsApp.
* Automatizar notificaciones sobre cambios de estado.

### Fase 4 — 5 a 6+ meses

**Panel completo**

Incorporar:

* Historial de clientes.
* Agenda de entregas.
* Reportes.
* Evaluación de crecimiento hacia un punto físico.

---

## 7. Implementación de Gobernanza

### Roles y responsabilidades

**Administrador del sistema**

El propietario será responsable de:

* Mantener actualizados los estados de los pedidos.
* Mantener actualizado el catálogo.
* Administrar precios.
* Mantener las plantillas de cotización.

**Usuaria de atención**

La mamá será responsable de:

* Revisar el panel de solicitudes.
* Dar primera respuesta.
* Mantener actualizado el contacto de los clientes.

### Respaldo de datos

Se deben realizar copias de seguridad periódicas de la información de:

* Clientes.
* Pedidos.

El objetivo es evitar depender de un único dispositivo.

### Privacidad

Los datos de contacto de los clientes, como nombre y teléfono, deben utilizarse únicamente para gestionar los pedidos y no deben compartirse con terceros.

### Revisión periódica

Se propone una evaluación mensual de:

* Tiempos de respuesta.
* Solicitudes pendientes.
* Cumplimiento de los objetivos del sistema.

### Control de cambios

Cualquier modificación al:

* Catálogo de estados.
* Plantilla de cotización.

debe ser autorizada por la propietaria, como responsable del proceso.

---

## 8. Plan del Cambio

### Gestión del cambio

* Realizar una capacitación breve y práctica sobre el uso del panel de solicitudes y pedidos.
* No se requiere formación técnica extensa.
* Instagram y WhatsApp continuarán siendo los canales de comunicación con los clientes.
* El sistema funcionará principalmente como soporte interno para organizar la información recibida.
* No es necesario modificar la experiencia del cliente final.

### Gestión de la resistencia

El sistema incorpora alertas de solicitudes sin atender como mecanismo de apoyo para evitar retrasos en las respuestas.

La herramienta busca apoyar el criterio de la propietaria, no reemplazarlo.

### Adopción

El proceso de adopción seguirá las mismas fases establecidas en el plan de migración, comenzando con un registro digital básico.

---

## 9. Resumen del Proyecto

El proyecto propone diseñar un **sistema de información para apoyar la gestión de clientes y pedidos de Oasis**, centralizando:

* Registro de solicitudes.
* Seguimiento de clientes.
* Cotizaciones.
* Estados de pedidos.
* Historial de clientes.
* Información necesaria para las entregas.

El propósito principal es **reducir los retrasos en la atención y mejorar el control del proceso de venta**, manteniendo Instagram y WhatsApp como canales principales de contacto con los clientes.

## Referencias — Visión de la Arquitectura

The Open Group. (2022). TOGAF® Standard, 10th Edition — Introduction and Core Concepts, Fase A: Arquitectura de la Visión. https://www.opengroup.org/togaf
Zachman, J. A. (1987). A Framework for Information Systems Architecture. IBM Systems Journal, 26(3), 276–292.
Universidad de La Sabana, Facultad de Ingeniería. (2025). Arquitectura Empresarial [material de curso]. Ing. César Augusto Vega Fernández.

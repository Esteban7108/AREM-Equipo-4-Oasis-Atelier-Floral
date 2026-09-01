# Informe Técnico del Taller

## Nombre del Taller

🛠️ Taller: Modelado del Proceso de Gestión de Clientes y Pedidos con BPMN

## 👥 Integrantes del equipo

 Esteban Díaz Vargas

 Katherin Juliana Moreno Carvajal

## 🧠 Descripción general del trabajo

Modelar el proceso de negocio de **gestión de clientes y pedidos de Oasis** utilizando la notación BPMN, identificando los actores involucrados, eventos, actividades, decisiones y puntos críticos del proceso actual.

El proceso fue seleccionado debido a que la comunicación y el seguimiento de los clientes representan una de las principales oportunidades de mejora identificadas en el levantamiento realizado a la empresa. Actualmente, la gestión se realiza principalmente mediante Instagram y WhatsApp, sin un sistema de información que centralice las solicitudes, cotizaciones y estados de los pedidos.

## 🔧 Proceso de desarrollo

### Inicio

El cliente se comunica con Oasis mediante los canales disponibles, principalmente Instagram o WhatsApp, con el objetivo de solicitar un arreglo floral personalizado.

El proceso comienza cuando el cliente realiza una **solicitud de información o de un arreglo**, proporcionando los datos y características que requiere.

### Recepción de la solicitud

El equipo de Oasis recibe el mensaje del cliente y revisa la información proporcionada.

Posteriormente, se analiza la solicitud para determinar las características del arreglo y las condiciones necesarias para realizar una cotización.

### Elaboración de la cotización

Oasis determina el precio y las condiciones del arreglo solicitado y comunica la cotización al cliente.

El cliente debe decidir si desea continuar con el pedido.

### Decisión del cliente

Si el cliente **no acepta la cotización**, el proceso finaliza sin realizar el pedido.

Si el cliente **acepta la cotización**, se procede a registrar y confirmar el pedido.

### Confirmación del pedido

Una vez aceptada la cotización, Oasis registra el pedido y establece la información necesaria para continuar con su elaboración.

El pedido pasa por los diferentes estados definidos para su seguimiento:

* Solicitado.
* Cotizado.
* Confirmado.
* En elaboración.
* Pendiente de aprobación.
* Listo.
* Entregado.

### Elaboración y entrega

Una vez confirmado el pedido, el propietario realiza la elaboración del arreglo floral utilizando las flores, accesorios y demás elementos necesarios.

Cuando el arreglo está terminado, se valida que esté listo para ser entregado al cliente.

Finalmente, se realiza la entrega y el pedido pasa al estado de **Entregado**.

### Fin

El proceso finaliza cuando el pedido ha sido entregado satisfactoriamente al cliente.

El BPMN representa el flujo actual de gestión de clientes y pedidos, donde gran parte de las actividades dependen de la comunicación manual mediante Instagram y WhatsApp.

---

## 📋 Tabla de actores, entidades o componentes

## Tabla de Elementos del Diagrama BPMN

| Nombre del elemento    | Tipo                    | Descripción                                                                                                          | Responsable  |
| ---------------------- | ----------------------- | -------------------------------------------------------------------------------------------------------------------- | ------------ |
| Cliente                | Actor                   | Persona que solicita un arreglo floral personalizado y proporciona las características necesarias para el pedido.    | Cliente      |
| Oasis                  | Actor                   | Responsable de recibir solicitudes, elaborar cotizaciones, gestionar pedidos, elaborar arreglos y realizar entregas. | Equipo Oasis |
| Inicio                 | Evento de Inicio        | Marca el comienzo del proceso cuando el cliente realiza una solicitud.                                               | Cliente      |
| Realizar solicitud     | Actividad               | El cliente comunica a Oasis las características del arreglo que desea solicitar.                                     | Cliente      |
| Recibir solicitud      | Actividad               | Oasis recibe y revisa la solicitud realizada por el cliente.                                                         | Oasis        |
| Elaborar cotización    | Actividad               | Oasis determina el precio y las condiciones correspondientes a la solicitud.                                         | Oasis        |
| Enviar cotización      | Actividad               | Oasis comunica al cliente el precio y las condiciones del arreglo solicitado.                                        | Oasis        |
| ¿Acepta la cotización? | Gateway Exclusivo (XOR) | Determina si el cliente desea continuar con el pedido.                                                               | Cliente      |
| Finalizar solicitud    | Evento de Fin           | Finaliza el proceso cuando el cliente decide no continuar con la cotización.                                         | Cliente      |
| Confirmar pedido       | Actividad               | Se confirma la solicitud del cliente y se registra el pedido para continuar con el proceso.                          | Oasis        |
| Elaborar arreglo       | Actividad               | El propietario diseña y elabora el arreglo floral solicitado.                                                        | Oasis        |
| Validar arreglo        | Actividad               | Se verifica que el arreglo esté terminado y listo para ser entregado.                                                | Oasis        |
| ¿Arreglo listo?        | Gateway Exclusivo (XOR) | Determina si el arreglo cumple las condiciones necesarias para continuar con la entrega.                             | Oasis        |
| Realizar entrega       | Actividad               | Oasis realiza la entrega del arreglo al cliente.                                                                     | Oasis        |
| Pedido entregado       | Evento de Fin           | Finaliza el proceso cuando el pedido ha sido entregado al cliente.                                                   | Oasis        |

## Referencias — BPMN (Capa de Negocio)

Object Management Group (OMG). Business Process Model and Notation (BPMN) Specification (sección introductoria). https://www.omg.org/spec/BPMN
Camunda. BPMN Cheat Sheet. https://camunda.com/bpmn/reference/
Bizagi. BPMN Training Guide. https://www.bizagi.com/
Astah. (2020). BPMN look and feel. https://astahblog.com/2020/05/29/bpmn-look-and-feel/


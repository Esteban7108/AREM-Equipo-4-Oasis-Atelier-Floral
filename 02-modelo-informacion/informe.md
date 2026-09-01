# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
Modelo Entidad-Relación y Modelo Relacional del caso Oasis (Floristería)_

## 👥 Integrantes del equipo
- Katherin Juliana Moreno Carvajal
- David Esteban Díaz Vargas

## 🧠 Descripción general del trabajo
El taller consistió en construir el modelo de datos del proceso comercial y productivo de Oasis, una floristería que atiende solicitudes de arreglos florales personalizados. El objetivo fue representar, primero mediante un modelo Entidad-Relación (ERD) conceptual y luego mediante un modelo relacional con atributos y llaves, el ciclo completo del negocio: desde que un cliente realiza una solicitud, pasando por la cotización y el pedido, hasta la elaboración del arreglo con materia prima suministrada por proveedores y su entrega final.

Se entregaron dos diagramas complementarios: un modelo entidad-relación (ERD_OASIS) que expresa las entidades del dominio, sus relaciones y cardinalidades en notación clásica, y un modelo relacional (Modelo_Relacional_Oasis) que traduce ese diseño conceptual a una estructura de tablas con tipos de dato, llaves primarias (PK) y llaves foráneas (FK), lista para su implementación en un motor de base de datos relacional.

## 🔧 Proceso de desarrollo
El equipo abordó el modelado en dos etapas secuenciales:

- **Identificación de entidades del negocio:** se analizó el flujo operativo de la floristería (solicitud → cotización → pedido → arreglo → entrega) y el flujo de abastecimiento (proveedor → materia prima), identificando ocho entidades principales: CLIENTE, SOLICITUD, COTIZACION, PEDIDO, ARREGLO, MATERIA_PRIMA, PROVEEDOR y ENTREGA.
- **Definición de relaciones y cardinalidades:** se establecieron las relaciones entre entidades (realiza, genera, origina, incluye, compone, produce, suministra) y se asignó su cardinalidad mínima y máxima (1,1 / 1,N / 0,1 / 0,N) según las reglas de negocio del caso, por ejemplo que una solicitud puede o no derivar en una cotización (0,1) mientras que un pedido siempre incluye al menos un arreglo (1,N).
- **Construcción del ERD conceptual:** con la herramienta draw.io se representaron las entidades como rectángulos, las relaciones como rombos y los atributos clave (identificadores y campos relevantes) como elipses conectadas a sus entidades, siguiendo la notación clásica de Chen.
- **Derivación del modelo relacional:** a partir del ERD se definieron las tablas correspondientes, asignando tipo de dato a cada atributo (INT, VARCHAR, TEXT, DATE, DATETIME2, NUMERIC), marcando las llaves primarias (PK) y agregando las llaves foráneas (FK) necesarias para materializar cada relación como una referencia entre tablas.
- **Ajuste iterativo:** se revisaron los nombres de atributos y las cardinalidades para que el modelo relacional fuera consistente con el ERD, evitando redundancias y garantizando que cada relación N:M (como ARREGLO–MATERIA_PRIMA en el suministro) quedara resuelta mediante llaves foráneas.

## 🧩 Análisis del modelo propuesto

### Estructura del modelo entregado
El modelo se organiza en dos cadenas relacionadas entre sí. La primera cadena comercial conecta CLIENTE → SOLICITUD → COTIZACION → PEDIDO → ARREGLO, reflejando el recorrido natural de una venta: un cliente realiza una o varias solicitudes (1,N), cada solicitud genera como máximo una cotización (0,1, pues una solicitud puede ser rechazada o quedar sin cotizar), cada cotización origina como máximo un pedido (0,1) y cada pedido incluye uno o varios arreglos (1,N). La segunda cadena de abastecimiento conecta PROVEEDOR → MATERIA_PRIMA → ARREGLO, y una tercera rama logística conecta PEDIDO → ENTREGA → PROVEEDOR (esta última relación de "suministra" corresponde en realidad al vínculo proveedor–materia prima, mostrado en la parte inferior del ERD junto a la entidad ENTREGA).

En el modelo relacional, esta estructura se traduce en ocho tablas. Cinco de ellas (SOLICITUD, COTIZACION, PEDIDO, ARREGLO, ENTREGA) contienen una llave foránea que apunta a la tabla anterior en su cadena, materializando así las relaciones 1 a N del ERD. La tabla MATERIA_PRIMA es la más particular: contiene dos llaves foráneas (id_proveedor e id_arreglo), lo que le permite actuar como tabla de enlace entre PROVEEDOR y ARREGLO, resolviendo de forma relacional la relación de suministro.

### Representación de las necesidades del cliente
El modelo cubre el ciclo de vida completo de un pedido personalizado: capturar la solicitud original (incluida una referencia de imagen, típica en encargos de diseño floral), formalizarla en una cotización con precio y condiciones, convertirla en un pedido con fecha de entrega y estado, descomponerla en uno o varios arreglos con su propio detalle de flores y accesorios, y finalmente registrar la entrega con dirección y responsable. Esto permite a Oasis dar trazabilidad a cada etapa del proceso comercial y responder preguntas de negocio como qué solicitudes no llegaron a convertirse en pedido, qué materia prima proviene de qué proveedor o cuál es el estado actual de una entrega.

### Supuestos tomados
- Una solicitud puede no derivar en cotización, y una cotización puede no derivar en pedido (cardinalidad 0,1), representando la posibilidad de que el cliente no acepte la propuesta.
- Un pedido agrupa uno o varios arreglos (1,N), asumiendo que un mismo pedido puede incluir distintas composiciones florales.
- La relación entre ARREGLO y MATERIA_PRIMA es de tipo muchos a muchos en el negocio (un arreglo usa varios tipos de flor y una misma flor se usa en varios arreglos), resuelta en el modelo relacional mediante la tabla MATERIA_PRIMA como entidad asociativa con doble llave foránea.
- Cada pedido tiene una única entrega asociada (1,1), asumiendo que no se contemplan entregas parciales o fraccionadas en el alcance del taller.
- Los campos de fecha se tipificaron como DATETIME2 cuando registran un evento con hora (solicitud, cotización) y como DATE cuando solo interesa el día (entrega del pedido).


## 📋 Tabla de actores, entidades o componentes

| Nombre del elemento | Tipo | Descripción | Responsable |
|---|---|---|---|
| CLIENTE | Entidad | Persona que solicita y encarga arreglos florales; origina todo el flujo del negocio. | Cliente |
| SOLICITUD | Entidad | Registro de la petición inicial del cliente, con descripción, referencia de imagen y estado. | Equipo comercial |
| COTIZACION | Entidad | Propuesta de precio y condiciones generada a partir de una solicitud. | Equipo comercial |
| PEDIDO | Entidad | Compromiso formal derivado de una cotización aceptada; define fecha y estado de entrega. | Equipo de producción |
| ARREGLO | Entidad | Composición floral asociada a un pedido, con tipo de flor y accesorios. | Equipo de producción |
| MATERIA_PRIMA | Entidad | Insumos (flores y materiales) usados en un arreglo y suministrados por un proveedor. | Equipo de producción |
| PROVEEDOR | Entidad | Tercero que suministra materia prima a la floristería. | Equipo de compras |
| ENTREGA | Entidad | Registro logístico de la entrega física de un pedido al cliente. | Equipo de logística |



---

_Este documento hace parte de la entrega del Taller 3 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._

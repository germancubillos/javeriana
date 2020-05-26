# EAES_Modelado y Validación de Arquitectura
### Taller 4 - Banco ABC
El siguiente proyecto pretende modelar e implementar una solución de arquitectura utilizando una aproximación orientada a servicios utilizando los principios de diseño de servicios, diseño de patrones y estrategias para la construcción de arquitectura orientada a microservicios.



# Indice
1. [Contexto](#CONTEXTO)
2. [Arquitectura de Solución](#ARQUITECTURA DE SOLUCIÓN)
3. [PATRONES DE ARQUITECTURA](#PATRONES DE ARQUITECTURA)


# CONTEXTO

El Banco ABC está realizando varios proyectos de actualización tecnológica los cuales le permiten ofrecer sus productos financieros de manera más ágil y de esta forma responder a nuevas necesidades del mercado. El Banco ABC quiere tener la posibilidad de adicionar nuevos convenios con otros proveedores de servicios de manera ágil, o incluso la posibilidad de terminar/eliminar los convenios existentes sin que esto represente indisponibilidad del servicio. Se llegó a un acuerdo de las capacidades/primitivas básicas que se deben soportar para cada convenio:


- Consulta de saldo a Pagar
- Pago del Servicio
- Compensación de pago (Opcional)

Principalmente el banco necesita un conjunto de servicios que representen sus necesidades internas de negocio, lo cual les permite desacoplar los servicios de los proveedores y así no depender de sus detalles.

# ARQUITECTURA DE SOLUCIÓN

La solución aquí planteada está basada en la necesidad de permitir al negocio una mayor interoperabilidad y un desacoplamiento de la lógica de negocio con la integración de los servicios de los sistemas de proveedores, lo que permite adicionar, eliminar o actualizar convenios de manera ágil y de manera trasparente para no generar indisponibilidades en la prestación del servicio. 

## PATRONES DE ARQUITECTURA

La solución planteada está principalmente basada sobre el estilo arquitectural de microservicios, la cual permite tener la encapsulación de una única responsabilidad y la independencia necesaria para la integración y orquestación de las operaciones necesarias en la consulta y pago de los servicios, así como también de su escalamiento, evolución o eliminación sin generar mayor impacto en la solución.

Los servicios se comunican entre sí a través de APIs bien definidas. A continuación, se detallan los patrones complementarios en los que se soporta la solución: 

-	API Gateway : Se realiza la implementación de un API Gateway con Apache Camel sobre sprint boot. Esta API permite tener un único punto de acceso a los diferentes microservicios expuestos por el Banco ABC. Allí se expone una puerta de enlace para cada uno de los microservicios que se tienen configurados actualmente y no se cuenta con ninguna lógica de negocio.

-	API Composition : Este patrón implementa una operación de consulta invocando los servicios que poseen los datos y combinando sus resultados. Allí se tienen dos tipos de participantes, un compositor el cual implementa la operación de consulta y un proveedor quien es el que posee algunos de los datos que devuelve la consulta. Esto permite realizar la composición por orquestación de los diferentes microservicios/servicios legados que se requieren para realizar las operaciones de consulta de saldo y pago de servicios y así solventar la necesidad de gestionar diversos proveedores sin impactar el sistema y su disponibilidad.

-	Intermediate Routing : Este patrón permite realizar el enrutamiento de mensajes a otros servicios o legados, en la solución que se diseña aquí se implementa bajo la funcionalidad de Content-Based Routing, que, en esencia, realizar el enrutamiento de los mensajes basados en su contenido, como por ejemplo el id de la referencia de pago. Puede ser utilizado para modelar procesos de negocio complejos y proporcionar una forma eficiente de recomponer los servicios sobre la ejecución.

-	Decoupled Contract :  Este patrón permite desacoplar el contrato del servicio de su implementación, permitiendo así que el servicio pueda evolucionar sin afectar directamente a los consumidores. Para esto los contratos se definieron bajo el principio de contrato primero.

-	Data Model Transformation : La lógica de transformación de modelo de datos se puede introducir para llevar a cabo la conversión en tiempo de ejecución de datos, de modo que estos se ajusten a un modelo de datos, pueden ser reestructurados para cumplir a un modelo de datos diferente. Esto se extiende un marco de mensajería no estandarizada lo que le permite superar dinámicamente disparidad entre los esquemas utilizados por un contrato de servicio y los mensajes transmitidos a ese contrato.

-	Service Inventory : Un inventario de servicios es una colección de servicios complementarios estandarizados y gobernados de forma independiente dentro de un límite que representa una empresa o un segmento significativo de una empresa.


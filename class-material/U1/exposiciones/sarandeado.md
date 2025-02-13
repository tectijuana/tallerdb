**Caso: Sistema de Gestión de Ventas al Mayoreo de Mariscos**

**Entrevista con el cliente:**

*Cliente*: Hola, necesito un sistema para organizar las ventas al mayoreo de mariscos en mi negocio. Actualmente llevamos todo en libretas, pero ya no es eficiente porque manejamos muchos pedidos y clientes.

*Consultor*: Entendido. ¿Qué procesos son los más importantes para su negocio?

*Cliente*: Principalmente, registrar los pedidos que nos hacen los clientes. Necesito saber qué productos pidieron, en qué cantidades y a qué precio. Además, quiero llevar un control del inventario y saber qué productos necesitamos reponer.

*Consultor*: ¿Qué información necesita guardar sobre los productos?

*Cliente*: Cada producto tiene un nombre, una descripción, el precio por kilogramo y la cantidad disponible en inventario. También me gustaría registrar la fecha en que se reciben nuevos lotes para tener un historial.

*Consultor*: ¿Qué datos necesita de sus clientes?

*Cliente*: Los clientes son mayoristas, así que me interesa guardar su nombre, el nombre de la empresa, teléfono, correo y dirección. Además, algunos clientes tienen precios preferenciales que me gustaría reflejar en el sistema.

*Consultor*: ¿Cómo se gestionan los pedidos?

*Cliente*: Los clientes hacen pedidos por teléfono o correo, y nosotros los preparamos y los enviamos. Necesitamos registrar la fecha del pedido, la fecha de entrega y el estado (pendiente, en preparación, entregado). También sería útil saber si el pedido ya fue pagado.

*Consultor*: ¿Qué métodos de pago usan?

*Cliente*: Aceptamos transferencias, pagos con tarjeta y efectivo. Sería bueno que el sistema registre el método de pago y si hay un saldo pendiente.

*Consultor*: ¿Qué reportes le serían útiles?

*Cliente*: Quiero un reporte de ventas por mes, los productos más vendidos, el estado del inventario y un listado de los clientes con mayor volumen de compras.

*Consultor*: ¿Algo más que necesite incluir en el sistema?

*Cliente*: Sí, sería ideal que el sistema me avise cuando el inventario de un producto esté por debajo de cierto nivel. Así podemos pedir más a tiempo.

---

**Resumen del Caso:**

Este sistema necesitaría las siguientes tablas principales:
1. **Clientes**: Información básica de cada cliente mayorista.
2. **Productos**: Detalles de los mariscos (nombre, precio por kilogramo, cantidad en inventario, etc.).
3. **Pedidos**: Registro de pedidos con fecha, estado y cliente.
4. **Detalle de Pedidos**: Productos, cantidades y precios específicos por pedido.
5. **Pagos**: Métodos y estados de pago.
6. **Historial de Inventario**: Lotes nuevos y ajustes de inventario.

Con este diseño, se puede desarrollar un sistema para gestionar pedidos, clientes, inventarios y reportes.


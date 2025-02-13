**Caso: Sistema de Gestión de un Restaurante con Servicio a Domicilio**

**Entrevista con el cliente:**

*Cliente*: Hola, necesitamos un sistema para organizar los pedidos en nuestro restaurante. Ofrecemos servicio en el local, para llevar y a domicilio, y ya es complicado manejarlo todo manualmente.

*Consultor*: Entendido. Cuénteme cómo funciona su operación actualmente.

*Cliente*: Los clientes hacen pedidos en el restaurante o por teléfono. Registramos el pedido en papel, pero a veces se pierden o no se entiende bien lo que anotamos. Además, para los pedidos a domicilio, tenemos que asignar un repartidor, pero no llevamos un control claro.

*Consultor*: ¿Qué información necesitan registrar para los pedidos?

*Cliente*: Lo básico: los detalles del cliente, el tipo de pedido (en el local, para llevar o a domicilio), los platos que han solicitado, y el total a pagar. En el caso de los pedidos a domicilio, también necesitamos la dirección y el repartidor asignado.

*Consultor*: ¿Qué tipo de información necesitan sobre los clientes?

*Cliente*: Nombre, teléfono y dirección. A veces tenemos clientes frecuentes, y nos gustaría que el sistema los recuerde para que el proceso sea más rápido.

*Consultor*: ¿Y sobre los productos que venden?

*Cliente*: Tenemos un menú fijo, con platos organizados por categorías (entradas, platos fuertes, bebidas, postres). Cada plato tiene un precio, y algunos tienen opciones extra, como ingredientes adicionales o tamaños diferentes.

*Consultor*: ¿Cómo gestionan el pago de los pedidos?

*Cliente*: Lo hacemos al momento, ya sea en efectivo, tarjeta o transferencia. Sería útil registrar el método de pago y si el pedido ya fue pagado o no.

*Consultor*: Perfecto. ¿Hay algo más que considere importante incluir?

*Cliente*: Sí, me gustaría un reporte diario de los pedidos realizados, ingresos, los platos más vendidos y un control del estado de los pedidos (en preparación, listo, entregado).

---

Este caso es ideal para diseñar un sistema de base de datos con tablas como *Clientes*, *Pedidos*, *Platos*, *Categorías de Platos*, *Repartidores* y *Pagos*. También permite trabajar con relaciones entre pedidos, clientes y repartidores, además de gestionar estados de pedidos y generar reportes.


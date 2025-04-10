


![Azure](https://github.com/user-attachments/assets/e65bda34-e5f2-4435-be30-a85a601b5c5e)

Fecha: 10/abril
# 30 Prácticas Tematizadas de SQL Server en AWS para Estudiantes Universitarios

Todas las prácticas a continuación están diseñadas para un entorno **SQL Server Web Edition en AWS**, accesible mediante **SQL Server Management Studio (SSMS)** o **DataGrip**. Cada práctica está pensada para completarse en **dos sesiones de laboratorio de 1 hora**. Las actividades cubren tanto **consultas SQL** (SELECT, JOIN, subconsultas, *triggers*, etc.) como aspectos básicos de **administración de bases de datos** (creación de usuarios, respaldos, vistas, etc.), en contextos de escenarios **reales y atractivos**. Se incluyen **al menos 3 prácticas con TRIGGERS**. A continuación se presenta la lista de las 30 prácticas con su título, escenario y objetivos de aprendizaje específicos:

## Práctica 1: Consultas básicas en la base de datos de una Biblioteca
En esta práctica, los estudiantes trabajan con la base de datos de una **biblioteca pública** que almacena libros, autores y copias disponibles. El objetivo es familiarizarse con consultas simples SELECT para explorar los datos de una sola tabla. Por ejemplo, se pedirá listar todos los libros registrados o ver detalles básicos de los autores. Este escenario ayuda a entender cómo recuperar información fundamental del sistema bibliotecario de manera directa.  

**Objetivos de aprendizaje:**
- Practicar la sintaxis básica de `SELECT` y `FROM` para obtener todos los registros de una tabla.
- Utilizar la cláusula `WHERE` sencilla para filtrar resultados por un criterio (por ejemplo, libros de un cierto género o autor específico).
- Familiarizarse con el entorno de consulta en SSMS o DataGrip visualizando resultados de tablas completas.

## Práctica 2: Tienda en línea – Filtros y búsquedas específicas en consultas SQL
Esta práctica sitúa a los estudiantes en el contexto de una **tienda en línea**. Se proporciona una tabla de productos con información como nombre, categoría, precio y stock. El objetivo es aplicar filtros detallados en las consultas. Por ejemplo, los estudiantes deberán extraer los productos de cierta categoría, buscar productos cuyo nombre contiene ciertas palabras clave y limitar resultados por rango de precio.  
**Objetivos de aprendizaje:**
- Utilizar múltiples condiciones en `WHERE` (operadores `AND`, `OR`, comparaciones con `>`, `<`, `BETWEEN`) para refinar búsquedas de datos.
- Emplear el operador `LIKE` y comodines (`%`) para realizar búsquedas parciales en campos de texto (por ejemplo, productos que contienen "Pro" en el nombre).
- Ordenar los resultados con `ORDER BY` según un campo (por precio, alfabéticamente, etc.) para presentar la información de forma significativa al usuario.

## Práctica 3: Videoclub – Ordenamiento de resultados y valores únicos (ORDER BY y DISTINCT)
En este escenario, se utiliza la base de datos de un **videoclub** que registra películas y sus géneros. Los estudiantes realizarán consultas donde el enfoque es ordenar los resultados y obtener valores únicos. Por ejemplo, listar todas las películas **ordenadas** por año de estreno, o extraer la lista de **géneros únicos** disponibles en el catálogo. Esto permite explorar cómo presentar resultados de forma organizada.  
**Objetivos de aprendizaje:**
- Aplicar la cláusula `ORDER BY` para ordenar registros según diferentes criterios (por ejemplo, título de película ascendente o año de estreno descendente).
- Utilizar `SELECT DISTINCT` para obtener listas de valores únicos y evitar duplicados en los resultados (por ejemplo, géneros de película sin repetir).
- Combinar filtros (`WHERE`) con ordenamiento y distinct en una misma consulta, reforzando la construcción de consultas más complejas paso a paso.

## Práctica 4: Universidad – Consultas multi-tabla con JOIN (unión interna)
En esta práctica, los estudiantes se colocan en el contexto de una **base de datos universitaria** que incluye estudiantes, cursos y matrículas (inscripciones). Usarán **JOIN interno (INNER JOIN)** para combinar datos de dos tablas. Por ejemplo, obtener una lista de estudiantes con los cursos en los que están inscritos, combinando la tabla de Estudiantes con la de Inscripciones y Cursos. El escenario refleja consultas comunes para obtener información distribuida en varias tablas relacionadas.  
**Objetivos de aprendizaje:**
- Comprender la sintaxis de `INNER JOIN` para unir tablas mediante claves foráneas (por ejemplo, emparejar estudiantes con sus cursos mediante un ID común).
- Seleccionar campos de múltiples tablas en una sola consulta, utilizando alias de tabla para facilitar la escritura y lectura de la consulta.
- Ver cómo los JOIN internos devuelven solo registros coincidentes en ambas tablas (por ejemplo, solo estudiantes que **tienen** una inscripción registrada en un curso).

## Práctica 5: Hospital – Uso de JOIN externos (LEFT JOIN/RIGHT JOIN)
En esta práctica, el escenario es una **base de datos de hospital** con tablas de Doctores y Pacientes. Se enfoca en los **JOIN externos** para incluir registros aunque no tengan correspondencia en la otra tabla. Por ejemplo, listar todos los doctores y las consultas realizadas; si un doctor no ha atendido pacientes aún, igual debe aparecer en la lista (demostrando LEFT JOIN). De forma similar, explorar un RIGHT JOIN en otro caso si es pertinente.  
**Objetivos de aprendizaje:**
- Entender la diferencia entre `INNER JOIN` y *joins* externos como `LEFT JOIN` o `RIGHT JOIN`, viendo cómo estos últimos incluyen filas sin correspondencia de una u otra tabla.
- Construir consultas que identifiquen elementos "sin pareja" (por ejemplo, pacientes que no tienen cita programada o doctores sin pacientes asignados) gracias a los JOIN externos.
- Aplicar condiciones en combinación con JOIN externos (por ejemplo, filtrar por un campo de una tabla opcionalmente relacionada) asegurando comprender el resultado cuando hay valores nulos en campos de la tabla no coincidente.

## Práctica 6: Empresa (RRHH) – Self JOIN para relaciones jerárquicas (empleados y supervisores)
Este escenario presenta la base de datos de **Recursos Humanos de una empresa** donde todos los empleados, incluyendo gerentes, están en una sola tabla que referencia a sí misma para la relación jefe-subordinado. Los estudiantes practicarán un **self-join** (auto unión) para obtener, por ejemplo, cada empleado junto con el nombre de su supervisor (gerente). Este caso muestra cómo manejar relaciones jerárquicas dentro de una misma tabla usando JOINS.  
**Objetivos de aprendizaje:**
- Aplicar un `JOIN` de una tabla consigo misma (self join) utilizando alias distintos para representar cada rol (empleado versus supervisor) en la consulta.
- Extraer información jerárquica, como pares de “empleado - jefe”, a partir de datos autorreferenciados (campo de clave foránea que apunta a la misma tabla).
- Comprender cómo evitar confusiones en self joins mediante alias y condiciones adecuadas (por ejemplo, `ON emp.id_jefe = jefe.id_empleado`), y visualizar resultados que reflejen la estructura organizacional.

## Práctica 7: Tienda en línea – Consultas con múltiples tablas (JOIN múltiple)
En esta práctica, se vuelve al escenario de una **tienda en línea**, ahora abarcando varias tablas: Clientes, Pedidos, DetallesPedido, Productos, etc. El objetivo es construir consultas que involucren **más de dos tablas** en un mismo SELECT, combinando múltiples JOINs. Por ejemplo, obtener un informe de pedidos con el nombre del cliente (tabla Clientes), información del pedido (tabla Pedidos) y lista de productos en cada pedido (tabla DetallesPedido unida a Productos).  
**Objetivos de aprendizaje:**
- Elaborar consultas complejas que usan varios `JOIN` encadenados (e.j., `JOIN` entre Pedidos y Clientes, y luego otro `JOIN` con DetallesPedido, etc.) para reunir datos de 3 o más tablas.
- Utilizar eficientemente alias de tabla y calificar columnas con el nombre de tabla para evitar ambigüedades en consultas de múltiples uniones.
- Interpretar el resultado de una consulta multi-tabla, comprobando que la combinación de todos los JOIN muestra datos coherentes (por ejemplo, un pedido aparece tantas veces como productos distintos incluidos en él, replicando la información del cliente para cada línea).

## Práctica 8: Hospital – Consultas con agregación (GROUP BY y funciones de grupo)
Siguiendo con el escenario de un **hospital**, esta práctica se centra en obtener **estadísticas** a partir de los datos mediante agregación. Por ejemplo, calcular el número de pacientes por cada departamento, el promedio de días de internación de pacientes por médico, o el total de citas atendidas en cada especialidad. Los estudiantes emplearán `GROUP BY` junto con funciones como `COUNT()`, `AVG()`, `MIN()`, `MAX()` y `SUM()`.  
**Objetivos de aprendizaje:**
- Utilizar la cláusula `GROUP BY` para agrupar registros según un campo categórico (por ejemplo, departamento, médico, especialidad) antes de aplicar funciones de agregación.
- Aplicar funciones de agregado de SQL (`COUNT`, `AVG`, `SUM`, etc.) para resumir información numérica o contable de los grupos formados (por ejemplo, cantidad de pacientes asignados a cada doctor).
- Comprender la diferencia entre filtrar antes de agrupar (`WHERE`) y después de agrupar (`HAVING`, que se introducirá en la siguiente práctica), mediante ejemplos concretos en los que solo se incluyen ciertos registros en los cálculos.

## Práctica 9: Universidad – Agrupamiento con condición (HAVING) sobre conjuntos de datos
En el contexto de la **universidad**, esta práctica profundiza en el filtrado de grupos agregados usando `HAVING`. Siguiendo las estadísticas académicas, se pueden plantear consultas como: listar únicamente las carreras que tienen **más de 100 estudiantes inscritos**, o cursos que tengan una tasa de aprobación inferior al 50%. Aquí los estudiantes combinarán `GROUP BY` con `HAVING` para imponer condiciones sobre los resultados agregados.  
**Objetivos de aprendizaje:**
- Distinguir el uso de `HAVING` frente a `WHERE`: comprender que `HAVING` filtra **grupos completos** resultantes de un GROUP BY, basándose en condiciones sobre funciones agregadas (por ejemplo, `COUNT(alumnos) > 100`).
- Escribir consultas que obtengan indicadores agregados (como conteos o promedios) pero solo muestren aquellos grupos que cumplen criterios específicos de negocio o relevancia.
- Consolidar el uso combinado de `SELECT` con agregaciones, `GROUP BY` y `HAVING` en un mismo enunciado SQL, asegurándose de incluir en la selección solo columnas válidas (campos agrupados o agregados).

## Práctica 10: Banco – Subconsultas en sentencias SELECT (consultas anidadas básicas)
En esta práctica, el escenario es una **base de datos bancaria** con tablas de Clientes, Cuentas y Transacciones. El enfoque está en el uso de **subconsultas (subqueries)** para resolver consultas que requieren un resultado intermedio. Por ejemplo, encontrar los clientes cuyo saldo es mayor que el saldo promedio de todas las cuentas (lo que implica calcular el saldo promedio en una subconsulta), u obtener la lista de cuentas que tienen un saldo por encima de cierto cliente específico utilizando un SELECT interno.  
**Objetivos de aprendizaje:**
- Insertar subconsultas no correlacionadas en la cláusula `WHERE` para comparar valores con resultados derivados (e.g., `WHERE saldo > (SELECT AVG(saldo) FROM Cuentas)`).
- Utilizar subconsultas en la cláusula `FROM` o `SELECT` si aplica, para crear tablas derivadas o columnas calculadas dinámicamente a partir de otras consultas.
- Entender la diferencia entre una subconsulta que devuelve un solo valor escalar (para comparaciones =, >, <) y las que devuelven conjuntos de filas (para usar con operadores `IN`, `ANY` o `ALL`), aplicándolo en preguntas típicas del dominio bancario (por ejemplo, “clientes con cuentas en sucursales donde...”).

## Práctica 11: Empresa – Subconsultas correlacionadas y consultas anidadas avanzadas
Bajo un escenario empresarial (por ejemplo, el departamento de ventas de una **empresa**), se exploran **subconsultas correlacionadas**, donde la subconsulta depende de cada fila procesada por la consulta externa. Un caso clásico: encontrar empleados que ganan por encima del **promedio salarial de su propio departamento**. Aquí la subconsulta de promedio salarial se correlaciona con el departamento de cada empleado de la consulta externa. Se abordarán también operadores como `EXISTS` para verificar existencia de registros relacionados.  
**Objetivos de aprendizaje:**
- Escribir subconsultas **correlacionadas** dentro de un `WHERE` que referencien columnas de la consulta externa, por ejemplo usando alias (e.g., `WHERE salario > (SELECT AVG(salario) FROM Empleados dept WHERE dept.depto_id = emp.depto_id)`).
- Utilizar el operador `EXISTS` para probar la existencia de filas que cumplan cierta condición en una subconsulta, entendiendo su diferencia con `IN` en cuanto a eficiencia y legibilidad.
- Resolver requerimientos complejos de información combinando correctamente subconsultas y joins cuando corresponda, decidiendo cuál enfoque es más claro o eficiente para cada problema.

## Práctica 12: Tienda en línea – Operaciones de conjunto (UNION, INTERSECT, EXCEPT)
En el escenario de la **tienda en línea**, se pide combinar resultados de múltiples consultas usando operaciones de conjunto de SQL. Por ejemplo, el área de marketing desea una lista de *todos* los clientes que **han realizado una compra este año o el año pasado** (UNION de dos listas de clientes diferentes), identificar clientes **comunes** a dos campañas distintas (INTERSECT de dos conjuntos de clientes), o listar clientes que compraron el año pasado **pero no** este año (EXCEPT entre conjuntos). Se usará una base de datos de pedidos y clientes con fechas para filtrar por año.  
**Objetivos de aprendizaje:**
- Utilizar `UNION` para combinar resultados de dos o más `SELECT` (con el mismo formato de columnas) eliminando duplicados automáticamente, logrando conjuntos unidos (por ejemplo, union de clientes de distintos periodos).
- Aplicar `INTERSECT` para obtener la intersección de resultados de dos consultas, extrayendo solo los elementos comunes a ambos conjuntos (por ejemplo, clientes que aparecen en dos listas diferentes de actividad).
- Emplear `EXCEPT` (o `MINUS` en otros SQL) para restar conjuntos de resultados, filtrando elementos presentes en la primera consulta pero no en la segunda, entendiendo casos prácticos de este operador (como detectar clientes inactivos comparando periodos).

## Práctica 13: Biblioteca – Manipulación de datos (INSERT, UPDATE, DELETE) en el catálogo
Volviendo a la base de datos de la **biblioteca**, los estudiantes ahora realizarán operaciones de **manipulación de datos** (DML). Se les presentarán situaciones como: la biblioteca adquiere un **nuevo libro** (INSERT en la tabla Libros), se necesita **actualizar** la información de un libro (UPDATE, por ejemplo corregir el número de copias disponibles) o **eliminar** registros (DELETE, por ejemplo dar de baja un libro extraviado). Estas tareas les permitirán modificar los datos existentes siguiendo las reglas de integridad.  
**Objetivos de aprendizaje:**
- Escribir sentencias `INSERT` para agregar nuevos registros a una tabla, asegurándose de especificar correctamente las columnas y valores, y observando restricciones (como campos NOT NULL).
- Utilizar `UPDATE` con `WHERE` para modificar ciertos campos de uno o varios registros existentes (por ej., actualizar el stock de copias de un libro), teniendo cuidado de no afectar filas incorrectas.
- Emplear `DELETE` con condiciones para remover registros específicos sin eliminar datos inadvertidamente (por ej., borrar un libro por ID) y comprender el impacto de borrar datos que puedan tener relaciones (p.e., qué ocurre si existieran préstamos asociados a ese libro, destacando la importancia de la integridad referencial).

## Práctica 14: Banco – Transacciones y transferencia de fondos.
En el entorno del **banco**, esta práctica introduce el concepto de **transacción** para asegurar operaciones atómicas. Se plantea el ejemplo de una **transferencia bancaria** entre dos cuentas: se debe debitar un monto de una cuenta y acreditarlo en otra. Los estudiantes implementarán esta lógica usando comandos de transacción (`BEGIN TRAN`, `COMMIT`, `ROLLBACK`) de forma que ambas operaciones ocurran como una sola unidad (o se deshacen en caso de error). Se simularán casos como una interrupción que obliga a un rollback para proteger la integridad financiera.  
**Objetivos de aprendizaje:**
- Entender el concepto de **ACID** y atomicidad: utilizar **transacciones** para agrupar múltiples sentencias `UPDATE/INSERT` de modo que se confirmen todas o ninguna (all-or-nothing).
- Escribir secuencias de comandos con `BEGIN TRANSACTION`, seguidas de operaciones DML dependientes entre sí (por ejemplo, restar saldo en una cuenta y sumar en otra) y finalizar con `COMMIT` si todo fue bien o `ROLLBACK` si alguna condición falla.
- Experimentar con la consistencia: forzar un fallo intermedio (por ejemplo, simulando que una cuenta no existe o que no hay saldo suficiente) y observar cómo un ROLLBACK revierte cambios parciales, asegurando que los datos regresen al estado inicial si la transacción no puede completarse completamente.

## Práctica 15: Hotel – Diseño e implementación del esquema de base de datos (DDL)
En esta práctica, los estudiantes asumen el rol de diseñadores de base de datos para un **hotel**. A partir de una descripción (por ejemplo, el hotel necesita registrar sus Habitaciones, Huéspedes, Reservas, y Pagos), deben **diseñar el esquema relacional** y crearlo en SQL Server. Esto implica definir tablas con sus columnas, tipos de datos adecuados, claves primarias y foráneas. Tras el diseño lógico, los estudiantes implementarán el esquema usando sentencias de **DDL (Data Definition Language)** como `CREATE TABLE`.  
**Objetivos de aprendizaje:**
- Identificar entidades y relaciones clave en un escenario del mundo real (hotel), traduciendo requerimientos a un modelo de tablas relacionales (por ejemplo, tabla Habitación, Huésped, Reserva, cada una con su PK, y relaciones como Reserva tiene FK a Huésped y Habitación).
- Escribir sentencias `CREATE TABLE` para crear dichas tablas en SQL Server, eligiendo correctamente tipos de datos (INT, VARCHAR, DATE, etc.) y estableciendo **Primary Keys** para identificadores únicos.
- Definir **claves foráneas** (`FOREIGN KEY`) en las tablas para reforzar la integridad referencial (por ejemplo, que la reserva referencie a un huésped existente), y comprobar que el SGBD aplica estas restricciones al intentar cargar datos de prueba.

## Práctica 16: Hotel – Modificación del esquema y evolución de la base de datos (ALTER TABLE)
Continuando con la base de datos del **hotel** (ya creada en la práctica anterior), ahora se abordan cambios en el esquema simulando nuevos requerimientos. Por ejemplo, el hotel decide agregar un campo de “email” de contacto en la tabla de Huéspedes, o se necesita cambiar el tipo de dato de cierto campo (tal vez ampliar el tamaño de un VARCHAR para nombre). Los estudiantes usarán `ALTER TABLE` para **modificar tablas existentes** sin perder datos. Asimismo, podrían experimentar eliminando una columna obsoleta o renombrando una columna si el SGBD lo permite.  
**Objetivos de aprendizaje:**
- Utilizar la sentencia `ALTER TABLE` con opciones como `ADD COLUMN` para añadir nuevas columnas a una tabla existente, definiendo correctamente su tipo de dato y restricciones necesarias.
- Emplear `ALTER TABLE ... ALTER COLUMN` (o modificar estructura) para cambiar definiciones de columnas (ej.: ancho de un campo de texto, o permitir `NULL`/`NOT NULL` según nuevas necesidades).
- (Opcional) Practicar la eliminación de columnas o tablas con `DROP COLUMN` / `DROP TABLE` en un entorno controlado, entendiendo las consecuencias y la importancia de migrar o respaldar datos antes de realizar estos cambios en un entorno real.

## Práctica 17: Universidad – Reglas de integridad con restricciones CHECK y UNIQUE
En el contexto de la **universidad**, se revisa y refuerza la integridad de los datos mediante la creación de **restricciones adicionales** en las tablas. Por ejemplo, asegurar que la calificación de un estudiante en un curso esté entre 0 y 100 (restricción `CHECK`), o que no pueda haber dos estudiantes con el mismo correo electrónico (restricción de unicidad `UNIQUE`). Los estudiantes aplicarán estas restricciones a una base existente para prevenir datos inválidos.  
**Objetivos de aprendizaje:**
- Crear restricciones `CHECK` en columnas para validar reglas de negocio simples a nivel de base de datos (por ejemplo, `CHECK (calificacion BETWEEN 0 AND 100)` en la tabla de calificaciones, garantizando que los puntajes estén en el rango válido).
- Definir restricciones `UNIQUE` en uno o varios campos para evitar duplicados donde no deben ocurrir (por ejemplo, que el número de estudiante o el email sean únicos en la tabla Estudiantes).
- Observar cómo el SGBD impide la inserción o actualización de datos que violan estas restricciones, reforzando la importancia de la integridad a nivel de base de datos más allá de la lógica de la aplicación.

## Práctica 18: Empresa – Creación y uso de Vistas (VIEW) para reportes
En esta práctica, se utiliza la base de datos de una **empresa** (por ejemplo, una compañía con empleados, departamentos y nóminas) para ilustrar la creación de **vistas**. Se pide crear una vista que reúna o filtre información para facilitar reportes o mejorar la seguridad. Por ejemplo, una vista denominada `vw_EmpleadosPublico` que muestre para cada empleado solo su nombre, departamento y puesto, **ocultando datos sensibles** como salario. Los estudiantes crearán la vista y la consultarán como si fuera una tabla.  
**Objetivos de aprendizaje:**
- Escribir sentencias `CREATE VIEW` para definir vistas basadas en consultas SELECT que combinan o filtran datos de una o más tablas (por ej., creando una vista que une Empleados con Departamento para un reporte sencillo).
- Comprender cómo las vistas actúan como tablas virtuales: consultar una vista con `SELECT` igual que una tabla normal, y verificar que refleja en tiempo real los datos subyacentes.
- Reconocer la utilidad de las vistas en escenarios reales: simplificar consultas complejas predefiniéndolas como vista, y/o limitar el acceso a ciertas columnas sensibles para diferentes tipos de usuarios (seguridad a nivel de columna mediante vistas).

## Práctica 19: Universidad – Procedimiento almacenado para gestión de inscripciones
En el escenario universitario, se abordará la creación de un **procedimiento almacenado (Stored Procedure)**. Por ejemplo, un procedimiento `usp_InscribirEstudiante` que reciba como parámetros el ID de estudiante y el ID del curso, y ejecute la lógica para inscribir al estudiante en dicho curso. Esto podría implicar validar que haya cupo disponible en el curso, insertar un registro en la tabla de Inscripciones, y disminuir el cupo disponible en el curso. Los estudiantes implementarán este SP y lo probarán con diferentes entradas.  
**Objetivos de aprendizaje:**
- Escribir un `CREATE PROCEDURE` definiendo parámetros de entrada (y salida si aplicara) y un bloque de instrucciones T-SQL entre `BEGIN...END` que ejecuta múltiples pasos lógicos.
- Dentro del procedimiento, utilizar instrucciones de control de flujo básicas (por ejemplo, `IF... ELSE`) para verificar condiciones previas (como disponibilidad de cupo) y decidir si procede la inscripción o se aborta.
- Llamar o **ejecutar el procedimiento** (`EXEC usp_InscribirEstudiante ...`) con distintos valores, comprobando que realiza las acciones en la base de datos (inserta la inscripción, actualiza cupo) correctamente; manejar también situaciones de error o mensajes, ilustrando cómo los SP encapsulan lógica de negocio en el servidor.

## Práctica 20: Universidad – Función definida por el usuario para cálculo de calificación
También en el entorno de la **universidad**, se explorará la creación de una **función definida por el usuario (UDF)**. Un ejemplo concreto: una función `fn_CalificacionEnLetra` que acepte un número (la calificación de un estudiante de 0 a 100) y retorne una cadena con la calificación en letra (A, B, C, etc., o "Aprobado"/"Reprobado"). Los estudiantes crearán la función y la emplearán dentro de una consulta para mostrar las calificaciones tanto numéricas como en letra.  
**Objetivos de aprendizaje:**
- Escribir `CREATE FUNCTION` para definir una función escalares de usuario, especificando parámetros de entrada y tipo de dato de retorno, con un cuerpo de calculo lógico (por ejemplo, usar `CASE` dentro de la función para convertir 90-100 -> "A", 80-89 -> "B", etc.).
- Diferenciar entre procedimientos almacenados y funciones: las funciones devuelven un valor (o tabla) y se pueden usar dentro de una consulta SELECT, mientras los SP son rutinas ejecutables independientes.
- Llamar la nueva función dentro de sentencias SQL (por ejemplo, `SELECT nombre, calificacion, dbo.fn_CalificacionEnLetra(calificacion) FROM Resultados`) para verificar su correcto funcionamiento e ilustrar cómo extiende las capacidades del SQL con lógica personalizada reutilizable.

## Práctica 21: Banco – Trigger de auditoría de transacciones financieras
Volviendo al escenario del **banco**, esta práctica introduce los **TRIGGERS** en SQL Server para auditoría. Se creará un trigger que, por ejemplo, dispare **después de insertar** (`AFTER INSERT`) una nueva transacción en la tabla Transacciones, y registre automáticamente un evento en una tabla de Auditoría. La tabla de auditoría podría guardar el ID de la cuenta, el monto movido y la fecha/hora, para tener un historial de movimientos críticos. Los estudiantes implementarán este disparador y probarán que al insertar una transacción, el trigger agrega correctamente el registro de auditoría.  
**Objetivos de aprendizaje:**
- Comprender la sintaxis para crear un trigger (`CREATE TRIGGER`) asociado a una tabla y un evento específico (INSERT, UPDATE o DELETE), definiendo el bloque de código T-SQL que debe ejecutarse automáticamente tras dicho evento.
- Implementar un trigger de auditoría que inserta en una tabla log los detalles relevantes cada vez que ocurre una transacción, usando las tablas virtuales `INSERTED` (y/o `DELETED` si fuera el caso) para obtener los valores nuevos o antiguos.
- Verificar el funcionamiento del trigger realizando operaciones que lo disparen (ejecutando un `INSERT` de prueba en Transacciones) y luego consultando la tabla de auditoría para confirmar que la acción fue registrada. Se refuerza así la utilidad de los triggers para mantener historiales o bitácoras de cambios automáticamente.

## Práctica 22: Tienda en línea – Trigger para mantener consistencia de inventario
En el contexto de la **tienda en línea**, se aplicará un **trigger** como mecanismo de negocio. El caso práctico: asegurar la consistencia del inventario de productos cuando se insertan detalles de un pedido. Se desarrollará un trigger `AFTER INSERT` en la tabla DetallesPedido que, tras agregar un artículo vendido, automáticamente **descuente** esa cantidad del stock en la tabla Productos. Adicionalmente, podría implementarse lógica para evitar que la venta ocurra si no hay suficiente stock (usando un trigger `INSTEAD OF` o validando dentro del trigger y haciendo ROLLBACK).  
**Objetivos de aprendizaje:**
- Construir un trigger que realiza **actualizaciones en cascada manuales**: tras una inserción en DetallesPedido, leer la cantidad y producto insertados (desde la tabla virtual `INSERTED`) y ejecutar un `UPDATE` en la tabla de Productos para disminuir el inventario.
- (Opcional) Incluir lógica de validación dentro del trigger para **prevenir inconsistencias**, por ejemplo comprobando que el stock nunca baje por debajo de 0; en caso de violación, emitir un error (`RAISERROR`) y revertir la transacción automáticamente, demostrando cómo un trigger puede abortar una operación inválida.
- Probar escenarios de venta con stock suficiente y con stock insuficiente, observando cómo el trigger mantiene los datos coherentes (actualiza existencias o bloquea la operación), resaltando el papel de los triggers en implementar reglas de negocio automáticas.

## Práctica 23: Universidad – Trigger para cálculo automático de promedio de estudiante
En la base de datos de la **universidad**, se utilizará un **trigger** para automatizar cálculos derivados. Por ejemplo, cada vez que se inserta o actualiza una calificación de un estudiante en un curso (en una tabla de Calificaciones), un trigger `AFTER INSERT/UPDATE` recalculará el **promedio general** de ese estudiante en la tabla de Estudiantes. Así, el campo "promedio" del estudiante se mantiene actualizado sin intervención manual. Los estudiantes crearán este trigger y verificarán que al cambiar o añadir calificaciones, el promedio del alumno se ajusta correctamente.  
**Objetivos de aprendizaje:**
- Desarrollar un trigger que responda a eventos `UPDATE` o `INSERT` en una tabla de detalles (Calificaciones) para efectuar un cálculo agregado y realizar un `UPDATE` correspondiente en la tabla maestra (Estudiantes).
- Utilizar las tablas virtuales `INSERTED` (y `DELETED` si fuera necesario en caso de UPDATE) para obtener los valores necesarios para el cálculo (por ejemplo, identificar qué estudiante tuvo una nueva calificación y qué valor se agregó) y luego usar una consulta agregada dentro del trigger para recalcular el promedio de ese estudiante.
- Evaluar el impacto de múltiples operaciones: probar inserciones o modificaciones en calificaciones de distintos alumnos y verificar que el trigger solo afecta al promedio del estudiante pertinente. Esto enfatiza buenas prácticas como limitar la lógica del trigger al cambio específico y evitar recalcular más de lo necesario.

## Práctica 24: Hospital – Creación de usuarios y control de permisos de acceso
En esta práctica de administración, el escenario es un **hospital** que quiere otorgar acceso limitado a su base de datos a distintos tipos de usuarios. Se pedirá a los estudiantes crear un **nuevo usuario** de base de datos (login y usuario) con privilegios restringidos. Por ejemplo, crear un usuario `reportes_medicos` que solo pueda **seleccionar** datos de ciertas vistas o tablas (por ejemplo, ver información de pacientes, pero no modificarla ni ver datos sensibles como diagnósticos confidenciales). Los estudiantes configurarán estos permisos usando comandos DCL (Data Control Language) o las herramientas gráficas.  
**Objetivos de aprendizaje:**
- Utilizar `CREATE LOGIN` (si aplica a nivel servidor) y `CREATE USER` (a nivel base de datos) para registrar un nuevo usuario que pueda conectarse a la base de datos SQL Server.
- Aplicar sentencias `GRANT`, `REVOKE` (y/o roles predefinidos) para otorgar permisos específicos a ese usuario – por ejemplo, `GRANT SELECT ON dbo.Pacientes TO reportes_medicos`, limitando otras operaciones como INSERT/UPDATE.
- Probar la configuración iniciando sesión como el nuevo usuario (usando SSMS/DataGrip con esas credenciales) e intentando realizar diferentes operaciones, confirmando que solo puede acceder a lo autorizado. Esto refuerza la importancia de la seguridad y la administración de usuarios en entornos multiusuario.

## Práctica 25: Hospital – Respaldo (backup) completo de la base de datos
Siguiendo con el **hospital**, se aborda ahora la **copia de seguridad** de la base de datos, una tarea crítica de administración. Los estudiantes realizarán un **backup completo** de la base de datos hospitalaria al final de la jornada. Usarán las herramientas de SQL Server (T-SQL o asistente gráfico en SSMS) para generar un archivo de backup (.bak). Se discutirán consideraciones como el destino del archivo (en AWS posiblemente un almacenamiento adjunto) y la verificación del éxito del respaldo.  
**Objetivos de aprendizaje:**
- Ejecutar la instrucción T-SQL de backup básica: `BACKUP DATABASE NombreBD TO DISK = 'archivo.bak'` con las opciones apropiadas, o realizar el equivalente mediante la interfaz de SSMS, comprendiendo cada parámetro esencial (nombre de medio, copia completa vs diferencial en contextos más amplios, etc.).
- Asegurar que el backup se realiza sin errores y **verificar** su existencia e integridad básica (por ejemplo, confirmando el tamaño del archivo, u obteniendo la lista de conjuntos de copia de seguridad con `RESTORE HEADERONLY`).
- Entender la importancia de los respaldos programados en un entorno de producción (aunque la práctica es manual, se comentará cómo se automatizaría en AWS/SQL Server Agent) y las implicaciones de almacenar backups de forma segura (p. ej., en AWS S3 o en medios externos) para protección ante fallos.

## Práctica 26: Hospital – Restauración de la base de datos desde un respaldo
Complementando la práctica anterior, ahora se simula que ocurrió un problema y se necesita **restaurar** la base de datos del **hospital** utilizando el respaldo realizado. Los estudiantes usarán el comando de **RESTORE** para recuperar la base de datos a partir del archivo .bak. Esta actividad puede realizarse restaurando sobre una base nueva de prueba para no sobrescribir la original. Se enfatizará el proceso completo de recuperación y la comprobación de que los datos quedan intactos tras la restauración.  
**Objetivos de aprendizaje:**
- Emplear la instrucción `RESTORE DATABASE` indicando el origen del backup y el nombre de la base de datos destino, junto con opciones como `WITH MOVE` si es necesario redirigir archivos (en caso de restaurar en otro servidor o nombre de base de datos).
- Practicar la restauración en un entorno seguro, por ejemplo creando primero una base de datos vacía de prueba o restaurando con otro nombre, para luego validar que las tablas y datos coinciden con el estado respaldado.
- Comprender conceptos como el estado de la base durante la restauración (por ejemplo, uso de `WITH NORECOVERY` para cadenas de backups, aunque en este caso sea solo uno completo), y verificar la funcionalidad de la base restaurada haciendo algunas consultas básicas post-restauración.

## Práctica 27: Tienda en línea – Mejora del rendimiento con índices
En el escenario de la **tienda en línea**, se introducen los **índices** como mecanismo para optimizar consultas. Se proporciona un caso donde una consulta (por ejemplo, buscar pedidos por fecha o productos por nombre) es lenta debido a que la tabla tiene muchos registros. Los estudiantes identificarán el campo apropiado para indexar (fecha de pedido, o nombre de producto) y crearán un índice no clustered sobre él. Luego repetirán la consulta para evaluar la mejora. Esta práctica puede incluir el uso del plan de ejecución para visualizar el impacto del índice.  
**Objetivos de aprendizaje:**
- Entender la sintaxis básica `CREATE INDEX` sobre una tabla y columna(s) y crear un índice no cluster en un campo frecuentemente filtrado (e.g., `CREATE INDEX idx_FechaPedido ON Pedidos(Fecha)`).
- Medir o apreciar la diferencia en rendimiento de una consulta antes y después de tener el índice: por ejemplo, utilizando la opción de mostrar el **plan de ejecución** en SSMS o las estadísticas de tiempo e IO (`SET STATISTICS IO/TIME ON`) para ver la reducción de costos.
- Discutir brevemente los trade-offs de los índices (mejora de velocidad de lectura vs. costo en escrituras y almacenamiento) y la importancia de elegir correctamente qué columnas indexar. Aun si la medición precisa es compleja en el laboratorio, los estudiantes deben verificar que consultas que antes hacían *table scan* ahora usan *index seek* en el plan de ejecución tras la creación del índice.

## Práctica 28: Empresa – Uso de CASE para generar informes categorizados
En esta práctica, con datos de una **empresa**, los estudiantes crearán consultas que incorporan expresiones condicionales usando `CASE`. Por ejemplo, a partir de una tabla Empleados con sus salarios, generar un informe que añada una columna categorizando a cada empleado en “Alto”, “Medio” o “Bajo” ingreso según rangos de salario. Otro ejemplo podría ser marcar una venta como "Rentable" o "No rentable" dependiendo de su margen. El objetivo es producir salidas más interpretativas directamente desde SQL.  
**Objetivos de aprendizaje:**
- Aprender a usar la expresión `CASE WHEN ... THEN ... ELSE ... END` dentro de un SELECT para **categorizar o etiquetar** filas en función de condiciones lógicas sobre los datos (por ejemplo, `CASE WHEN salario > 2000 THEN 'Alta' WHEN salario > 1000 THEN 'Media' ELSE 'Baja' END`).
- Incorporar múltiples condiciones en un CASE para cubrir rangos o casos mutuamente excluyentes, asegurándose de manejar también el caso *ELSE* por defecto.
- Generar consultas tipo reporte donde los resultados numéricos se acompañan de interpretaciones legibles. Tras esta práctica, el estudiante podrá producir, por ejemplo, una lista de empleados con su sueldo y una etiqueta de nivel salarial, todo en la misma consulta SQL sin necesidad de posprocesamiento en la aplicación.

## Práctica 29: Biblioteca – Cálculos de tiempo con funciones de fecha (días de atraso en préstamos)
En la base de datos de la **biblioteca**, se abordarán las funciones de fecha para cálculos prácticos. El escenario propuesto: la biblioteca quiere calcular cuántos días de atraso tienen los préstamos de libros vencidos. Cada préstamo tiene una fecha de vencimiento y una fecha de devolución (o null si no ha sido devuelto). Los estudiantes usarán funciones como `DATEDIFF` para obtener la diferencia en días entre la fecha de devolución (o la fecha actual si sigue prestado) y la fecha de vencimiento. Incluso podrían calcular una multa estimada multiplicando días de atraso por una tarifa diaria.  
**Objetivos de aprendizaje:**
- Utilizar funciones de fecha y hora de SQL Server, especialmente `DATEDIFF`, para calcular intervalos (por ejemplo, días de diferencia entre dos fechas).
- Combinar lógica condicional y funciones de fecha, por ejemplo usando `ISNULL` o `COALESCE` para asumir la fecha actual cuando una devolución aún no ha ocurrido, de modo que también se calcule el atraso de préstamos activos no devueltos.
- Aplicar estos cálculos en consultas que presentan información útil, como una lista de préstamos con el número de días atrasados y posiblemente un monto de multa calculado (`CASE` o multiplicación de días * tarifa). Los estudiantes comprenderán cómo el servidor puede realizar cálculos temporales masivos sin necesitar exportar los datos a Excel u otro medio.

## Práctica 30: Empresa – Automatización de procesamiento fila por fila con CURSOR
En esta práctica avanzada, usando datos de una **empresa**, se introduce el concepto de **cursor** en T-SQL para procesamiento fila por fila. Aunque no es la forma más eficiente generalmente, sirve para comprender cómo iterar sobre conjuntos de resultados cuando se requiere una lógica secuencial. El ejemplo: la empresa quiere asignar un **bono personalizado** a cada empleado según criterios complejos (no fácilmente logrables con una sola consulta). Se usará un cursor para recorrer cada empleado, calcular su bono (quizá usando condiciones dentro del bucle), y actualizar su registro con el bono calculado.  
**Objetivos de aprendizaje:**
- Escribir la sintaxis de declaración de un cursor (`DECLARE cursor_name CURSOR FOR SELECT ...`), y luego los pasos de `OPEN`, `FETCH NEXT`, procesamiento dentro de un bucle `WHILE`, y finalmente `CLOSE` y `DEALLOCATE`.
- Implementar lógica procedural dentro del bucle cursor: por ejemplo, para cada empleado obtenido del cursor, calcular un valor derivado (el bono) en base a condiciones o consultas adicionales, demostrando cómo se puede hacer tratamiento fila a fila cuando es necesario.
- Entender las consideraciones de rendimiento: si bien se practica el uso de cursores, también se recalca que, de ser posible, es preferible una solución set-based. Sin embargo, el estudiante aprende cómo manejar casos en que un cursor es válido y cómo asegurar que se liberen adecuadamente los recursos tras su uso.

Trabajar

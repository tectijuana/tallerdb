Quintero Angulo Roberto Carlos 22210784

```markdown
## Usando `INSERT IGNORE` para evitar duplicados en MySQL

El comando `INSERT IGNORE` en MySQL se utiliza para insertar datos en una tabla, pero si ya existe una fila con una clave primaria o un índice único que cause un conflicto, MySQL simplemente ignora esa fila y no genera un error.

### ¿Para qué sirve?

- **Evitar errores**: Si intentas insertar una fila con valores duplicados en una columna que tiene una restricción de unicidad (como una clave primaria o un índice único), con `INSERT IGNORE` no se genera un error, y la inserción simplemente se omite.
  
- **Evitar duplicados**: Permite insertar varios registros sin preocuparse por si ya existen registros duplicados, ya que aquellos que ya están presentes no se duplicarán.

### Ejemplo:

Imagina que tienes una tabla llamada `usuarios` con los siguientes campos: `id` (clave primaria) y `email` (único).

```sql
CREATE TABLE usuarios (
    id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(100) UNIQUE,
    nombre VARCHAR(100)
);
```

Si intentas insertar dos veces el mismo correo electrónico, normalmente obtendrás un error. Pero si usas `INSERT IGNORE`, la segunda inserción con el mismo correo electrónico se ignorará sin generar un error.

```sql
-- Inserción 1
INSERT INTO usuarios (email, nombre) VALUES ('juan@ejemplo.com', 'Juan Pérez');

-- Inserción 2 (se omite debido a la restricción UNIQUE en el campo email)
INSERT IGNORE INTO usuarios (email, nombre) VALUES ('juan@ejemplo.com', 'Juan Pérez');
```

En este caso, la primera inserción será exitosa. La segunda será ignorada porque el correo electrónico `'juan@ejemplo.com'` ya existe en la tabla, pero no provocará ningún error.

### ¿Cuándo usarlo?

- Cuando estás insertando datos desde una fuente externa y no quieres que los duplicados provoquen fallos.
- Para evitar insertar registros duplicados sin tener que escribir lógica adicional para verificar si ya existen.

```

Este formato te permitirá mostrar el contenido de manera estructurada y clara. Si necesitas más detalles o ejemplos, no dudes en preguntar.

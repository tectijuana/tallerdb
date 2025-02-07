#Perez Lopez Oscar Ricardo 23210645
# Manejo de Claves Foráneas con `FOREIGN KEY`

Las claves foráneas (`FOREIGN KEY`) se utilizan en SQL para mantener la integridad referencial entre dos tablas, asegurando que los valores de una columna en una tabla existan en una columna de otra tabla.

---

## 📌 Creación de una Clave Foránea

Para crear una clave foránea en una tabla, se utiliza la cláusula `FOREIGN KEY` al definir la estructura de la tabla.

### 📍 Sintaxis:
```sql
CREATE TABLE detalle_pedido (
    id_detalle INT PRIMARY KEY,
    id_pedido INT,
    producto VARCHAR(100),
    cantidad INT,
    FOREIGN KEY (id_pedido) REFERENCES pedidos(id_pedido)
);
```
En este caso:
- `id_pedido` en la tabla `detalle_pedido` es una clave foránea.
- Se referencia a la columna `id_pedido` en la tabla `pedidos`.

---

## 🔗 Reglas de Integridad Referencial

Las claves foráneas pueden configurarse con diferentes reglas para manejar cambios en los datos referenciados.

### 📍 `ON DELETE`
Define qué sucede cuando se elimina una fila en la tabla padre.

- **`CASCADE`**: Elimina también las filas relacionadas en la tabla hija.
- **`SET NULL`**: Establece la clave foránea en `NULL` si la clave primaria es eliminada.
- **`NO ACTION`** / **`RESTRICT`**: Impide la eliminación si existen registros relacionados.

### 📍 `ON UPDATE`
Determina qué sucede cuando la clave primaria es actualizada.

- **`CASCADE`**: Propaga los cambios a la tabla hija.
- **`SET NULL`**: Establece la clave foránea en `NULL` si la clave primaria cambia.
- **`NO ACTION`** / **`RESTRICT`**: Impide la actualización si existen registros relacionados.

### 📍 Ejemplo Completo:
```sql
CREATE TABLE detalle_pedido (
    id_detalle INT PRIMARY KEY,
    id_pedido INT,
    producto VARCHAR(100),
    cantidad INT,
    FOREIGN KEY (id_pedido) REFERENCES pedidos(id_pedido)
    ON DELETE CASCADE
    ON UPDATE CASCADE
);
```
En este caso:
- Si se elimina un `id_pedido` de `pedidos`, los registros correspondientes en `detalle_pedido` también serán eliminados.
- Si se actualiza un `id_pedido`, los cambios se propagarán a `detalle_pedido`.

---

## 🛠️ Agregar una Clave Foránea a una Tabla Existente

Si una tabla ya fue creada sin la clave foránea, se puede agregar con `ALTER TABLE`:

```sql
ALTER TABLE detalle_pedido
ADD CONSTRAINT fk_pedido FOREIGN KEY (id_pedido)
REFERENCES pedidos(id_pedido)
ON DELETE SET NULL
ON UPDATE CASCADE;
```
Aquí:
- Si un `id_pedido` en `pedidos` es eliminado, `id_pedido` en `detalle_pedido` se establecerá en `NULL`.
- Si un `id_pedido` cambia, los cambios se propagarán automáticamente.

---

## 🚨 Consideraciones Importantes
✔️ La columna referenciada debe ser una **clave primaria** o tener un **índice único**.  
✔️ Los tipos de datos de ambas columnas (clave foránea y referenciada) deben coincidir.  
✔️ Las claves foráneas ayudan a **mantener la integridad de los datos**, pero pueden afectar el rendimiento en bases de datos grandes.  

---

## 📚 Referencias
Para más información, puedes consultar los manuales oficiales de:
- [MySQL Foreign Keys](https://dev.mysql.com/doc/refman/8.0/en/create-table-foreign-keys.html)
- [SQL Server Foreign Keys](https://docs.microsoft.com/en-us/sql/relational-databases/tables/create-foreign-key-relationships)

# ANGEL ELOY LANDGRAVE REZA

## 💡 Usa OFFSET para paginación

La paginación en SQL se puede lograr utilizando la cláusula `OFFSET` junto con `FETCH` o `LIMIT`, dependiendo del motor de base de datos que estés utilizando.

### **Paginación en SQL Server**
Desde SQL Server 2012, la forma más eficiente de paginar datos es con `OFFSET` y `FETCH`:

```sql
SELECT OrderId, ProductId
FROM OrderDetail
ORDER BY OrderId
OFFSET (@PageNumber - 1) * @RowsPerPage ROWS
FETCH NEXT @RowsPerPage ROWS ONLY;
```

- `OFFSET N ROWS`: Salta los primeros `N` registros.
- `FETCH NEXT M ROWS ONLY`: Devuelve los siguientes `M` registros después del `OFFSET`.
- **Es obligatorio usar `ORDER BY`**, ya que define el orden de paginación.

Ejemplo práctico para la página 4, mostrando 10 registros por página:

```sql
DECLARE @RowsPerPage INT = 10, @PageNumber INT = 4;
SELECT OrderId, ProductId
FROM OrderDetail
ORDER BY OrderId
OFFSET (@PageNumber - 1) * @RowsPerPage ROWS
FETCH NEXT @RowsPerPage ROWS ONLY;
```

---

### **Paginación en MySQL**
En MySQL, `LIMIT` se usa junto con `OFFSET`:

```sql
SELECT * FROM users ORDER BY id ASC LIMIT 10 OFFSET 20;
```

- `LIMIT 10 OFFSET 20` significa: saltar los primeros 20 registros y mostrar los siguientes 10.
- Alternativamente, se puede escribir como `LIMIT 20, 10`.

Ejemplo práctico para la página 3, mostrando 5 registros por página:

```sql
SELECT * FROM users ORDER BY id ASC LIMIT 5 OFFSET 10;
```

Este ejemplo ignora los primeros 10 registros (página 1 y 2) y devuelve los siguientes 5 registros de la página 3.

---

### **Otras bases de datos**
- **PostgreSQL**: Usa `LIMIT` y `OFFSET` igual que MySQL.
- **Oracle**: Se usa `ROW_NUMBER()` con `BETWEEN` en una subconsulta.
- **ANSI SQL**: `FETCH FIRST N ROWS ONLY` también es una alternativa aceptada en algunos motores.

Si usas una versión antigua de SQL Server (< 2012), se recomienda `ROW_NUMBER()` para la paginación.

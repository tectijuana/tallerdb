# JOSE JUAREZ CASTELAN 
 

# 📌 Tip: Limita filas con LIMIT o FETCH FIRST en SQL

Cuando trabajamos con bases de datos relacionales, a menudo es necesario restringir la cantidad de filas que se devuelven en una consulta. Para esto, podemos utilizar `LIMIT` o `FETCH FIRST`, dependiendo del sistema de gestión de bases de datos (DBMS) que estemos usando.

## 🎯 Uso de `LIMIT`
`LIMIT` se usa en **MySQL**, **PostgreSQL** y algunos otros sistemas para especificar el número máximo de filas que se devolverán.

### 🔹 Ejemplo en MySQL/PostgreSQL:
```sql
SELECT * FROM productos
ORDER BY precio DESC
LIMIT 10;
```
📌 **Explicación:** Devuelve los 10 productos más caros.

Además, `LIMIT` se puede combinar con `OFFSET` para paginación:
```sql
SELECT * FROM productos
ORDER BY precio DESC
LIMIT 10 OFFSET 20;
```
📌 **Explicación:** Salta las primeras 20 filas y devuelve las siguientes 10.

---

## 🎯 Uso de `FETCH FIRST`
`FETCH FIRST` es una alternativa utilizada en **SQL Server**, **DB2** y **Oracle**.

### 🔹 Ejemplo en SQL Server:
```sql
SELECT * FROM productos
ORDER BY precio DESC
FETCH FIRST 10 ROWS ONLY;
```
📌 **Explicación:** Devuelve las 10 filas con los precios más altos.

También se puede usar con `OFFSET`:
```sql
SELECT * FROM productos
ORDER BY precio DESC
OFFSET 20 ROWS FETCH NEXT 10 ROWS ONLY;
```
📌 **Explicación:** Salta las primeras 20 filas y devuelve las siguientes 10.

---

## 🚀 ¿Cuándo usar cada uno?
- ✅ **Usa `LIMIT`** en **MySQL** y **PostgreSQL**.
- ✅ **Usa `FETCH FIRST`** en **SQL Server**, **DB2** y **Oracle**.
- ✅ **Usa `OFFSET`** para paginación en ambas opciones.

⚡ **Consejo adicional:** Si la consulta es muy grande, usa índices para optimizar el rendimiento al paginar grandes volúmenes de datos.

---

💡 **Ahora ya sabes cómo limitar filas en SQL! 🚀**
```

Este formato **Markdown** es ideal para documentaciones, wikis o publicaciones en foros y blogs técnicos. ¿Necesitas algún ajuste? 😊

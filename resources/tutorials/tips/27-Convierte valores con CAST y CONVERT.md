### **Conversión de Datos con CAST y CONVERT**  
**America Martinez Perez - 21211991**  

En SQL Server, las funciones `CAST` y `CONVERT` se utilizan para convertir datos de un tipo a otro. Aquí te explico cómo funcionan:  

---

### **CAST**  
La función `CAST` es una función estándar de ANSI SQL y se utiliza para convertir un valor de un tipo de dato a otro.  

#### **Sintaxis:**  
```sql
CAST(expresión AS tipo_de_dato)
```

#### **Ejemplo:**  
```sql
SELECT CAST(123.45 AS INT) AS Entero;
```
Esto devolverá `123`, ya que el número decimal se convierte en un entero.  

Otro ejemplo con fechas:  
```sql
SELECT CAST(GETDATE() AS VARCHAR(20)) AS FechaComoTexto;
```
Esto convierte la fecha actual a un tipo de dato `VARCHAR`.  

---

### **CONVERT**  
`CONVERT` es una función específica de SQL Server que, además de convertir datos, permite especificar estilos de formato cuando se trabaja con tipos `datetime`.  

#### **Sintaxis:**  
```sql
CONVERT(tipo_de_dato, expresión [, estilo])
```

#### **Ejemplo de conversión de fecha:**  
```sql
SELECT CONVERT(VARCHAR, GETDATE(), 101) AS FechaFormatoMMDDYYYY;
```
Esto devuelve la fecha en formato `MM/DD/YYYY`.  

#### **Otro ejemplo con números:**  
```sql
SELECT CONVERT(VARCHAR, 1234.56, 1) AS NumeroComoTexto;
```
Aquí, el número se convierte en texto con un formato específico.  

---

### **Diferencias Clave**  
| Función  | Estándar SQL | Personalización de formato |
|----------|-------------|--------------------------|
| `CAST`   | Sí          | No                       |
| `CONVERT` | No (SQL Server) | Sí (solo en ciertos tipos) |

En SQL Server, `CAST` es preferido cuando no se requiere formato específico, mientras que `CONVERT` es útil cuando se necesita un formato personalizado para fechas o números.

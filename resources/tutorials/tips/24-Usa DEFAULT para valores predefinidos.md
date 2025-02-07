## López Félix Nicolás
## Tip: Usa `DEFAULT` para valores predefinidos en SQL

Cuando diseñes una base de datos, asignar valores predeterminados a tus columnas puede hacer tu aplicación más robusta y facilitar la gestión de datos.

### ¿Por qué usar `DEFAULT`?
- **Estandarización:** Evita valores nulos no deseados en columnas específicas.
- **Simplicidad:** Reduce la necesidad de manejar valores nulos en el código de la aplicación.
- **Consistencia:** Proporciona valores coherentes sin necesidad de especificarlos manualmente en cada operación `INSERT`.

### Ejemplo de Uso
```sql
CREATE TABLE empleados (
  id INT PRIMARY KEY,
  nombre VARCHAR(50) NOT NULL,
  fecha_contratacion DATE DEFAULT CURRENT_DATE,
  estado VARCHAR(20) DEFAULT 'Activo'
);

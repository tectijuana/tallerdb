*Cruz Hernandez Edgar Eduardo*

```sql
-- Ejemplo de LEFT JOIN en SQL
-- Se muestran todos los departamentos, incluso aquellos sin empleados asignados.

SELECT 
    Departments.Name AS Departamento,
    Employees.FName AS Nombre_Empleado
FROM 
    Departments
LEFT JOIN 
    Employees
ON 
    Departments.Id = Employees.DepartmentId;
```

### Explicación:
- `LEFT JOIN` garantiza que **todos los registros de la tabla izquierda** (`Departments`) sean incluidos en el resultado.
- Si no hay coincidencias en la tabla derecha (`Employees`), las columnas correspondientes devolverán `NULL`.

#### 📌 **Ejemplo de salida esperada**
| Departamento  | Nombre_Empleado |
|--------------|----------------|
| HR          | James          |
| HR          | John           |
| Sales       | Michael        |
| Tech        | NULL           |

En este caso, **el departamento "Tech" no tiene empleados asignados**, por lo que `Nombre_Empleado` aparece como `NULL`.

---

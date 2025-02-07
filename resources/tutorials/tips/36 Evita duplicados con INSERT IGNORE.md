
#Quintero Angulo Roberto Carlos 22210784#

En MySQL, puedes usar `INSERT IGNORE` para evitar la inserción de registros duplicados. Si intentas insertar una fila que provocaría un conflicto de clave única, **INSERT IGNORE** simplemente ignora la nueva fila y no genera un error. Aquí te dejo un ejemplo:

```sql
INSERT IGNORE INTO tabla (columna1, columna2)
VALUES ('valor1', 'valor2');
```

### Explicación:

- **`INSERT IGNORE`**: Intenta insertar los datos. Si ocurre una violación de clave única, MySQL ignora esa fila en lugar de devolver un error.
  
- En este ejemplo, si ya existe una fila con los mismos valores de clave única que los especificados, no se insertará un duplicado y la operación continuará sin errores.

Esta técnica es útil para asegurar que no se agreguen duplicados, especialmente en situaciones donde las verificaciones de duplicados son críticas pero no deseas detener el flujo del programa por errores.

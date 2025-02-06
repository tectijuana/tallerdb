### Jorge Joshel Leon Cruz
### -----------------------------------------------

### tabla del usuario con el uso de `UNIQUE`
```sql
CREATE TABLE usuarios (
    id INT IDENTITY PRIMARY KEY,
    nombre VARCHAR(100),
    email VARCHAR(255) UNIQUE
);
```
#Ventajas de usar UNIQUE
- 1.Evita duplicados: Garantiza que no haya valores repetidos en la columna definida.
- 2.Optimiza las búsquedas: Mejora la eficiencia de consultas filtradas por la columna indexada.
- 3.Ayuda a mantener la integridad de los datos: Se pueden evitar problemas con datos redundantes o inconsistentes.

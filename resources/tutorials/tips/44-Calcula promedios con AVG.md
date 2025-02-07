### Brian Guadalupe Tellez Escobar C21211400

# Función `AVG()` en SQL

La función `AVG()` se utiliza para calcular el promedio de los valores en una columna numérica en SQL.

## 📌 Sintaxis básica
```sql
SELECT AVG(nombre_columna) AS promedio
FROM nombre_tabla;
```

## 📝 Ejemplo práctico
Si tienes una tabla llamada empleados con una columna salario, puedes calcular el salario promedio con la siguiente consulta:

```SELECT AVG(salario) AS salario_promedio
FROM empleados;
```
## 🎯 Uso con WHERE

Puedes filtrar los datos antes de calcular el promedio:
```
SELECT AVG(salario) AS salario_promedio
FROM empleados
WHERE departamento = 'Ventas';
```
## 🔄 Uso con GROUP BY

Si quieres calcular el promedio por departamento:
```
sql
SELECT departamento, AVG(salario) AS salario_promedio
FROM empleados
GROUP BY departamento;
```
## ⚠️ Consideraciones importantes

La función AVG() ignora valores NULL en los cálculos.
Se puede combinar con DISTINCT si solo quieres considerar valores únicos:
```
sql
SELECT AVG(DISTINCT salario) AS salario_promedio
FROM empleados;
```
## 📖 Referencia: Documentos de SQL revisados​.

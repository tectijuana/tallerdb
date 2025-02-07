# Morales Hernandez Christian Gahel
# Uso de AT TIME ZONE en SQL Server

El operador `AT TIME ZONE` en SQL Server se usa para convertir valores de fecha y hora entre distintas zonas horarias.  

## Cómo funciona
1. **Si la fecha/hora original no tiene información de zona horaria**, `AT TIME ZONE` convierte el valor en un `datetimeoffset` con la zona horaria especificada.  
2. **Si la fecha/hora ya es `datetimeoffset`**, ajusta el valor a la nueva zona horaria, manteniendo el punto en el tiempo pero cambiando el desplazamiento desde UTC.  

## Ejemplo de uso
Supongamos que queremos ver datos en la zona horaria "Eastern Standard Time" (EST), pero también convertirlos a UTC:

```sql
DECLARE @asOfTime datetime2(1) = '2020-05-28';

SET @asOfTime = @asOfTime
    AT TIME ZONE 'Eastern Standard Time'  -- Asigna la zona horaria EST
    AT TIME ZONE 'UTC';                   -- Convierte a UTC

SELECT EmployeeId, RowStartTime,
       CAST(RowStartTime AT TIME ZONE 'UTC' 
            AT TIME ZONE 'Eastern Standard Time' AS datetime2(1)) AS RowStartTimeLocal
FROM HumanResources.Employee FOR SYSTEM_TIME AS OF @asOfTime;

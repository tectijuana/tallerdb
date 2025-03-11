# Manejo de Zonas Horarias en SQL Server

Para convertir fechas y horas a diferentes zonas horarias en SQL Server, usa `AT TIME ZONE`. 

### Ejemplo:

```sql
DECLARE @asOfTime datetime2(1) = '2020-05-28';

SET @asOfTime = @asOfTime
         AT TIME ZONE 'Eastern Standard Time'
         AT TIME ZONE 'UTC';

SELECT EmployeeId, RowStartTime,
       CAST(RowStartTime AT TIME ZONE 'UTC'
            AT TIME ZONE 'Eastern Standard Time' AS datetime2(1))
            AS RowStartTimeLocal
FROM   HumanResources.Employee FOR SYSTEM_TIME AS OF @asOfTime;

```

/rsr

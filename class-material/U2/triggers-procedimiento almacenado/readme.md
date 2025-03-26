
<p align="center">
  <img src="https://github.com/user-attachments/assets/4b49989a-12a8-4cbb-9590-4723b411da46" alt="Ilustración medieval de Tux programando" width="500"/>
</p>


# **Actividad: Investigación y Aplicación de Triggers y Procedimientos Almacenados**

### Objetivos:
- Investigar y comprender qué son los triggers y procedimientos almacenados en bases de datos relacionales.
- Diferenciar claramente su funcionamiento y utilidad.
- Implementar ejemplos prácticos utilizando SQL.

### Evidencia de aprendizaje:
Un documento que incluya:
- Definiciones claras de Trigger y Procedimiento Almacenado.
- Una tabla comparativa sobre sus características, ventajas y casos de uso.
- Implementación práctica con ejemplos en SQL.

---

### Ejercicio práctico (Template):

**Base de Datos:** Supón una base de datos sencilla llamada `Biblioteca` con la tabla `Libros`.

```sql
CREATE TABLE Libros (
    id_libro INT PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(100),
    autor VARCHAR(50),
    cantidad INT
);
```

**1. Ejemplo de Procedimiento Almacenado:**

Crea un procedimiento almacenado que permita agregar nuevos libros fácilmente.

```sql
DELIMITER $$
CREATE PROCEDURE AgregarLibro(IN tituloNuevo VARCHAR(100), IN autorNuevo VARCHAR(50), IN cantidadNueva INT)
BEGIN
    INSERT INTO Libros (titulo, autor, cantidad) VALUES (tituloNuevo, autorNuevo, cantidadNueva);
END$$
DELIMITER ;
```

**Uso del procedimiento:**

```sql
CALL AgregarLibro('Cien años de soledad', 'Gabriel García Márquez', 10);
```

**2. Ejemplo de Trigger:**

Crea un trigger que registre automáticamente en una tabla llamada `RegistroCambios` cada vez que se actualice la cantidad de ejemplares de un libro.

```sql
CREATE TABLE RegistroCambios (
    id_registro INT PRIMARY KEY AUTO_INCREMENT,
    id_libro INT,
    cantidad_anterior INT,
    cantidad_nueva INT,
    fecha_cambio TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

DELIMITER $$
CREATE TRIGGER trg_ActualizarCantidad
AFTER UPDATE ON Libros
FOR EACH ROW
BEGIN
    INSERT INTO RegistroCambios (id_libro, cantidad_anterior, cantidad_nueva)
    VALUES (NEW.id_libro, OLD.cantidad, NEW.cantidad);
END$$
DELIMITER ;
```

**Probando el trigger:**

```sql
UPDATE Libros SET cantidad = 15 WHERE id_libro = 1;
```

---

### Entrega:
Sube a a Idoceo Connect el GIST (de preferencia con markdown) con tu investigación, tabla comparativa y capturas de pantalla de los ejemplos prácticos funcionando en una base de datos relacional real.

### Criterios de Evaluación:
![Screenshot 2025-03-25 at 5 17 16 p m](https://github.com/user-attachments/assets/38268a1d-7f1a-4b3e-b850-ef1836e176e4)

¡Éxito! 🚀📚


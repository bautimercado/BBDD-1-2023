# Practica 4 - SQL

# Consultas SQL

## Llevar a SQL las siguientes consultas antes realizadas en Álgebra Relacional:

- DUEÑO (id_dueño, nombre, teléfono, dirección, dni)
- CHOFER (id_chofer, nombre, teléfono, dirección, fecha_licencia_desde, fecha_licencia_hasta, dni)
- AUTO (patente, id_dueño, id_chofer, marca, modelo, año)
- VIAJE (patente, hora_desde, hora_hasta, origen, destino, tarifa, metraje)

### a. Listar el dni, nombre y teléfono de todos los dueños que NO son choferes

```sql
SELECT dni, nombre, telefono
FROM DUEÑO
WHERE dni NOT IN (SELECT dni
                  FROM CHOFER)
```

### b. Listar la patente y el id_chofer de todos los autos a cuyos choferes les caduca la licencia el 01/01/2024

```sql
SELECT a.patente, a.id_chofer
FROM AUTO as a NATURAL JOIN CHOFER as c
WHERE c.fecha_licencia_hasta like 20240101
```

- ESTUDIANTE(#legajo, nombreCompleto, nacionalidad, añoDeIngreso, códigoDeCarrera)
- CARRERA(códigoDeCarrera, nombre)
- INSCRIPCIONAMATERIA(#legajo, códigoDeMateria)
- MATERIA(códigoDeMateria, nombre)

### a. Obtener el nombre de los estudiantes que ingresaron en 2019.

```sql
SELECT nombreCompleto
FROM ESTUDIANTE
WHERE añoDeIngreso = '2019'
```

### b. Obtener el nombre de los estudiantes con nacionalidad “Argentina” que NO estén en la carrera con código “LI07”

- Preguntar cuál sería correcta.

```sql
-- Forma 1
SELECT nombreCompleto
FROM ESTUDIANTE
WHERE nacionalidad = 'Argentina' 
    and codigoDeCarrera <> 'LI07'
```
```sql
-- Forma 2
SELECT nombreCompleto
FROM ESTUDIANTE
WHERE nacionalidad = 'Argentina'
    and codigoDeCarrera NOT IN (SELECT codigoDeCarrera
                                FROM CARRERA
                                WHERE codigoDeCarrera = 'LI07')
```

### Compare las resoluciones de estos ejercicios con las realizadas en álgebra relacional, que paralelismo encuentra entre las diferentes operaciones de AR en SQL y en las formas de las resoluciones?

- SQL está basado en AR, por esa razón es que sus consultas son parecidas. Si embargo, por las caracterísitcas de SQL, las consultas son distintas, por ejemplo, en Álgebra Relacional podemos modularizar las consultas y en SQL podemos hacer subconsultas.
- En AR solo se indica el nombre de la relación, en SQL debemos hacerlo con la cláusula `FROM`.

|     | Proyección | Selección | Renombre | Prod. Cartesiano |
| --- | ---------- | --------- | -------- | ---------------- |
| AR  |     π      |     σ     |     ρ    |         ⨯        |
| SQL |  `SELECT`  |  `WHERE`  |   `as`   |    INNER JOIN    |

# Ejercicios

## 1. Crear usuarios para las bases de datos, usando los nombres reparacion para la versión normalizada y reparacion_dn para la desnormalizada. Asigne a estos todos los permisos sobre sus respectivas tablas. Habiendo creado estos usuarios, evitar el uso de root para el resto del trabajo práctico.

```sql
CREATE USER 'reparacion'@'localhost'
    IDENTIFIED with caching_sha2_password BY '1234';

GRANT ALL PRIVILEGES ON reparacion.* TO 'reparacion'@'localhost';
```

```sql
CREATE USER 'reparacion_dn'@'localhost'
    IDENTIFIED with caching_sha2_password BY '1234';

GRANT ALL PRIVILEGES ON reparacion_dn.* TO 'reparacion_dn'@'localhost';
```

### 1.1. Adicionalmente, en ambas bases:

- Cree un usuario sólo con permisos para realizar consultas de selección, es decir que no puedan realizar cambios en la base. Use los nombres 'reparacion_select’ y 'reparacion_dn_select’.

```sql
CREATE USER 'reparacion_select'@'localhost'
    IDENTIFIED with caching_sha2_password BY '1234';

GRANT SELECT ON reparacion.* TO 'reparacion_select'@'localhost';
```

```sql
CREATE USER 'reparacion_dn_select'@'localhost'
    IDENTIFIED with caching_sha2_password BY '1234';

GRANT SELECT ON reparacion_dn.* TO 'reparacion_dn_select'@'localhost';
```

- Cree un usuario que pueda realizar consultas de selección, inserción, actualización y eliminación a nivel de filas, pero que no puedan modificar el esquema. Use los nombres 'reparacion_update’ y 'reparacion_dn_update’.

```sql
CREATE USER 'reparacion_update'@'localhost'
    IDENTIFIED with caching_sha2_password BY '1234';

GRANT SELECT, INSERT, UPDATE, DELETE ON reparacion.* TO 'reparacion_update'@'localhost';
```

```sql
CREATE USER 'reparacion_dn_update'@'localhost'
    IDENTIFIED with caching_sha2_password BY '1234';

GRANT SELECT, INSERT, UPDATE, DELETE ON reparacion_dn.* TO 'reparacion_dn_update'@'localhost';
```

- Cree un usuario que tenga los permisos de los anteriores, pero que además pueda modificar el esquema de la base de datos. Use los nombres 'reparacion_schema’ y 'reparacion_dn_schema’.

```sql
CREATE USER 'reparacion_schema'@'localhost'
    IDENTIFIED with caching_sha2_password BY '1234';

GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, ALTER ON reparacion.* TO 'reparacion_schema'@'localhost';
```

```sql
CREATE USER 'reparacion_dn_schema'@'localhost'
    IDENTIFIED with caching_sha2_password BY '1234';

GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, ALTER ON reparacion_dn.* TO 'reparacion_dn_schema'@'localhost';
```

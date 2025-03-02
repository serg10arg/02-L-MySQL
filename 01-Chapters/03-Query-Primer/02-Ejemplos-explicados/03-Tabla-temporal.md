
### Desglose de las Sentencias SQL

#### 1. Creación de una Tabla Temporal

```sql
CREATE TEMPORARY TABLE actors_j
  (actor_id SMALLINT(5),
   first_name VARCHAR(45),
   last_name VARCHAR(45));
```
- **`CREATE TEMPORARY TABLE actors_j`**:
   - Esta sentencia crea una tabla temporal llamada `actors_j`.
   - Las tablas temporales son utilizadas para almacenar datos de manera temporal durante la sesión de la base de datos. Los datos en estas tablas desaparecerán al finalizar la sesión.

- **Definición de Columnas**:
   - `actor_id SMALLINT(5)`: Define una columna llamada `actor_id` con el tipo de datos `SMALLINT` y un tamaño de 5 dígitos.
   - `first_name VARCHAR(45)`: Define una columna llamada `first_name` con el tipo de datos `VARCHAR` y un tamaño máximo de 45 caracteres.
   - `last_name VARCHAR(45)`: Define una columna llamada `last_name` con el tipo de datos `VARCHAR` y un tamaño máximo de 45 caracteres.

#### 2. Inserción de Datos en la Tabla Temporal

```sql
INSERT INTO actors_j
  SELECT actor_id, first_name, last_name
  FROM actor
  WHERE last_name LIKE 'J%';
```
- **`INSERT INTO actors_j`**:
   - Esta sentencia inserta datos en la tabla temporal `actors_j`.

- **Subconsulta**:
   - `SELECT actor_id, first_name, last_name FROM actor WHERE last_name LIKE 'J%'`:
      - Selecciona las columnas `actor_id`, `first_name` y `last_name` de la tabla `actor`.
      - Filtra las filas donde el `last_name` comienza con la letra 'J' (indicado por el patrón `LIKE 'J%'`).
      - Los datos seleccionados se insertan en la tabla temporal `actors_j`.

#### 3. Selección de Todos los Datos de la Tabla Temporal

```sql
SELECT * FROM actors_j;
```
- **`SELECT * FROM actors_j`**:
   - Esta sentencia selecciona todas las columnas y filas de la tabla temporal `actors_j`.

### Explicación del Resultado

Supongamos que la tabla `actor` contiene los siguientes datos:

| actor_id | first_name | last_name |
|----------|------------|-----------|
| 1        | JOHNNY     | DEPP      |
| 2        | SCARLETT   | JOHANSSON |
| 3        | BRAD       | PITT      |
| 4        | JENNIFER   | LAWRENCE  |
| 5        | JACK       | NICHOLSON |
| 6        | JESSICA    | ALBA      |
| 7        | JAKE       | GYLLENHAAL|

Después de ejecutar las sentencias SQL, la tabla `actors_j` contendrá las filas que tienen apellidos que comienzan con 'J':

| actor_id | first_name | last_name |
|----------|------------|-----------|
| 1        | JOHNNY     | DEPP      |
| 2        | SCARLETT   | JOHANSSON |
| 4        | JENNIFER   | LAWRENCE  |
| 5        | JACK       | NICHOLSON |
| 6        | JESSICA    | ALBA      |
| 7        | JAKE       | GYLLENHAAL|

Y la salida de la sentencia `SELECT * FROM actors_j` mostrará todos los datos en la tabla `actors_j`:

| actor_id | first_name | last_name |
|----------|------------|-----------|
| 1        | JOHNNY     | DEPP      |
| 2        | SCARLETT   | JOHANSSON |
| 4        | JENNIFER   | LAWRENCE  |
| 5        | JACK       | NICHOLSON |
| 6        | JESSICA    | ALBA      |
| 7        | JAKE       | GYLLENHAAL|

Espero que esta explicación te sea útil para comprender mejor cómo funcionan estas sentencias SQL.
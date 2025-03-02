### Sentencia SQL:

```sql
SELECT last_name, first_name
FROM customer
WHERE last_name REGEXP '^[QY]';
```

---

### Paso 1: `SELECT last_name, first_name`

- **`SELECT`**: Esta es una palabra clave en SQL que se utiliza para especificar qué columnas queremos obtener de la base de datos.
- **`last_name, first_name`**: Aquí, estamos seleccionando dos columnas específicas: `last_name` (apellido) y `first_name` (nombre).

### Explicación:

La parte `SELECT last_name, first_name` nos dice que queremos obtener los valores de las columnas `last_name` y `first_name` de cada fila que cumpla con los criterios que definamos más adelante.

### Ejemplo Práctico:

Imagina una tabla llamada `customer` con los siguientes datos:

| customer_id | last_name | first_name |
| --- | --- | --- |
| 1 | Quinn | John |
| 2 | Young | Jane |
| 3 | Smith | Alice |
| 4 | Johnson | Bob |

Si ejecutamos solo `SELECT last_name, first_name FROM customer;`, obtendremos:

---

### Paso 2: `FROM customer`

- **`FROM`**: Esta palabra clave se utiliza para especificar la tabla de la que queremos obtener los datos.
- **`customer`**: Este es el nombre de la tabla que contiene los datos que necesitamos.

### Explicación:

La parte `FROM customer` nos dice que los datos que queremos seleccionar provienen de la tabla llamada `customer`.

### Ejemplo Práctico:

La tabla `customer` es la fuente de los datos. En nuestro ejemplo anterior, la tabla `customer` contiene los datos de los clientes, incluyendo sus nombres y apellidos.

---

### Paso 3: `WHERE last_name REGEXP '^[QY]'`

- **`WHERE`**: Esta palabra clave se utiliza para filtrar los datos según una condición específica. Solo se seleccionarán las filas que cumplan con esta condición.
- **`last_name`**: Esta es la columna en la que aplicaremos la condición. En este caso, estamos filtrando por el apellido (`last_name`).
- **`REGEXP '^[QY]'`**: Esta es una condición que utiliza una expresión regular (regex) para buscar patrones en los datos.
- `^` significa "comienza con".
- `[QY]` significa "cualquier letra que sea Q o Y".
- Juntos, `^[QY]` significa "selecciona solo los apellidos que comiencen con Q o Y".

### Explicación:

La parte `WHERE last_name REGEXP '^[QY]'` nos dice que solo queremos las filas donde el apellido (`last_name`) comience con la letra Q o la letra Y.

### Ejemplo Práctico:

Siguiendo nuestro ejemplo anterior, la tabla `customer` tiene los siguientes datos:

| customer_id | last_name | first_name |
| --- | --- | --- |
| 1 | Quinn | John |
| 2 | Young | Jane |
| 3 | Smith | Alice |
| 4 | Johnson | Bob |

La condición `WHERE last_name REGEXP '^[QY]'` filtrará solo los apellidos que comiencen con Q o Y. En este caso, solo se seleccionarán las filas con los apellidos "Quinn" y "Young".

El resultado final sería:

---

### Resumen Completo:

La sentencia SQL:

```sql
SELECT last_name, first_name
FROM customer
WHERE last_name REGEXP '^[QY]';
```

hace lo siguiente:

1. Selecciona las columnas `last_name` y `first_name` de la tabla `customer`.
2. Filtra solo las filas donde el apellido (`last_name`) comience con la letra Q o la letra Y.
3. Dev losuelve resultados que cumplen con esta condición.

---

### Ejemplo Práctico Completo:

Supongamos que la tabla `customer` tiene los siguientes datos:

| customer_id | last_name | first_name |
| --- | --- | --- |
| 1 | Quinn | John |
| 2 | Young | Jane |
| 3 | Smith | Alice |
| 4 | Johnson | Bob |
| 5 | Yoder | Mark |

Al ejecutar la sentencia SQL, el resultado sería:

Esto es porque "Quinn", "Young" y "Yoder" son los únicos apellidos que comienzan con Q o Y.

---
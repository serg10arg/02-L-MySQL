Este es un ejemplo práctico y detallado del uso de **Expresiones de Tabla Comunes (CTEs)** en MySQL. Vamos a analizar paso a paso cómo funciona esta consulta y qué hace cada parte.

---

### Estructura de la consulta

La consulta utiliza tres CTEs (`actors_s`, `actors_s_pg`, y `actors_s_pg_revenue`) para calcular los ingresos totales generados por los alquileres de películas con clasificación PG en las que han actuado actores cuyos apellidos comienzan con la letra "S". Finalmente, la consulta principal agrupa los resultados por actor y ordena los ingresos de mayor a menor.

---

### Paso 1: Primera CTE (`actors_s`)

```sql
WITH actors_s AS (
    SELECT actor_id, first_name, last_name
    FROM actor
    WHERE last_name LIKE 'S%'
)
```

- **Objetivo:** Seleccionar todos los actores cuyo apellido comienza con la letra "S".
- **Tabla involucrada:** `actor`.
- **Filtro:** `last_name LIKE 'S%'` (apellidos que comienzan con "S").
- **Resultado:** Una tabla temporal con las columnas `actor_id`, `first_name`, y `last_name` de los actores que cumplen la condición.

---

### Paso 2: Segunda CTE (`actors_s_pg`)

```sql
actors_s_pg AS (
    SELECT s.actor_id, s.first_name, s.last_name, f.film_id, f.title
    FROM actors_s s
    INNER JOIN film_actor fa ON s.actor_id = fa.actor_id
    INNER JOIN film f ON f.film_id = fa.film_id
    WHERE f.rating = 'PG'
)
```

- **Objetivo:** Filtrar las películas con clasificación PG en las que han actuado los actores seleccionados en la primera CTE.
- **Tablas involucradas:**
    - `actors_s` (CTE anterior).
    - `film_actor` (tabla de relación entre actores y películas).
    - `film` (tabla de películas).
- **Filtro:** `f.rating = 'PG'` (solo películas con clasificación PG).
- **Resultado:** Una tabla temporal con las columnas `actor_id`, `first_name`, `last_name`, `film_id`, y `title` de las películas PG en las que han actuado los actores cuyo apellido comienza con "S".

---

### Paso 3: Tercera CTE (`actors_s_pg_revenue`)

```sql
actors_s_pg_revenue AS (
    SELECT spg.first_name, spg.last_name, p.amount
    FROM actors_s_pg spg
    INNER JOIN inventory i ON i.film_id = spg.film_id
    INNER JOIN rental r ON i.inventory_id = r.inventory_id
    INNER JOIN payment p ON r.rental_id = p.rental_id
)
```

- **Objetivo:** Calcular los ingresos generados por los alquileres de las películas seleccionadas en la segunda CTE.
- **Tablas involucradas:**
    - `actors_s_pg` (CTE anterior).
    - `inventory` (tabla de inventario de películas).
    - `rental` (tabla de alquileres).
    - `payment` (tabla de pagos).
- **Resultado:** Una tabla temporal con las columnas `first_name`, `last_name`, y `amount` (monto pagado por el alquiler).

---

### Paso 4: Consulta principal

```sql
SELECT spg_rev.first_name, spg_rev.last_name, SUM(spg_rev.amount) AS tot_revenue
FROM actors_s_pg_revenue spg_rev
GROUP BY spg_rev.first_name, spg_rev.last_name
ORDER BY tot_revenue DESC;
```

- **Objetivo:** Agrupar los resultados por actor y sumar los ingresos totales generados por cada uno.
- **Funciones utilizadas:**
    - `SUM(spg_rev.amount)`: Suma los montos pagados por los alquileres.
    - `GROUP BY`: Agrupa los resultados por `first_name` y `last_name`.
    - `ORDER BY tot_revenue DESC`: Ordena los resultados de mayor a menor ingreso.
- **Resultado final:** Una lista de actores cuyos apellidos comienzan con "S", junto con los ingresos totales generados por los alquileres de películas PG en las que han actuado.

---

### Resultado de la consulta

```sql
+------------+-------------+-------------+
| first_name | last_name   | tot_revenue |
+------------+-------------+-------------+
| NICK       | STALLONE    |      692.21 |
| JEFF       | SILVERSTONE |      652.35 |
| DAN        | STREEP      |      509.02 |
| GROUCHO    | SINATRA     |      457.97 |
| SISSY      | SOBIESKI    |      379.03 |
| JAYNE      | SILVERSTONE |      372.18 |
| CAMERON    | STREEP      |      361.00 |
| JOHN       | SUVARI      |      296.36 |
| JOE        | SWANK       |      177.52 |
+------------+-------------+-------------+
9 rows in set (0.18 sec)
```

- **Interpretación:** La consulta devuelve 9 filas, donde cada fila representa un actor cuyo apellido comienza con "S" y el total de ingresos generados por los alquileres de películas PG en las que ha participado.
- **Tiempo de ejecución:** 0.18 segundos.

---

### Resumen

1. **CTE 1 (`actors_s`):** Filtra actores con apellidos que comienzan con "S".
2. **CTE 2 (`actors_s_pg`):** Filtra películas PG en las que han actuado esos actores.
3. **CTE 3 (`actors_s_pg_revenue`):** Calcula los ingresos generados por los alquileres de esas películas.
4. **Consulta principal:** Agrupa y suma los ingresos por actor, ordenándolos de mayor a menor.

Este enfoque modular y estructurado hace que la consulta sea más clara, mantenible y fácil de entender. Las CTEs son una excelente alternativa a las subconsultas anidadas o a las tablas temporales en muchos casos.
Las **Expresiones de Tabla Comunes** (CTEs, por sus siglas en inglés, **Common Table Expressions**) son una característica poderosa en SQL que permite definir subconsultas con nombre que pueden ser reutilizadas dentro de una consulta principal. Estas subconsultas se definen dentro de una cláusula `WITH` y son especialmente útiles para simplificar consultas complejas, mejorar la legibilidad y evitar la repetición de código.

### Características principales de las CTEs:

1. **Definición en la cláusula `WITH`:** Las CTEs se definen al principio de una consulta utilizando la cláusula `WITH`. Puedes definir una o varias CTEs separadas por comas.

2. **Reutilización:** Una vez definida una CTE, puedes referenciarla en la consulta principal o en otras CTEs definidas posteriormente en la misma cláusula `WITH`.

3. **Vida útil:** Las CTEs existen solo durante la ejecución de la consulta. No son almacenadas en la base de datos como tablas temporales, lo que las hace más eficientes en términos de recursos.

4. **Legibilidad:** Al dividir una consulta compleja en partes más pequeñas y nombradas, las CTEs hacen que el código sea más fácil de entender y mantener.

### Ejemplo detallado:

El ejemplo que proporcionaste utiliza tres CTEs para calcular los ingresos totales generados por los alquileres de películas con clasificación PG donde el elenco incluye a un actor cuyo apellido comienza con "S". Vamos a desglosar cada parte:

#### 1. Primera CTE: `actors_s`
```sql
WITH actors_s AS (
    SELECT actor_id, first_name, last_name
    FROM actor
    WHERE last_name LIKE 'S%'
)
```
- **Objetivo:** Selecciona todos los actores cuyo apellido comienza con la letra "S".
- **Resultado:** Una tabla temporal con las columnas `actor_id`, `first_name`, y `last_name` de los actores que cumplen con la condición.

#### 2. Segunda CTE: `actors_s_pg`
```sql
actors_s_pg AS (
    SELECT s.actor_id, s.first_name, s.last_name, f.film_id, f.title
    FROM actors_s s
    INNER JOIN film_actor fa ON s.actor_id = fa.actor_id
    INNER JOIN film f ON f.film_id = fa.film_id
    WHERE f.rating = 'PG'
)
```
- **Objetivo:** Filtra las películas con clasificación PG en las que han actuado los actores seleccionados en la primera CTE.
- **Resultado:** Una tabla temporal con las columnas `actor_id`, `first_name`, `last_name`, `film_id`, y `title` de las películas PG en las que han actuado los actores cuyo apellido comienza con "S".

#### 3. Tercera CTE: `actors_s_pg_revenue`
```sql
actors_s_pg_revenue AS (
    SELECT spg.first_name, spg.last_name, p.amount
    FROM actors_s_pg spg
    INNER JOIN inventory i ON i.film_id = spg.film_id
    INNER JOIN rental r ON i.inventory_id = r.inventory_id
    INNER JOIN payment p ON r.rental_id = p.rental_id
)
```
- **Objetivo:** Calcula los ingresos generados por los alquileres de las películas seleccionadas en la segunda CTE.
- **Resultado:** Una tabla temporal con las columnas `first_name`, `last_name`, y `amount` (monto pagado por el alquiler).

#### 4. Consulta principal:
```sql
SELECT spg_rev.first_name, spg_rev.last_name, SUM(spg_rev.amount) AS tot_revenue
FROM actors_s_pg_revenue spg_rev
GROUP BY spg_rev.first_name, spg_rev.last_name
ORDER BY tot_revenue DESC;
```
- **Objetivo:** Agrupa los resultados por actor y suma los ingresos totales generados por cada uno.
- **Resultado:** Una lista de actores cuyos apellidos comienzan con "S", junto con los ingresos totales generados por los alquileres de películas PG en las que han actuado, ordenados de mayor a menor ingreso.

### Ventajas de usar CTEs:

1. **Legibilidad:** Al dividir la consulta en partes más pequeñas, es más fácil entender y mantener el código.
2. **Reutilización:** Puedes referenciar una CTE múltiples veces dentro de la misma consulta, evitando la repetición de subconsultas.
3. **Alternativa a las tablas temporales:** Las CTEs son una alternativa más eficiente a las tablas temporales, ya que no requieren almacenamiento persistente en la base de datos.

### Consideraciones:

- **Vida útil:** Las CTEs solo existen durante la ejecución de la consulta. Si necesitas reutilizar los resultados en múltiples consultas, podrías considerar el uso de tablas temporales o vistas.
- **Rendimiento:** En algunos casos, el uso de CTEs puede no ser tan eficiente como otras técnicas, especialmente en consultas muy complejas o con grandes volúmenes de datos. Es importante analizar el plan de ejecución para optimizar el rendimiento.

En resumen, las CTEs son una herramienta poderosa en SQL que te permite escribir consultas más claras, modulares y mantenibles, especialmente cuando trabajas con consultas complejas que involucran múltiples pasos o subconsultas.
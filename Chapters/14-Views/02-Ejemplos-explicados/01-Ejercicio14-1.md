Create a view definition that can be used by the following query to generate the given
results:

```sql
SELECT title, category_name, first_name, last_name
FROM film_ctgry_actor
WHERE last_name = 'FAWCETT';
```
---
**Sentencia `CREATE VIEW` en español:**

```sql
CREATE VIEW film_ctgry_actor AS
SELECT
    f.title,
    c.name AS category_name,
    a.first_name,
    a.last_name
FROM
    film f
INNER JOIN
    film_category fc ON f.film_id = fc.film_id
INNER JOIN
    category c ON fc.category_id = c.category_id
INNER JOIN
    film_actor fa ON f.film_id = fa.film_id
INNER JOIN
    actor a ON fa.actor_id = a.actor_id;
```

**Explicación Detallada en español:**

1.  **`CREATE VIEW film_ctgry_actor AS`**: Esta parte de la sentencia inicia la creación de una vista llamada `film_ctgry_actor`.  En español, esto se traduce como "CREAR VISTA film_ctgry_actor COMO".  `CREATE VIEW` es la instrucción SQL para definir una nueva vista. `film_ctgry_actor` es el nombre que estamos eligiendo para esta vista. `AS` indica que lo que sigue es la consulta `SELECT` que definirá el contenido de la vista.

2.  **`SELECT f.title, c.name AS category_name, a.first_name, a.last_name`**:  Esta sección especifica las columnas que se incluirán en la vista y cómo se obtienen:
    *   `f.title`: Selecciona la columna `title` de la tabla `film` (a la que se le ha dado el alias `f`). En español, `title` se refiere al "título" de la película.
    *   `c.name AS category_name`: Selecciona la columna `name` de la tabla `category` (con el alias `c`) y la renombra como `category_name` en la vista.  `name` en la tabla `category` contiene el "nombre" de la categoría, y al renombrarla a `category_name` hacemos más claro su significado en la vista (nombre de categoría).  `AS category_name` en español significa "COMO nombre_categoría", es decir, estamos dando un alias a la columna.
    *   `a.first_name`: Selecciona la columna `first_name` de la tabla `actor` (con el alias `a`). `first_name` es el "nombre" del actor.
    *   `a.last_name`: Selecciona la columna `last_name` de la tabla `actor` (con el alias `a`). `last_name` es el "apellido" del actor.

3.  **`FROM film f`**: Esto indica que la tabla de partida para las uniones (JOINs) es la tabla `film`, a la que se le asigna el alias `f`.  `FROM film f` en español significa "DESDE tabla película f".  `film` es la tabla base principal desde donde comenzamos a construir nuestra vista.

4.  **`INNER JOIN film_category fc ON f.film_id = fc.film_id`**:  Esto realiza una unión interna (`INNER JOIN`) entre la tabla `film` (alias `f`) y la tabla `film_category` (alias `fc`), utilizando la columna `film_id` como condición de unión.  `ON f.film_id = fc.film_id` especifica que la unión se basa en que los valores de la columna `film_id` sean iguales en ambas tablas. Esta unión es necesaria para relacionar las películas con sus categorías.  `INNER JOIN film_category fc ON f.film_id = fc.film_id` en español sería "UNION INTERNA tabla_película_categoría fc ON columna película_id de f ES IGUAL A columna película_id de fc".

5.  **`INNER JOIN category c ON fc.category_id = c.category_id`**: Esto une (`INNER JOIN`) la tabla `film_category` (alias `fc`) con la tabla `category` (alias `c`), usando la columna `category_id` como clave de unión. Esta unión se usa para obtener el nombre de la categoría (`c.name`) para cada película. `INNER JOIN category c ON fc.category_id = c.category_id` en español sería "UNION INTERNA tabla_categoría c ON columna categoría_id de fc ES IGUAL A columna categoría_id de c".

6.  **`INNER JOIN film_actor fa ON f.film_id = fa.film_id`**:  Esto une (`INNER JOIN`) la tabla `film` (alias `f`) con la tabla `film_actor` (alias `fa`), utilizando la columna `film_id`.  Esta unión es necesaria para relacionar las películas con los actores que participan en ellas. `INNER JOIN film_actor fa ON f.film_id = fa.film_id` en español sería "UNION INTERNA tabla_película_actor fa ON columna película_id de f ES IGUAL A columna película_id de fa".

7.  **`INNER JOIN actor a ON fa.actor_id = a.actor_id`**: Finalmente, se une (`INNER JOIN`) la tabla `film_actor` (alias `fa`) con la tabla `actor` (alias `a`), usando la columna `actor_id`. Esta unión se utiliza para obtener el nombre y apellido del actor (`a.first_name`, `a.last_name`). `INNER JOIN actor a ON fa.actor_id = a.actor_id` en español sería "UNION INTERNA tabla_actor a ON columna actor_id de fa ES IGUAL A columna actor_id de a".

**En resumen, esta vista `film_ctgry_actor` combina información de las tablas `film`, `category` y `actor` a través de las tablas de unión `film_category` y `film_actor`.  El resultado es una vista que muestra el título de la película, el nombre de la categoría, el nombre del actor y el apellido del actor para cada combinación posible.**

**Para verificar la vista, puedes ejecutar la consulta proporcionada en el ejercicio contra la vista recién creada:**

```sql
SELECT title, category_name, first_name, last_name
FROM film_ctgry_actor
WHERE last_name = 'FAWCETT';
```

Esto debería devolver exactamente el conjunto de resultados que se muestra en el ejercicio, demostrando que la vista está correctamente definida y funciona como se espera para la consulta dada.


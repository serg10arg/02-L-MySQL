### Comprobación de Existencia

El texto trata sobre cómo verificar la existencia de una relación entre dos entidades en una base de datos sin preocuparse por la cantidad exacta de relaciones. Esto se logra utilizando la cláusula `EXISTS` en SQL, que devuelve `TRUE` si al menos una fila cumple con la condición y `FALSE` si no hay ninguna fila que cumpla con la condición.

### Ejemplo de Comprobación de Existencia

#### Consulta para verificar si un actor ha aparecido en películas con clasificación G, PG y NC-17

```sql
SELECT a.first_name, a.last_name,
       CASE
           WHEN EXISTS (SELECT 1 FROM film_actor fa
                        INNER JOIN film f ON fa.film_id = f.film_id
                        WHERE fa.actor_id = a.actor_id
                          AND f.rating = 'G') THEN 'Y'
           ELSE 'N'
       END g_actor,
       CASE
           WHEN EXISTS (SELECT 1 FROM film_actor fa
                        INNER JOIN film f ON fa.film_id = f.film_id
                        WHERE fa.actor_id = a.actor_id
                          AND f.rating = 'PG') THEN 'Y'
           ELSE 'N'
       END pg_actor,
       CASE
           WHEN EXISTS (SELECT 1 FROM film_actor fa
                        INNER JOIN film f ON fa.film_id = f.film_id
                        WHERE fa.actor_id = a.actor_id
                          AND f.rating = 'NC-17') THEN 'Y'
           ELSE 'N'
       END nc17_actor
FROM actor a
WHERE a.last_name LIKE 'S%' OR a.first_name LIKE 'S%';
```

#### Explicación detallada

1. **Selección de actores**:
    - `SELECT a.first_name, a.last_name`: Selecciona el nombre y apellido de los actores.
    - `FROM actor a`: La tabla `actor` se usa como fuente principal.
    - `WHERE a.last_name LIKE 'S%' OR a.first_name LIKE 'S%'`: Filtra los actores cuyo nombre o apellido comienza con 'S'.

2. **Comprobación de existencia de películas por clasificación**:
    - **`CASE` para películas con clasificación G**:
        - `WHEN EXISTS (SELECT 1 FROM film_actor fa INNER JOIN film f ON fa.film_id = f.film_id WHERE fa.actor_id = a.actor_id AND f.rating = 'G') THEN 'Y' ELSE 'N'`:
            - `EXISTS (SELECT 1 ...)`: Verifica si existe al menos una fila que cumpla con la condición.
            - `INNER JOIN film f ON fa.film_id = f.film_id`: Une la tabla `film_actor` con la tabla `film` para obtener la clasificación de las películas.
            - `WHERE fa.actor_id = a.actor_id AND f.rating = 'G'`: Filtra las películas donde el actor tiene un papel y la película tiene clasificación G.
            - `THEN 'Y' ELSE 'N'`: Si existe al menos una película con clasificación G, devuelve 'Y'; de lo contrario, devuelve 'N'.
    - **`CASE` para películas con clasificación PG**:
        - Similar al caso anterior, pero para películas con clasificación PG.
    - **`CASE` para películas con clasificación NC-17**:
        - Similar al caso anterior, pero para películas con clasificación NC-17.

#### Resultado

El resultado es una tabla que muestra si cada actor ha aparecido en películas con clasificación G, PG y NC-17:

| first_name | last_name   | g_actor | pg_actor | nc17_actor |
|------------|-------------|---------|----------|------------|
| JOE        | SWANK       | Y       | Y        | Y          |
| SANDRA     | KILMER      | Y       | Y        | Y          |
| CAMERON    | STREEP      | Y       | Y        | Y          |
| ...        | ...         | ...     | ...      | ...        |

### Ejemplo de Conteo Condicional

#### Consulta para verificar la disponibilidad de películas en el inventario

```sql
SELECT f.title,
       CASE (SELECT count(*) FROM inventory i 
             WHERE i.film_id = f.film_id)
           WHEN 0 THEN 'Out Of Stock'
           WHEN 1 THEN 'Scarce'
           WHEN 2 THEN 'Scarce'
           WHEN 3 THEN 'Available'
           WHEN 4 THEN 'Available'
           ELSE 'Common'
       END film_availability
FROM film f;
```

#### Explicación detallada

1. **Selección de películas**:
    - `SELECT f.title`: Selecciona el título de las películas.
    - `FROM film f`: La tabla `film` se usa como fuente principal.

2. **Conteo condicional de copias en inventario**:
    - `CASE (SELECT count(*) FROM inventory i WHERE i.film_id = f.film_id) ...`:
        - `SELECT count(*) FROM inventory i WHERE i.film_id = f.film_id`: Cuenta el número de copias de cada película en el inventario.
        - `WHEN 0 THEN 'Out Of Stock'`: Si no hay copias, la película está agotada.
        - `WHEN 1 THEN 'Scarce'`: Si hay una copia, la película es escasa.
        - `WHEN 2 THEN 'Scarce'`: Si hay dos copias, la película sigue siendo escasa.
        - `WHEN 3 THEN 'Available'`: Si hay tres copias, la película está disponible.
        - `WHEN 4 THEN 'Available'`: Si hay cuatro copias, la película está disponible.
        - `ELSE 'Common'`: Si hay más de cuatro copias, la película es común.

#### Resultado

El resultado es una tabla que muestra la disponibilidad de cada película en el inventario:

| title             | film_availability |
|-------------------|-------------------|
| ACADEMY DINOSAUR  | Common            |
| ACE GOLDFINGER    | Available         |
| ADAPTATION HOLES  | Available         |
| AFFAIR PREJUDICE  | Common            |
| ...               | ...               |

### Resumen

- **Comprobación de Existencia**: Utiliza `EXISTS` para verificar si hay al menos una fila que cumpla con una condición específica.
- **Conteo Condicional**: Utiliza `CASE` con `SELECT count(*)` para evaluar el número de filas y devolver un valor específico según el conteo.

Estos ejemplos demuestran cómo se pueden utilizar consultas SQL para verificar la existencia de relaciones y para evaluar condiciones basadas en el conteo de filas.
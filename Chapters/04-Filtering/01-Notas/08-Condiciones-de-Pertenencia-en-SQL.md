### Condiciones de Pertenencia en SQL

En algunos casos, no restringirás una expresión a un solo valor o rango de valores, sino a un conjunto finito de valores. Vamos a desglosar cómo funcionan estas condiciones de pertenencia y cómo puedes utilizarlas.

#### Ejemplo de Condición de Pertenencia

Supongamos que deseas localizar todas las películas que tienen una calificación de `'G'` o `'PG'`. Puedes hacerlo con la siguiente consulta:

```sql
mysql> SELECT title, rating
    -> FROM film
    -> WHERE rating = 'G' OR rating = 'PG';
```

**Resultado**:

```sql
+---------------------------+--------+
| title                     | rating |
+---------------------------+--------+
| ACADEMY DINOSAUR          | PG     |
| ACE GOLDFINGER            | G      |
| AFFAIR PREJUDICE          | G      |
| AFRICAN EGG               | G      |
| AGENT TRUMAN              | PG     |
| ALAMO VIDEOTAPE           | G      |
| ALASKA PHANTOM            | PG     |
| ALI FOREVER               | PG     |
| AMADEUS HOLY              | PG     |
...
| WEDDING APOLLO            | PG     |
| WEREWOLF LOLA             | G      |
| WEST LION                 | G      |
| WIZARD COLDBLOODED        | PG     |
| WON DARES                 | PG     |
| WONDERLAND CHRISTMAS      | PG     |
| WORDS HUNTER              | PG     |
| WORST BANGER              | PG     |
| YOUNG LANGUAGE            | G      |
+---------------------------+--------+
372 rows in set (0.00 sec)
```

Aunque esta cláusula `WHERE` no fue demasiado tediosa de generar, imagina si el conjunto de expresiones contuviera 10 o 20 miembros. Para estas situaciones, puedes usar el operador `IN` en su lugar:

```sql
SELECT title, rating
FROM film
WHERE rating IN ('G', 'PG');
```

Con el operador `IN`, puedes escribir una sola condición sin importar cuántas expresiones haya en el conjunto.

### Usando Subconsultas

Además de escribir tu propio conjunto de expresiones, como `('G', 'PG')`, puedes usar una subconsulta para generar un conjunto sobre la marcha. Por ejemplo, si asumes que cualquier película cuyo título incluya la cadena `'PET'` sería segura para ver en familia, podrías ejecutar una subconsulta contra la tabla `film` para recuperar todas las calificaciones asociadas con estas películas y luego recuperar todas las películas que tengan cualquiera de estas calificaciones:

```sql
mysql> SELECT title, rating
    -> FROM film
    -> WHERE rating IN (SELECT rating FROM film WHERE title LIKE '%PET%');
```

**Resultado**:

```sql
+---------------------------+--------+
| title                     | rating |
+---------------------------+--------+
| ACADEMY DINOSAUR          | PG     |
| ACE GOLDFINGER            | G      |
| AFFAIR PREJUDICE          | G      |
| AFRICAN EGG               | G      |
| AGENT TRUMAN              | PG     |
| ALAMO VIDEOTAPE           | G      |
| ALASKA PHANTOM            | PG     |
| ALI FOREVER               | PG     |
| AMADEUS HOLY              | PG     |
...
| WEDDING APOLLO            | PG     |
| WEREWOLF LOLA             | G      |
| WEST LION                 | G      |
| WIZARD COLDBLOODED        | PG     |
| WON DARES                 | PG     |
| WONDERLAND CHRISTMAS      | PG     |
| WORDS HUNTER              | PG     |
| WORST BANGER              | PG     |
| YOUNG LANGUAGE            | G      |
+---------------------------+--------+
372 rows in set (0.00 sec)
```

La subconsulta devuelve el conjunto `'G'` y `'PG'`, y la consulta principal verifica si el valor de la columna `rating` se encuentra en el conjunto devuelto por la subconsulta.

### Usando `NOT IN`

A veces, quieres ver si una expresión no existe dentro de un conjunto de expresiones. Para estas situaciones, puedes usar el operador `NOT IN`:

```sql
SELECT title, rating
FROM film
WHERE rating NOT IN ('PG-13', 'R', 'NC-17');
```

Esta consulta encuentra todas las películas que no tienen una calificación de `'PG-13'`, `'R'` o `'NC-17'`, lo que devolverá el mismo conjunto de 372 filas que las consultas anteriores.


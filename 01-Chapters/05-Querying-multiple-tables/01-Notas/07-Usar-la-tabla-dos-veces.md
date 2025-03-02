
---

# Usar la Misma Tabla Dos Veces

Si estás uniendo múltiples tablas, podrías encontrar que necesitas unir la misma tabla más de una vez. En la base de datos de muestra, por ejemplo, los actores están relacionados con las películas en las que aparecieron a través de la tabla `film_actor`. Si deseas encontrar todas las películas en las que aparecieron dos actores específicos, podrías escribir una consulta como esta, que une la tabla `film` con la tabla `film_actor` y luego con la tabla `actor`:

```sql
mysql> SELECT f.title
    -> FROM film f
    ->   INNER JOIN film_actor fa
    ->   ON f.film_id = fa.film_id
    ->   INNER JOIN actor a
    ->   ON fa.actor_id = a.actor_id
    -> WHERE ((a.first_name = 'CATE' AND a.last_name = 'MCQUEEN')
    ->     OR (a.first_name = 'CUBA' AND a.last_name = 'BIRCH'));
+----------------------+
| title                |
+----------------------+
| ATLANTIS CAUSE       |
| BLOOD ARGONAUTS      |
| COMMANDMENTS EXPRESS |
| DYNAMITE TARZAN      |
| EDGE KISSING         |
...
| TOWERS HURRICANE     |
| TROJAN TOMORROW      |
| VIRGIN DAISY         |
| VOLCANO TEXAS        |
| WATERSHIP FRONTIER   |
+----------------------+
54 rows in set (0.00 sec)
```

Esta consulta devuelve todas las películas en las que aparecieron Cate McQueen o Cuba Birch. Sin embargo, supongamos que deseas recuperar solo aquellas películas en las que ambos actores aparecieron. Para lograr esto, necesitarás encontrar todas las filas en la tabla `film` que tengan dos filas en la tabla `film_actor`, una de las cuales está asociada con Cate McQueen y la otra con Cuba Birch. Por lo tanto, necesitarás incluir las tablas `film_actor` y `actor` dos veces, cada una con un alias diferente para que el servidor sepa a cuál te refieres en las diversas cláusulas:

```sql
mysql> SELECT f.title
    ->  FROM film f
    ->    INNER JOIN film_actor fa1
    ->    ON f.film_id = fa1.film_id
    ->    INNER JOIN actor a1
    ->    ON fa1.actor_id = a1.actor_id
    ->    INNER JOIN film_actor fa2
    ->    ON f.film_id = fa2.film_id
    ->    INNER JOIN actor a2
    ->    ON fa2.actor_id = a2.actor_id
    -> WHERE (a1.first_name = 'CATE' AND a1.last_name = 'MCQUEEN')
    ->   AND (a2.first_name = 'CUBA' AND a2.last_name = 'BIRCH');
+------------------+
| title            |
+------------------+
| BLOOD ARGONAUTS  |
| TOWERS HURRICANE |
+------------------+
2 rows in set (0.00 sec)
```

Entre los dos, los actores aparecieron en 52 películas diferentes, pero solo hay dos películas en las que ambos actores aparecieron. Este es un ejemplo de una consulta que requiere el uso de alias de tabla, ya que las mismas tablas se usan múltiples veces.

---


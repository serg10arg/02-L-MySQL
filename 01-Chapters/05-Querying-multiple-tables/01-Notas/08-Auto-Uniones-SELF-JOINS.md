
---

# Auto-Uniones (Self-Joins)

No solo puedes incluir la misma tabla más de una vez en la misma consulta, sino que también puedes unir una tabla consigo misma. Esto puede parecer una cosa extraña de hacer al principio, pero hay razones válidas para hacerlo. Algunas tablas incluyen una clave externa autorreferencial, lo que significa que incluyen una columna que apunta a la clave primaria dentro de la misma tabla. Aunque la base de datos de ejemplo no incluye tal relación, imaginemos que la tabla `film` incluye la columna `prequel_film_id`, que apunta a la película de la que es precuela (por ejemplo, la película *Fiddler Lost II* usaría esta columna para apuntar a la película principal *Fiddler Lost*). Aquí está cómo se vería la tabla si añadiéramos esta columna adicional:

```sql
mysql> desc film;
+----------------------+-----------------------+------+-----+-------------------+
| Field                | Type                  | Null | Key | Default           |
+----------------------+-----------------------+------+-----+-------------------+
| film_id              | smallint(5) unsigned  | NO   | PRI | NULL              |
| title                | varchar(255)          | NO   | MUL | NULL              |
| description          | text                  | YES  |     | NULL              |
| release_year         | year(4)               | YES  |     | NULL              |
| language_id          | tinyint(3) unsigned   | NO   | MUL | NULL              |
| original_language_id | tinyint(3) unsigned   | YES  | MUL | NULL              |
| rental_duration      | tinyint(3) unsigned   | NO   |     | 3                 |
| rental_rate          | decimal(4,2)          | NO   |     | 4.99              |
| length               | smallint(5) unsigned  | YES  |     | NULL              |
| replacement_cost     | decimal(5,2)          | NO   |     | 19.99             |
| rating               | enum('G','PG','PG-13',
                           'R','NC-17')        | YES  |     | G                 |
| special_features     | set('Trailers',...,
                           'Behind the Scenes')| YES  |     | NULL              |
| last_update          | timestamp             | NO   |     | CURRENT_TIMESTAMP |
| prequel_film_id      | smallint(5) unsigned  | YES  | MUL | NULL              |
+----------------------+-----------------------+------+-----+-------------------+
```

Usando una auto-unión (self-join), puedes escribir una consulta que liste cada película que tenga una precuela, junto con el título de la precuela:

```sql
mysql> SELECT f.title, f_prnt.title AS prequel
    -> FROM film f
    ->   INNER JOIN film f_prnt
    ->   ON f_prnt.film_id = f.prequel_film_id
    -> WHERE f.prequel_film_id IS NOT NULL;
+-----------------+--------------+
| title           | prequel      |
+-----------------+--------------+
| FIDDLER LOST II | FIDDLER LOST |
+-----------------+--------------+
1 row in set (0.00 sec)
```

Esta consulta une la tabla `film` consigo misma usando la clave externa `prequel_film_id`, y se asignan los alias de tabla `f` y `f_prnt` para dejar claro qué tabla se usa para qué propósito.

---



---

# Operadores de Conjuntos

El lenguaje SQL incluye tres operadores de conjuntos que te permiten realizar cada una de las diversas operaciones de conjuntos descritas anteriormente en el capítulo. Además, cada operador de conjunto tiene dos variantes, una que incluye duplicados y otra que elimina duplicados (pero no necesariamente todos los duplicados). Las siguientes subsecciones definen cada operador y demuestran cómo se utilizan.

## El Operador `union`

Los operadores `union` y `union all` te permiten combinar múltiples conjuntos de datos. La diferencia entre los dos es que `union` ordena el conjunto combinado y elimina duplicados, mientras que `union all` no lo hace. Con `union all`, el número de filas en el conjunto de datos final será siempre igual a la suma del número de filas en los conjuntos que se están combinando. Esta operación es la operación de conjunto más simple de realizar (desde el punto de vista del servidor), ya que no es necesario que el servidor verifique la superposición de datos. El siguiente ejemplo demuestra cómo puedes usar el operador `union all` para generar un conjunto de nombres y apellidos de múltiples tablas:

```sql
mysql> SELECT 'CUST' typ, c.first_name, c.last_name
    -> FROM customer c
    -> UNION ALL
    -> SELECT 'ACTR' typ, a.first_name, a.last_name
    -> FROM actor a;
+------+------------+-------------+
| typ  | first_name | last_name   |
+------+------------+-------------+
| CUST | MARY       | SMITH       |
| CUST | PATRICIA   | JOHNSON     |
| CUST | LINDA      | WILLIAMS    |
| CUST | BARBARA    | JONES       |
| CUST | ELIZABETH  | BROWN       |
| CUST | JENNIFER   | DAVIS       |
| CUST | MARIA      | MILLER      |
| CUST | SUSAN      | WILSON      |
| CUST | MARGARET   | MOORE       |
| CUST | DOROTHY    | TAYLOR      |
| CUST | LISA       | ANDERSON    |
| CUST | NANCY      | THOMAS      |
| CUST | KAREN      | JACKSON     |
...
| ACTR | BURT       | TEMPLE      |
| ACTR | MERYL      | ALLEN       |
| ACTR | JAYNE      | SILVERSTONE |
| ACTR | BELA       | WALKEN      |
| ACTR | REESE      | WEST        |
| ACTR | MARY       | KEITEL      |
| ACTR | JULIA      | FAWCETT     |
| ACTR | THORA      | TEMPLE      |
+------+------------+-------------+
799 rows in set (0.00 sec)
```

La consulta devuelve 799 nombres, con 599 filas provenientes de la tabla `customer` y las otras 200 provenientes de la tabla `actor`. La primera columna, que tiene el alias `typ`, no es necesaria, pero se agregó para mostrar la fuente de cada nombre devuelto por la consulta.

Para enfatizar el punto de que el operador `union all` no elimina duplicados, aquí tienes otra versión del ejemplo anterior, pero con dos consultas idénticas contra la tabla `actor`:

```sql
mysql> SELECT 'ACTR' typ, a.first_name, a.last_name
    -> FROM actor a
    -> UNION ALL
    -> SELECT 'ACTR' typ, a.first_name, a.last_name
    -> FROM actor a;
+------+-------------+--------------+
| typ  | first_name  | last_name    |
+------+-------------+--------------+
| ACTR | PENELOPE    | GUINESS      |
| ACTR | NICK        | WAHLBERG     |
| ACTR | ED          | CHASE        |
| ACTR | JENNIFER    | DAVIS        |
| ACTR | JOHNNY      | LOLLOBRIGIDA |
| ACTR | BETTE       | NICHOLSON    |
| ACTR | GRACE       | MOSTEL       |
...
| ACTR | BURT        | TEMPLE       |
| ACTR | MERYL       | ALLEN        |
| ACTR | JAYNE       | SILVERSTONE  |
| ACTR | BELA        | WALKEN       |
| ACTR | REESE       | WEST         |
| ACTR | MARY        | KEITEL       |
| ACTR | JULIA       | FAWCETT      |
| ACTR | THORA       | TEMPLE       |
+------+-------------+--------------+
400 rows in set (0.00 sec)
```

Como puedes ver en los resultados, las 200 filas de la tabla `actor` se incluyen dos veces, para un total de 400 filas.

Aunque es poco probable que repitas la misma consulta dos veces en una consulta compuesta, aquí tienes otra consulta compuesta que devuelve datos duplicados:

```sql
mysql> SELECT c.first_name, c.last_name
    -> FROM customer c
    -> WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'
    -> UNION ALL
    -> SELECT a.first_name, a.last_name
    -> FROM actor a
    -> WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%';
+------------+-----------+
| first_name | last_name |
+------------+-----------+
| JENNIFER   | DAVIS     |
| JENNIFER   | DAVIS     |
| JUDY       | DEAN      |
| JODIE      | DEGENERES |
| JULIANNE   | DENCH     |
+------------+-----------+
5 rows in set (0.00 sec)
```

Ambas consultas devuelven los nombres de personas con las iniciales JD. De las cinco filas en el conjunto de resultados, una de ellas es un duplicado (Jennifer Davis). Si deseas que tu tabla combinada excluya las filas duplicadas, necesitas usar el operador `union` en lugar de `union all`:

```sql
mysql> SELECT c.first_name, c.last_name
    -> FROM customer c
    -> WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'
    -> UNION
    -> SELECT a.first_name, a.last_name
    -> FROM actor a
    -> WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%';
+------------+-----------+
| first_name | last_name |
+------------+-----------+
| JENNIFER   | DAVIS     |
| JUDY       | DEAN      |
| JODIE      | DEGENERES |
| JULIANNE   | DENCH     |
+------------+-----------+
4 rows in set (0.00 sec)
```

Para esta versión de la consulta, solo se incluyen los cuatro nombres distintos en el conjunto de resultados, en lugar de las cinco filas devueltas al usar `union all`.

---


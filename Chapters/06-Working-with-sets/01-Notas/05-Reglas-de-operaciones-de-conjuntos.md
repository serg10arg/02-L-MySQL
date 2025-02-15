
---

# Reglas de Operaciones de Conjuntos

Las siguientes secciones describen algunas reglas que debes seguir al trabajar con consultas compuestas.

## Ordenar Resultados de Consultas Compuestas

Si deseas que los resultados de tu consulta compuesta estén ordenados, puedes agregar una cláusula `order by` después de la última consulta. Al especificar nombres de columnas en la cláusula `order by`, deberás elegir entre los nombres de columnas de la primera consulta de la consulta compuesta. Con frecuencia, los nombres de las columnas son los mismos para ambas consultas en una consulta compuesta, pero esto no necesita ser el caso, como se demuestra a continuación:

```sql
mysql> SELECT a.first_name AS fname, a.last_name AS lname
    -> FROM actor a
    -> WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
    -> UNION ALL
    -> SELECT c.first_name, c.last_name
    -> FROM customer c
    -> WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'
    -> ORDER BY lname, fname;
+----------+-----------+
| fname    | lname     |
+----------+-----------+
| JENNIFER | DAVIS     |
| JENNIFER | DAVIS     |
| JUDY     | DEAN      |
| JODIE    | DEGENERES |
| JULIANNE | DENCH     |
+----------+-----------+
5 rows in set (0.00 sec)
```

Los nombres de las columnas especificados en las dos consultas son diferentes en este ejemplo. Si especificas un nombre de columna de la segunda consulta en tu cláusula `order by`, verás el siguiente error:

```sql
mysql> SELECT a.first_name AS fname, a.last_name AS lname
    -> FROM actor a
    -> WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
    -> UNION ALL
    -> SELECT c.first_name, c.last_name
    -> FROM customer c
    -> WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'
    -> ORDER BY last_name, first_name;
ERROR 1054 (42S22): Unknown column 'last_name' in 'order clause'
```

Recomiendo dar a las columnas en ambas consultas alias de columna idénticos para evitar este problema.

## Precedencia de Operaciones de Conjuntos

Si tu consulta compuesta contiene más de dos consultas que utilizan diferentes operadores de conjuntos, debes pensar en el orden en el que colocar las consultas en tu declaración compuesta para lograr los resultados deseados. Considera la siguiente declaración compuesta de tres consultas:

```sql
mysql> SELECT a.first_name, a.last_name
    -> FROM actor a
    -> WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
    -> UNION ALL
    -> SELECT a.first_name, a.last_name
    -> FROM actor a
    -> WHERE a.first_name LIKE 'M%' AND a.last_name LIKE 'T%'
    -> UNION
    -> SELECT c.first_name, c.last_name
    -> FROM customer c
    -> WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%';
+------------+-----------+
| first_name | last_name |
+------------+-----------+
| JENNIFER   | DAVIS     |
| JUDY       | DEAN      |
| JODIE      | DEGENERES |
| JULIANNE   | DENCH     |
| MARY       | TANDY     |
| MENA       | TEMPLE    |
+------------+-----------+
6 rows in set (0.00 sec)
```

Esta consulta compuesta incluye tres consultas que devuelven conjuntos de nombres no únicos; la primera y segunda consultas están separadas con el operador `union all`, mientras que la segunda y tercera consultas están separadas con el operador `union`. Aunque podría parecer que no hace mucha diferencia dónde se colocan los operadores `union` y `union all`, de hecho, sí hace una diferencia. Aquí está la misma consulta compuesta con los operadores de conjuntos invertidos:

```sql
mysql> SELECT a.first_name, a.last_name
    -> FROM actor a
    -> WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
    -> UNION
    -> SELECT a.first_name, a.last_name
    -> FROM actor a
    -> WHERE a.first_name LIKE 'M%' AND a.last_name LIKE 'T%'
    -> UNION ALL
    -> SELECT c.first_name, c.last_name
    -> FROM customer c
    -> WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%';
+------------+-----------+
| first_name | last_name |
+------------+-----------+
| JENNIFER   | DAVIS     |
| JUDY       | DEAN      |
| JODIE      | DEGENERES |
| JULIANNE   | DENCH     |
| MARY       | TANDY     |
| MENA       | TEMPLE    |
| JENNIFER   | DAVIS     |
+------------+-----------+
7 rows in set (0.00 sec)
```

Al observar los resultados, es obvio que sí hace una diferencia cómo se organiza la consulta compuesta al usar diferentes operadores de conjuntos. En general, las consultas compuestas que contienen tres o más consultas se evalúan en orden de arriba hacia abajo, pero con las siguientes advertencias:
- La especificación ANSI SQL establece que el operador `intersect` tiene precedencia sobre los otros operadores de conjuntos.
- Puedes dictar el orden en el que se combinan las consultas encerrando múltiples consultas entre paréntesis.

MySQL aún no permite paréntesis en consultas compuestas, pero si estás utilizando un servidor de base de datos diferente, puedes envolver las consultas adyacentes entre paréntesis para anular el procesamiento de arriba hacia abajo predeterminado de las consultas compuestas, como en:

```sql
SELECT a.first_name, a.last_name
FROM actor a
WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
UNION
(SELECT a.first_name, a.last_name
FROM actor a
WHERE a.first_name LIKE 'M%' AND a.last_name LIKE 'T%'
UNION ALL
SELECT c.first_name, c.last_name
FROM customer c
WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'
)
```

Para esta consulta compuesta, la segunda y tercera consultas se combinarían usando el operador `union all`, luego los resultados se combinarían con la primera consulta usando el operador `union`.

---


## Conceptos de Agrupación

A veces querrás encontrar tendencias en tus datos que requerirán que el servidor de la base de datos procese un poco los datos antes de que puedas generar los resultados que estás buscando. Por ejemplo, supongamos que estás a cargo de enviar cupones para alquileres gratuitos a tus mejores clientes. Podrías emitir una consulta simple para ver los datos sin procesar:

```sql
mysql> SELECT customer_id FROM rental;
+-------------+
| customer_id |
+-------------+
|           1 |
|           1 |
|           1 |
|           1 |
|           1 |
|           1 |
|           1 |
...
|         599 |
|         599 |
|         599 |
|         599 |
|         599 |
|         599 |
+-------------+
16044 rows in set (0.01 sec)
```

Con 599 clientes abarcando más de 16,000 registros de alquiler, no es factible determinar qué clientes han alquilado más películas mirando los datos sin procesar. En cambio, puedes pedirle al servidor de la base de datos que agrupe los datos por ti usando la cláusula `group by`. Aquí tienes la misma consulta, pero empleando una cláusula `group by` para agrupar los datos de alquiler por ID de cliente:

```sql
mysql> SELECT customer_id
    -> FROM rental
    -> GROUP BY customer_id;
+-------------+
| customer_id |
+-------------+
|           1 |
|           2 |
|           3 |
|           4 |
|           5 |
|           6 |
...
|         594 |
|         595 |
|         596 |
|         597 |
|         598 |
|         599 |
+-------------+
599 rows in set (0.00 sec)
```

El conjunto de resultados contiene una fila para cada valor distinto en la columna `customer_id`, resultando en 599 filas en lugar de las 16,044 filas completas. La razón del conjunto de resultados más pequeño es que algunos de los clientes alquilaron más de una película. Para ver cuántas películas alquiló cada cliente, puedes usar una función de agregado en la cláusula `select` para contar el número de filas en cada grupo:

```sql
mysql> SELECT customer_id, count(*)
    -> FROM rental
    -> GROUP BY customer_id;
+-------------+----------+
| customer_id | count(*) |
+-------------+----------+
|           1 |       32 |
|           2 |       27 |
|           3 |       26 |
|           4 |       22 |
|           5 |       38 |
|           6 |       28 |
...
|         594 |       27 |
|         595 |       30 |
|         596 |       28 |
|         597 |       25 |
|         598 |       22 |
|         599 |       19 |
+-------------+----------+
599 rows in set (0.01 sec)
```

La función de agregado `count()` cuenta el número de filas en cada grupo, y el asterisco le dice al servidor que cuente todo en el grupo. Usando la combinación de una cláusula `group by` y la función de agregado `count()`, puedes generar exactamente los datos necesarios para responder a la pregunta comercial sin tener que ver los datos sin procesar.

Mirando los resultados, puedes ver que el cliente con ID 1 alquiló 32 películas y el cliente con ID 597 alquiló 25 películas. Para determinar qué clientes han alquilado más películas, simplemente agrega una cláusula `order by`:

```sql
mysql> SELECT customer_id, count(*)
    -> FROM rental
    -> GROUP BY customer_id
    -> ORDER BY 2 DESC;
+-------------+----------+
| customer_id | count(*) |
+-------------+----------+
|         148 |       46 |
|         526 |       45 |
|         236 |       42 |
|         144 |       42 |
|          75 |       41 |
...
|         248 |       15 |
|         110 |       14 |
|         281 |       14 |
|          61 |       14 |
|         318 |       12 |
+-------------+----------+
599 rows in set (0.01 sec)
```

Ahora que los resultados están ordenados, puedes ver fácilmente que el cliente con ID 148 ha alquilado la mayor cantidad de películas (46), mientras que el cliente con ID 318 ha alquilado la menor cantidad de películas (12).

Al agrupar datos, es posible que necesites filtrar datos no deseados de tu conjunto de resultados basándote en grupos de datos en lugar de basándote en los datos sin procesar. Dado que la cláusula `group by` se ejecuta después de que se ha evaluado la cláusula `where`, no puedes agregar condiciones de filtro a tu cláusula `where` para este propósito. Por ejemplo, aquí tienes un intento de filtrar a cualquier cliente que haya alquilado menos de 40 películas:

```sql
mysql> SELECT customer_id, count(*)
    -> FROM rental
    -> WHERE count(*) >= 40
    -> GROUP BY customer_id;
ERROR 1111 (HY000): Invalid use of group function
```

No puedes referirte a la función de agregado `count(*)` en tu cláusula `where`, porque los grupos aún no se han generado en el momento en que se evalúa la cláusula `where`. En su lugar, debes poner tus condiciones de filtro de grupo en la cláusula `having`. Así es como se vería la consulta usando `having`:

```sql
mysql> SELECT customer_id, count(*)
    -> FROM rental
    -> GROUP BY customer_id
    -> HAVING count(*) >= 40;
+-------------+----------+
| customer_id | count(*) |
+-------------+----------+
|          75 |       41 |
|         144 |       42 |
|         148 |       46 |
|         197 |       40 |
|         236 |       42 |
|         469 |       40 |
|         526 |       45 |
+-------------+----------+
7 rows in set (0.01 sec)
```

Debido a que los grupos que contienen menos de 40 miembros han sido filtrados mediante la cláusula `having`, el conjunto de resultados ahora contiene solo aquellos clientes que han alquilado 40 o más películas.

---


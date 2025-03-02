### Cláusulas `GROUP BY` y `HAVING`

Hasta ahora, todas las consultas han recuperado datos en bruto sin manipulación. Sin embargo, a veces querrás encontrar tendencias en tus datos que requerirán que el servidor de la base de datos procese un poco los datos antes de recuperar el conjunto de resultados. Un mecanismo para esto es la cláusula `GROUP BY`, que se utiliza para agrupar datos por valores de columna.

#### Ejemplo de `GROUP BY` y `HAVING`

Supongamos que deseas encontrar todos los clientes que han alquilado 40 o más películas. En lugar de revisar las 16,044 filas de la tabla `rental`, puedes escribir una consulta que instruya al servidor a agrupar todos los alquileres por cliente, contar el número de alquileres para cada cliente y luego devolver solo aquellos clientes cuyo recuento de alquileres sea al menos 40. Al usar la cláusula `GROUP BY` para generar grupos de filas, también puedes usar la cláusula `HAVING`, que te permite filtrar datos agrupados de la misma manera que la cláusula `WHERE` filtra datos en bruto.

#### Consulta con `GROUP BY` y `HAVING`

```sql
mysql> SELECT c.first_name, c.last_name, COUNT(*)
    -> FROM customer c
    ->   INNER JOIN rental r
    ->   ON c.customer_id = r.customer_id
    -> GROUP BY c.first_name, c.last_name
    -> HAVING COUNT(*) >= 40;
```

**Resultado**:
```
+------------+-----------+----------+
| first_name | last_name | count(*) |
+------------+-----------+----------+
| TAMMY      | SANDERS   |       41 |
| CLARA      | SHAW      |       42 |
| ELEANOR    | HUNT      |       46 |
| SUE        | PETERS    |       40 |
| MARCIA     | DEAN      |       42 |
| WESLEY     | BULL      |       40 |
| KARL       | SEAL      |       45 |
+------------+-----------+----------+
7 rows in set (0.03 sec)
```

En esta consulta, la cláusula `GROUP BY` agrupa las filas por los nombres y apellidos de los clientes, y la cláusula `HAVING` filtra los resultados para incluir solo aquellos grupos donde el recuento de alquileres es al menos 40.

### Importancia de `GROUP BY` y `HAVING`

- **`GROUP BY`**: Utilizada para agrupar filas que comparten un valor común en una o más columnas.
- **`HAVING`**: Filtra los grupos de resultados generados por `GROUP BY`.

Aunque estas dos cláusulas son un poco más avanzadas que las otras cuatro cláusulas `SELECT`, es importante mencionarlas para que no te tomen por sorpresa más adelante en el libro. Para una descripción completa de cómo y cuándo usar `GROUP BY` y `HAVING`, te recomiendo revisar el Capítulo 8.


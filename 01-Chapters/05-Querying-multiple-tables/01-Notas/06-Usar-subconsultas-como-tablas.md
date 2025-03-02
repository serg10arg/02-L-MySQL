
---

# Usar Subconsultas como Tablas

Ya has visto varios ejemplos de consultas que incluyen múltiples tablas, pero hay una variación que vale la pena mencionar: qué hacer si algunos de los conjuntos de datos son generados por subconsultas. Las subconsultas son el enfoque del Capítulo 9, pero ya introduje el concepto de una subconsulta en la cláusula `from` en el capítulo anterior. La siguiente consulta une la tabla `customer` con una subconsulta contra las tablas `address` y `city`:

```sql
mysql> SELECT c.first_name, c.last_name, addr.address, addr.city
    -> FROM customer c
    ->   INNER JOIN
    ->    (SELECT a.address_id, a.address, ct.city
    ->     FROM address a
    ->       INNER JOIN city ct
    ->       ON a.city_id = ct.city_id
    ->     WHERE a.district = 'California'
    ->    ) addr
    ->   ON c.address_id = addr.address_id;
+------------+-----------+------------------------+----------------+
| first_name | last_name | address                | city           |
+------------+-----------+------------------------+----------------+
| PATRICIA   | JOHNSON   | 1121 Loja Avenue       | San Bernardino |
| BETTY      | WHITE     | 770 Bydgoszcz Avenue   | Citrus Heights |
| ALICE      | STEWART   | 1135 Izumisano Parkway | Fontana        |
| ROSA       | REYNOLDS  | 793 Cam Ranh Avenue    | Lancaster      |
| RENEE      | LANE      | 533 al-Ayn Boulevard   | Compton        |
| KRISTIN    | JOHNSTON  | 226 Brest Manor        | Sunnyvale      |
| CASSANDRA  | WALTERS   | 920 Kumbakonam Loop    | Salinas        |
| JACOB      | LANCE     | 1866 al-Qatif Avenue   | El Monte       |
| RENE       | MCALISTER | 1895 Zhezqazghan Drive | Garden Grove   |
+------------+-----------+------------------------+----------------+
9 rows in set (0.00 sec)
```

La subconsulta, que comienza en la línea 4 y se le da el alias `addr`, encuentra todas las direcciones que están en California. La consulta externa une los resultados de la subconsulta con la tabla `customer` para devolver el nombre, apellido, dirección y ciudad de todos los clientes que viven en California. Aunque esta consulta podría haberse escrito sin el uso de una subconsulta simplemente uniendo las tres tablas, a veces puede ser ventajoso desde el punto de vista del rendimiento y/o la legibilidad usar una o más subconsultas.

Una forma de visualizar lo que está sucediendo es ejecutar la subconsulta por sí sola y mirar los resultados. Aquí están los resultados de la subconsulta del ejemplo anterior:

```sql
mysql> SELECT a.address_id, a.address, ct.city
    -> FROM address a
    ->   INNER JOIN city ct
    ->   ON a.city_id = ct.city_id
    -> WHERE a.district = 'California';
+------------+------------------------+----------------+
| address_id | address                | city           |
+------------+------------------------+----------------+
|          6 | 1121 Loja Avenue       | San Bernardino |
|         18 | 770 Bydgoszcz Avenue   | Citrus Heights |
|         55 | 1135 Izumisano Parkway | Fontana        |
|        116 | 793 Cam Ranh Avenue    | Lancaster      |
|        186 | 533 al-Ayn Boulevard   | Compton        |
|        218 | 226 Brest Manor        | Sunnyvale      |
|        274 | 920 Kumbakonam Loop    | Salinas        |
|        425 | 1866 al-Qatif Avenue   | El Monte       |
|        599 | 1895 Zhezqazghan Drive | Garden Grove   |
+------------+------------------------+----------------+
9 rows in set (0.00 sec)
```

Este conjunto de resultados consiste en las nueve direcciones de California. Cuando se une a la tabla `customer` a través de la columna `address_id`, tu conjunto de resultados contendrá información sobre los clientes asignados a estas direcciones.

---


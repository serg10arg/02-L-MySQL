
---

# Teoría de Conjuntos en la Práctica

Los círculos utilizados en los diagramas de la sección anterior para representar conjuntos de datos no transmiten nada sobre la composición de los conjuntos de datos. Sin embargo, al tratar con datos reales, es necesario describir la composición de los conjuntos de datos involucrados si se van a combinar. Imagina, por ejemplo, qué sucedería si intentaras generar la unión de la tabla `customer` y la tabla `city`, cuyas definiciones son las siguientes:

```sql
mysql> desc customer;
+-------------+----------------------+------+-----+-------------------+
| Field       | Type                 | Null | Key | Default           |
+-------------+----------------------+------+-----+-------------------+
| customer_id | smallint(5) unsigned | NO   | PRI | NULL              |
| store_id    | tinyint(3) unsigned  | NO   | MUL | NULL              |
| first_name  | varchar(45)          | NO   |     | NULL              |
| last_name   | varchar(45)          | NO   | MUL | NULL              |
| email       | varchar(50)          | YES  |     | NULL              |
| address_id  | smallint(5) unsigned | NO   | MUL | NULL              |
| active      | tinyint(1)           | NO   |     | 1                 |
| create_date | datetime             | NO   |     | NULL              |
| last_update | timestamp            | YES  |     | CURRENT_TIMESTAMP |
+-------------+----------------------+------+-----+-------------------+

mysql> desc city;
+-------------+----------------------+------+-----+-------------------+
| Field       | Type                 | Null | Key | Default           |
+-------------+----------------------+------+-----+-------------------+
| city_id     | smallint(5) unsigned | NO   | PRI | NULL              |
| city        | varchar(50)          | NO   |     | NULL              |
| country_id  | smallint(5) unsigned | NO   | MUL | NULL              |
| last_update | timestamp            | NO   |     | CURRENT_TIMESTAMP |
+-------------+----------------------+------+-----+-------------------+
```

Cuando se combinan, la primera columna en el conjunto de resultados incluiría tanto las columnas `customer.customer_id` como `city.city_id`, la segunda columna sería la combinación de las columnas `customer.store_id` y `city.city`, y así sucesivamente. Mientras que algunos pares de columnas son fáciles de combinar (por ejemplo, dos columnas numéricas), no está claro cómo se deben combinar otros pares de columnas, como una columna numérica con una columna de cadena o una columna de cadena con una columna de fecha. Además, las columnas de la quinta a la novena de las tablas combinadas incluirían datos solo de las columnas de la quinta a la novena de la tabla `customer`, ya que la tabla `city` solo tiene cuatro columnas. Claramente, debe haber alguna comunalidad entre los dos conjuntos de datos que deseas combinar.

Por lo tanto, cuando realices operaciones de conjuntos en dos conjuntos de datos, deben aplicarse las siguientes pautas:
- Ambos conjuntos de datos deben tener el mismo número de columnas.
- Los tipos de datos de cada columna en los dos conjuntos de datos deben ser los mismos (o el servidor debe ser capaz de convertir uno al otro).

Con estas reglas en su lugar, es más fácil visualizar lo que significa "datos superpuestos" en la práctica; cada par de columnas de los dos conjuntos combinados debe contener la misma cadena, número o fecha para que las filas en las dos tablas se consideren iguales.

Realizas una operación de conjuntos colocando un operador de conjuntos entre dos declaraciones `select`, como se demuestra en el siguiente ejemplo:

```sql
mysql> SELECT 1 num, 'abc' str
    -> UNION
    -> SELECT 9 num, 'xyz' str;
+-----+-----+
| num | str |
+-----+-----+
|   1 | abc |
|   9 | xyz |
+-----+-----+
2 rows in set (0.02 sec)
```

Cada una de las consultas individuales produce un conjunto de datos que consta de una sola fila con una columna numérica y una columna de cadena. El operador de conjuntos, que en este caso es `union`, le dice al servidor de la base de datos que combine todas las filas de los dos conjuntos. Por lo tanto, el conjunto final incluye dos filas de dos columnas. Esta consulta se conoce como una consulta compuesta porque comprende múltiples consultas independientes. Como verás más adelante, las consultas compuestas pueden incluir más de dos consultas si se necesitan múltiples operaciones de conjuntos para obtener los resultados finales.

---


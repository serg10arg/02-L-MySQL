
---

# Unir Tres o Más Tablas

Unir tres tablas es similar a unir dos tablas, pero con una pequeña diferencia. Con una unión de dos tablas, hay dos tablas y un tipo de unión en la cláusula `from`, y una sola subcláusula `on` para definir cómo se unen las tablas. Con una unión de tres tablas, hay tres tablas y dos tipos de unión en la cláusula `from`, y dos subcláusulas `on`.

Para ilustrarlo, cambiemos la consulta anterior para devolver la ciudad del cliente en lugar de su dirección. El nombre de la ciudad, sin embargo, no se almacena en la tabla `address` sino que se accede a través de una clave externa a la tabla `city`. Aquí están las definiciones de las tablas:

```sql
mysql> desc address;
+-------------+----------------------+------+-----+-------------------+
| Field       | Type                 | Null | Key | Default           |
+-------------+----------------------+------+-----+-------------------+
| address_id  | smallint(5) unsigned | NO   | PRI | NULL              |
| address     | varchar(50)          | NO   |     | NULL              |
| address2    | varchar(50)          | YES  |     | NULL              |
| district    | varchar(20)          | NO   |     | NULL              |
| city_id     | smallint(5) unsigned | NO   | MUL | NULL              |
| postal_code | varchar(10)          | YES  |     | NULL              |
| phone       | varchar(20)          | NO   |     | NULL              |
| location    | geometry             | NO   | MUL | NULL              |
| last_update | timestamp            | NO   |     | CURRENT_TIMESTAMP |
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

Para mostrar la ciudad de cada cliente, necesitarás recorrer desde la tabla `customer` a la tabla `address` utilizando la columna `address_id` y luego desde la tabla `address` a la tabla `city` utilizando la columna `city_id`. La consulta sería como la siguiente:

```sql
mysql> SELECT c.first_name, c.last_name, ct.city
    -> FROM customer c
    ->   INNER JOIN address a
    ->   ON c.address_id = a.address_id
    ->   INNER JOIN city ct
    ->   ON a.city_id = ct.city_id;
+-------------+--------------+----------------------------+
| first_name  | last_name    | city                       |
+-------------+--------------+----------------------------+
| JULIE       | SANCHEZ      | A Corua (La Corua)         |
| PEGGY       | MYERS        | Abha                       |
| TOM         | MILNER       | Abu Dhabi                  |
| GLEN        | TALBERT      | Acua                       |
| LARRY       | THRASHER     | Adana                      |
| SEAN        | DOUGLASS     | Addis Abeba                |
...
| MICHELE     | GRANT        | Yuncheng                   |
| GARY        | COY          | Yuzhou                     |
| PHYLLIS     | FOSTER       | Zalantun                   |
| CHARLENE    | ALVAREZ      | Zanzibar                   |
| FRANKLIN    | TROUTMAN     | Zaoyang                    |
| FLOYD       | GANDY        | Zapopan                    |
| CONSTANCE   | REID         | Zaria                      |
| JACK        | FOUST        | Zeleznogorsk               |
| BYRON       | BOX          | Zhezqazghan                |
| GUY         | BROWNLEE     | Zhoushan                   |
| RONNIE      | RICKETTS     | Ziguinchor                 |
+-------------+--------------+----------------------------+
599 rows in set (0.03 sec)
```

Para esta consulta, hay tres tablas, dos tipos de unión y dos subcláusulas `on` en la cláusula `from`, así que las cosas se han vuelto un poco más ocupadas. A primera vista, podría parecer que el orden en que aparecen las tablas en la cláusula `from` es importante, pero si cambias el orden de las tablas, obtendrás los mismos resultados exactos. Todas estas tres variaciones devuelven los mismos resultados:

```sql
SELECT c.first_name, c.last_name, ct.city
FROM customer c
  INNER JOIN address a
  ON c.address_id = a.address_id
  INNER JOIN city ct
  ON a.city_id = ct.city_id;

SELECT c.first_name, c.last_name, ct.city
FROM city ct
  INNER JOIN address a
  ON a.city_id = ct.city_id
  INNER JOIN customer c
  ON c.address_id = a.address_id;

SELECT c.first_name, c.last_name, ct.city
FROM address a
  INNER JOIN city ct
  ON a.city_id = ct.city_id
  INNER JOIN customer c
  ON c.address_id = a.address_id;
```

La única diferencia que podrías ver sería el orden en que se devuelven las filas, ya que no hay una cláusula `order by` para especificar cómo deben ordenarse los resultados.

# ¿Importa el Orden de Unión?

Si estás confundido acerca de por qué las tres versiones de la consulta `customer/address/city` arrojan los mismos resultados, ten en cuenta que SQL es un lenguaje no procedural, lo que significa que describes lo que quieres recuperar y qué objetos de base de datos deben estar involucrados, pero depende del servidor de base de datos determinar la mejor manera de ejecutar tu consulta. Usando estadísticas recopiladas de tus objetos de base de datos, el servidor debe elegir una de las tres tablas como punto de partida (la tabla elegida se conoce en adelante como la tabla de conducción) y luego decidir en qué orden unir las tablas restantes. Por lo tanto, el orden en que aparecen las tablas en tu cláusula `from` no es significativo.

Sin embargo, si crees que las tablas en tu consulta siempre deben unirse en un orden particular, puedes colocar las tablas en el orden deseado y luego especificar la palabra clave `straight_join` en MySQL, solicitar la opción `force order` en SQL Server o usar las pistas de optimización `ordered` o `leading` en Oracle Database. Por ejemplo, para decirle al servidor MySQL que use la tabla `city` como la tabla de conducción y luego una las tablas `address` y `customer`, podrías hacer lo siguiente:

```sql
SELECT STRAIGHT_JOIN c.first_name, c.last_name, ct.city
FROM city ct
  INNER JOIN address a
  ON a.city_id = ct.city_id
  INNER JOIN customer c
  ON c.address_id = a.address_id;
```

---


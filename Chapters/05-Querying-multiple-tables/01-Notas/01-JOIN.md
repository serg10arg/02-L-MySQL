
---

# Join

Las consultas a una sola tabla no son raras, pero descubrirás que la mayoría de tus consultas requerirán dos, tres o incluso más tablas. Para ilustrarlo, veamos las definiciones de las tablas `cliente` y `dirección` y luego definimos una consulta que recupere datos de ambas tablas:

```sql
mysql> desc cliente;
+-------------+----------------------+------+-----+-------------------+
| Campo       | Tipo                 | Nulo | Clave | Predeterminado    |
+-------------+----------------------+------+-----+-------------------+
| id_cliente  | smallint(5) unsigned | NO   | PRI | NULL              |
| id_tienda   | tinyint(3) unsigned  | NO   | MUL | NULL              |
| nombre      | varchar(45)          | NO   |     | NULL              |
| apellido    | varchar(45)          | NO   | MUL | NULL              |
| correo      | varchar(50)          | YES  |     | NULL              |
| id_direccion| smallint(5) unsigned | NO   | MUL | NULL              |
| activo      | tinyint(1)           | NO   |     | 1                 |
| fecha_creacion | datetime          | NO   |     | NULL              |
| ultima_actualizacion | timestamp   | YES  |     | CURRENT_TIMESTAMP |
+-------------+----------------------+------+-----+-------------------+
mysql> desc dirección;
+-------------+----------------------+------+-----+-------------------+
| Campo       | Tipo                 | Nulo | Clave | Predeterminado    |
+-------------+----------------------+------+-----+-------------------+
| id_direccion| smallint(5) unsigned | NO   | PRI | NULL              |
| dirección   | varchar(50)          | NO   |     | NULL              |
| dirección2  | varchar(50)          | YES  |     | NULL              |
| distrito    | varchar(20)          | NO   |     | NULL              |
| id_ciudad   | smallint(5) unsigned | NO   | MUL | NULL              |
| código_postal | varchar(10)        | YES  |     | NULL              |
| teléfono    | varchar(20)          | NO   |     | NULL              |
| ubicación   | geometry             | NO   | MUL | NULL              |
| ultima_actualizacion | timestamp   | NO   |     | CURRENT_TIMESTAMP |
+-------------+----------------------+------+-----+-------------------+
```

Supongamos que deseas recuperar los nombres y apellidos de cada cliente, junto con su dirección. Tu consulta, por lo tanto, deberá recuperar los campos `cliente.nombre`, `cliente.apellido` y `dirección.dirección`. ¿Pero cómo puedes recuperar datos de ambas tablas en la misma consulta? La respuesta radica en la columna `cliente.id_direccion`, que contiene el ID del registro del cliente en la tabla `dirección` (en términos más formales, la columna `cliente.id_direccion` es la clave externa para la tabla `dirección`). La consulta, que verás en breve, instruye al servidor a usar la columna `cliente.id_direccion` como el nexo entre las tablas `cliente` y `dirección`, permitiendo así que las columnas de ambas tablas se incluyan en el conjunto de resultados de la consulta. Este tipo de operación se conoce como una unión.

Una restricción de clave externa puede crearse opcionalmente para verificar que los valores en una tabla existan en otra tabla. Para el ejemplo anterior, una restricción de clave externa podría crearse en la tabla `cliente` para asegurar que cualquier valor insertado en la columna `cliente.id_direccion` se pueda encontrar en la columna `dirección.id_direccion`. Ten en cuenta que no es necesario tener una restricción de clave externa para unir dos tablas.

---


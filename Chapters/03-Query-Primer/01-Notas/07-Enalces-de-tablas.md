### Enlaces de Tablas

Al usar múltiples tablas en la cláusula `FROM`, es importante incluir las condiciones utilizadas para enlazar las tablas. Este método está aprobado por ANSI y es el más portátil entre los diversos servidores de bases de datos.

### Ejemplo de Unión de Tablas

```sql
mysql> SELECT customer.first_name, customer.last_name,
    ->   time(rental.rental_date) rental_time
    -> FROM customer
    ->   INNER JOIN rental
    ->   ON customer.customer_id = rental.customer_id
    -> WHERE date(rental.rental_date) = '2005-06-14';
```

**Resultado**:
```
+------------+-----------+-------------+
| first_name | last_name | rental_time |
+------------+-----------+-------------+
| JEFFERY    | PINSON    | 22:53:33    |
| ELMER      | NOE       | 22:55:13    |
| MINNIE     | ROMERO    | 23:00:34    |
| MIRIAM     | MCKINNEY  | 23:07:08    |
| DANIEL     | CABRAL    | 23:09:38    |
| TERRANCE   | ROUSH     | 23:12:46    |
| JOYCE      | EDWARDS   | 23:16:26    |
| GWENDOLYN  | MAY       | 23:16:27    |
| CATHERINE  | CAMPBELL  | 23:17:03    |
| MATTHEW    | MAHAN     | 23:25:58    |
| HERMAN     | DEVORE    | 23:35:09    |
| AMBER      | DIXON     | 23:42:56    |
| TERRENCE   | GUNDERSON | 23:47:35    |
| SONIA      | GREGORY   | 23:50:11    |
| CHARLES    | KOWALSKI  | 23:54:34    |
| JEANETTE   | GREENE    | 23:54:46    |
+------------+-----------+-------------+
16 rows in set (0.01 sec)
```

Esta consulta muestra datos tanto de la tabla `customer` (first_name, last_name) como de la tabla `rental` (rental_date), por lo que ambas tablas se incluyen en la cláusula `FROM`. El mecanismo para enlazar las dos tablas (conocido como una unión) es el ID de cliente almacenado en ambas tablas. Las condiciones de unión se encuentran en la subcláusula `ON` de la cláusula `FROM`:

```sql
ON customer.customer_id = rental.customer_id
```

La cláusula `WHERE` no forma parte de la unión y solo se incluye para mantener el conjunto de resultados pequeño, ya que hay más de 16,000 filas en la tabla `rental`.

### Definiendo Alias de Tablas

Cuando se unen múltiples tablas en una sola consulta, necesitas una forma de identificar a qué tabla te refieres al mencionar columnas en las cláusulas `SELECT`, `WHERE`, `GROUP BY`, `HAVING` y `ORDER BY`. Tienes dos opciones:

1. **Usar el nombre completo de la tabla**:
   ```sql
   SELECT customer.first_name, customer.last_name
   FROM customer
   INNER JOIN rental
   ON customer.customer_id = rental.customer_id;
   ```

2. **Asignar un alias a cada tabla y usar el alias en la consulta**:
   ```sql
   SELECT c.first_name, c.last_name,
       time(r.rental_date) rental_time
   FROM customer c
   INNER JOIN rental r
   ON c.customer_id = r.customer_id
   WHERE date(r.rental_date) = '2005-06-14';
   ```

### Uso de la Palabra Clave `AS`

También puedes utilizar la palabra clave `AS` con tus alias de tabla para mejorar la legibilidad:

```sql
SELECT c.first_name, c.last_name,
    time(r.rental_date) rental_time
FROM customer AS c
INNER JOIN rental AS r
ON c.customer_id = r.customer_id
WHERE date(r.rental_date) = '2005-06-14';
```

Usar alias hace que la declaración sea más compacta sin causar confusión, siempre y cuando los nombres de alias sean razonables.


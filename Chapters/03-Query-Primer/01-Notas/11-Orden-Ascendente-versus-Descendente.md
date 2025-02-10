### Orden Ascendente versus Descendente

Cuando ordenas los resultados de una consulta, tienes la opción de especificar el orden ascendente o descendente mediante las palabras clave `ASC` y `DESC`. El orden predeterminado es ascendente, por lo que necesitarás agregar la palabra clave `DESC` si deseas un orden descendente.

#### Ejemplo de Orden Descendente

La siguiente consulta muestra a todos los clientes que alquilaron películas el 14 de junio de 2005, en orden descendente de tiempo de alquiler:

```sql
mysql> SELECT c.first_name, c.last_name,
    ->   time(r.rental_date) rental_time
    -> FROM customer c
    ->   INNER JOIN rental r
    ->   ON c.customer_id = r.customer_id
    -> WHERE date(r.rental_date) = '2005-06-14'
    -> ORDER BY time(r.rental_date) DESC;
```

**Resultado**:
```
+------------+-----------+-------------+
| first_name | last_name | rental_time |
+------------+-----------+-------------+
| JEANETTE   | GREENE    | 23:54:46    |
| CHARLES    | KOWALSKI  | 23:54:34    |
| SONIA      | GREGORY   | 23:50:11    |
| TERRENCE   | GUNDERSON | 23:47:35    |
| AMBER      | DIXON     | 23:42:56    |
| HERMAN     | DEVORE    | 23:35:09    |
| MATTHEW    | MAHAN     | 23:25:58    |
| CATHERINE  | CAMPBELL  | 23:17:03    |
| GWENDOLYN  | MAY       | 23:16:27    |
| JOYCE      | EDWARDS   | 23:16:26    |
| TERRANCE   | ROUSH     | 23:12:46    |
| DANIEL     | CABRAL    | 23:09:38    |
| MIRIAM     | MCKINNEY  | 23:07:08    |
| MINNIE     | ROMERO    | 23:00:34    |
| ELMER      | NOE       | 22:55:13    |
| JEFFERY    | PINSON    | 22:53:33    |
+------------+-----------+-------------+
16 rows in set (0.01 sec)
```

### Uso Común del Orden Descendente

Los ordenamientos en orden descendente se utilizan comúnmente para consultas de clasificación, como "muéstrame los cinco saldos de cuenta más altos". MySQL incluye una cláusula `LIMIT` que permite ordenar tus datos y luego descartar todas las filas excepto las primeras X.

#### Ejemplo de Orden Ascendente y Descendente

Para ordenamientos más complejos, puedes combinar ambos órdenes. Por ejemplo, puedes ordenar primero por apellido en orden ascendente y luego por nombre en orden descendente:

```sql
mysql> SELECT c.first_name, c.last_name,
    ->   time(r.rental_date) rental_time
    -> FROM customer c
    ->   INNER JOIN rental r
    ->   ON c.customer_id = r.customer_id
    -> WHERE date(r.rental_date) = '2005-06-14'
    -> ORDER BY c.last_name ASC, c.first_name DESC;
```


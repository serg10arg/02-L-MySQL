### Ordenamiento mediante Posiciones Numéricas

Cuando ordenas utilizando las columnas en tu cláusula `SELECT`, puedes optar por referenciar las columnas por su posición en la cláusula `SELECT` en lugar de por nombre. Esto puede ser especialmente útil si estás ordenando una expresión.

### Ejemplo de Uso de Posiciones Numéricas

Aquí tienes el ejemplo anterior una vez más, con una cláusula `ORDER BY` que especifica un orden descendente utilizando el tercer elemento en la cláusula `SELECT`:

```sql
mysql> SELECT c.first_name, c.last_name,
    ->   time(r.rental_date) rental_time
    -> FROM customer c
    ->   INNER JOIN rental r
    ->   ON c.customer_id = r.customer_id
    -> WHERE date(r.rental_date) = '2005-06-14'
    -> ORDER BY 3 DESC;
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

### Precauciones al Usar Posiciones Numéricas

Usa esta característica con moderación, ya que agregar una columna a la cláusula `SELECT` sin cambiar los números en la cláusula `ORDER BY` puede llevar a resultados inesperados. Personalmente, puedo referenciar columnas posicionalmente cuando escribo consultas ad hoc, pero siempre referencio columnas por nombre cuando escribo código.

### Conclusión

Referenciar columnas por su posición numérica en la cláusula `SELECT` puede ser útil para ordenamientos rápidos, pero debes tener cuidado para evitar errores al modificar la consulta. Utiliza esta técnica sabiamente y considera referenciar las columnas por nombre para una mayor claridad y mantenimiento en tu código.


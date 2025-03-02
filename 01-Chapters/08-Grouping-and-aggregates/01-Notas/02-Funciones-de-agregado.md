### Funciones de Agregado

Las funciones de agregado realizan una operación específica sobre todas las filas en un grupo. Aunque cada servidor de base de datos tiene su propio conjunto de funciones de agregado especializadas, las funciones de agregado comunes implementadas por todos los servidores principales incluyen:

- **max()**: Devuelve el valor máximo dentro de un conjunto.
- **min()**: Devuelve el valor mínimo dentro de un conjunto.
- **avg()**: Devuelve el valor promedio en un conjunto.
- **sum()**: Devuelve la suma de los valores en un conjunto.
- **count()**: Devuelve el número de valores en un conjunto.

Aquí tienes una consulta que usa todas las funciones de agregado comunes para analizar los datos sobre los pagos de alquiler de películas:

```sql
mysql> SELECT MAX(amount) max_amt,
    ->   MIN(amount) min_amt,
    ->   AVG(amount) avg_amt,
    ->   SUM(amount) tot_amt,
    ->   COUNT(*) num_payments
    -> FROM payment;
+---------+---------+----------+----------+--------------+
| max_amt | min_amt | avg_amt  | tot_amt  | num_payments |
+---------+---------+----------+----------+--------------+
|   11.99 |    0.00 | 4.200667 | 67416.51 |        16049 |
+---------+---------+----------+----------+--------------+
1 row in set (0.09 sec)
```

Los resultados de esta consulta te dicen que, en las 16,049 filas de la tabla `payment`, el monto máximo pagado para alquilar una película fue de $11.99, el monto mínimo fue de $0, el pago promedio fue de $4.20 y el total de todos los pagos de alquiler fue de $67,416.51. Con suerte, esto te da una apreciación por el papel de estas funciones de agregado; las siguientes subsecciones aclaran aún más cómo puedes utilizar estas funciones.

---


### Condiciones de Rango en SQL

Además de comprobar si una expresión es igual a (o no es igual a) otra expresión, puedes construir condiciones que verifiquen si una expresión se encuentra dentro de un cierto rango. Este tipo de condición es común cuando se trabaja con datos numéricos o temporales.

#### Ejemplo de Consulta con Límite Superior

```sql
mysql> SELECT customer_id, rental_date
    -> FROM rental
    -> WHERE rental_date < '2005-05-25';
```

**Resultado**:

```sql
+-------------+---------------------+
| customer_id | rental_date         |
+-------------+---------------------+
|         130 | 2005-05-24 22:53:30 |
|         459 | 2005-05-24 22:54:33 |
|         408 | 2005-05-24 23:03:39 |
|         333 | 2005-05-24 23:04:41 |
|         222 | 2005-05-24 23:05:21 |
|         549 | 2005-05-24 23:08:07 |
|         269 | 2005-05-24 23:11:53 |
|         239 | 2005-05-24 23:31:46 |
+-------------+---------------------+
8 rows in set (0.00 sec)
```

Esta consulta encuentra todos los alquileres de películas anteriores al 25 de mayo de 2005.

#### Ejemplo de Consulta con Límite Inferior y Superior

```sql
mysql> SELECT customer_id, rental_date
    -> FROM rental
    -> WHERE rental_date <= '2005-06-16'
    ->   AND rental_date >= '2005-06-14';
```

**Resultado**:

```sql
+-------------+---------------------+
| customer_id | rental_date         |
+-------------+---------------------+
|         416 | 2005-06-14 22:53:33 |
|         516 | 2005-06-14 22:55:13 |
|         239 | 2005-06-14 23:00:34 |
|         285 | 2005-06-14 23:07:08 |
|         310 | 2005-06-14 23:09:38 |
|         592 | 2005-06-14 23:12:46 |
...
|         148 | 2005-06-15 23:20:26 |
|         237 | 2005-06-15 23:36:37 |
|         155 | 2005-06-15 23:55:27 |
|         341 | 2005-06-15 23:57:20 |
|         149 | 2005-06-15 23:58:53 |
+-------------+---------------------+
364 rows in set (0.00 sec)
```

Esta versión de la consulta recupera todas las películas alquiladas el 14 o 15 de junio de 2005.

### El Operador `BETWEEN`

Cuando tienes tanto un límite superior como inferior para tu rango, puedes optar por usar una sola condición que utilice el operador `BETWEEN` en lugar de usar dos condiciones separadas.

#### Ejemplo con `BETWEEN`

```sql
mysql> SELECT customer_id, rental_date
    -> FROM rental
    -> WHERE rental_date BETWEEN '2005-06-14' AND '2005-06-16';
```

**Resultado**:

```sql
+-------------+---------------------+
| customer_id | rental_date         |
+-------------+---------------------+
|         416 | 2005-06-14 22:53:33 |
|         516 | 2005-06-14 22:55:13 |
|         239 | 2005-06-14 23:00:34 |
|         285 | 2005-06-14 23:07:08 |
|         310 | 2005-06-14 23:09:38 |
|         592 | 2005-06-14 23:12:46 |
...
|         148 | 2005-06-15 23:20:26 |
|         237 | 2005-06-15 23:36:37 |
|         155 | 2005-06-15 23:55:27 |
|         341 | 2005-06-15 23:57:20 |
|         149 | 2005-06-15 23:58:53 |
+-------------+---------------------+
364 rows in set (0.00 sec)
```

### Consideraciones al Usar `BETWEEN`

1. **Orden de los Límites**:
    - Siempre debes especificar primero el límite inferior (después de `BETWEEN`) y el límite superior segundo (después de `AND`).
    - Ejemplo incorrecto: `BETWEEN '2005-06-16' AND '2005-06-14'` no devolverá datos porque el rango es inválido.

2. **Inclusividad**:
    - Los límites superior e inferior son inclusivos, lo que significa que los valores que proporcionas se incluyen en los límites del rango.
    - Ejemplo: `BETWEEN '2005-06-14' AND '2005-06-16'` incluye las fechas 14 y 15 de junio de 2005.

### Condiciones de Rango Numérico

También puedes construir condiciones para especificar rangos de números.

#### Ejemplo de Rango Numérico

```sql
mysql> SELECT customer_id, payment_date, amount
    -> FROM payment
    -> WHERE amount BETWEEN 10.0 AND 11.99;
```

**Resultado**:

```sql
+-------------+---------------------+--------+
| customer_id | payment_date        | amount |
+-------------+---------------------+--------+
|           2 | 2005-07-30 13:47:43 |  10.99 |
|           3 | 2005-07-27 20:23:12 |  10.99 |
|          12 | 2005-08-01 06:50:26 |  10.99 |
|          13 | 2005-07-29 22:37:41 |  11.99 |
|          21 | 2005-06-21 01:04:35 |  10.99 |
|          29 | 2005-07-09 21:55:19 |  10.99 |
...
|         571 | 2005-06-20 08:15:27 |  10.99 |
|         572 | 2005-06-17 04:05:12 |  10.99 |
|         573 | 2005-07-31 12:14:19 |  10.99 |
|         591 | 2005-07-07 20:45:51 |  11.99 |
|         592 | 2005-07-06 22:58:31 |  11.99 |
|         595 | 2005-07-31 11:51:46 |  10.99 |
+-------------+---------------------+--------+
114 rows in set (0.01 sec)
```

### Condiciones de Rango de Cadenas

Aunque los rangos de fechas y números son fáciles de entender, también puedes construir condiciones que busquen rangos de cadenas.

#### Ejemplo de Rango de Cadenas

```sql
mysql> SELECT last_name, first_name
    -> FROM customer
    -> WHERE last_name BETWEEN 'FA' AND 'FR';
```

**Resultado**:

```sql
+------------+------------+
| last_name  | first_name |
+------------+------------+
| FARNSWORTH | JOHN       |
| FENNELL    | ALEXANDER  |
| FERGUSON   | BERTHA     |
| FERNANDEZ  | MELINDA    |
| FIELDS     | VICKI      |
| FISHER     | CINDY      |
| FLEMING    | MYRTLE     |
| FLETCHER   | MAE        |
| FLORES     | JULIA      |
| FORD       | CRYSTAL    |
| FORMAN     | MICHEAL    |
| FORSYTHE   | ENRIQUE    |
| FORTIER    | RAUL       |
| FORTNER    | HOWARD     |
| FOSTER     | PHYLLIS    |
| FOUST      | JACK       |
| FOWLER     | JO         |
| FOX        | HOLLY      |
+------------+------------+
18 rows in set (0.00 sec)
```

### Collation

Para trabajar con rangos de cadenas, necesitas conocer el orden de los caracteres dentro de tu conjunto de caracteres, lo que se llama collation (orden de clasificación).


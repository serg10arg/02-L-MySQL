
### La Sentencia SQL:
```sql
SELECT MAX(amount) max_amt,
       MIN(amount) min_amt,
       AVG(amount) avg_amt,
       SUM(amount) tot_amt,
       COUNT(*) num_payments
FROM payment;
```

### ¿Qué hace esta sentencia?

1. **SELECT**: Es una palabra clave en SQL que se utiliza para seleccionar datos. En este caso, estamos seleccionando el resultado de varias funciones de agregación aplicadas a la columna `amount` de la tabla `payment`.

2. **Funciones de agregación**:
    - **`MAX(amount)`**: Devuelve el valor máximo de la columna `amount`.
    - **`MIN(amount)`**: Devuelve el valor mínimo de la columna `amount`.
    - **`AVG(amount)`**: Devuelve el valor promedio de la columna `amount`.
    - **`SUM(amount)`**: Devuelve la suma total de los valores de la columna `amount`.
    - **`COUNT(*)`**: Devuelve el número total de filas en la tabla `payment`.

3. **Alias**: Se utilizan alias (`max_amt`, `min_amt`, `avg_amt`, `tot_amt`, `num_payments`) para dar nombres descriptivos a las columnas resultantes.

4. **FROM payment**: Indica que los datos se seleccionan de la tabla `payment`.

---

### Desglosando los resultados:

#### 1. **`MAX(amount) max_amt`**:
- Devuelve el valor máximo de la columna `amount`.
- En este caso, el valor máximo es `11.99`.

#### 2. **`MIN(amount) min_amt`**:
- Devuelve el valor mínimo de la columna `amount`.
- En este caso, el valor mínimo es `0.00`.

#### 3. **`AVG(amount) avg_amt`**:
- Devuelve el valor promedio de la columna `amount`.
- En este caso, el valor promedio es `4.200667`.

#### 4. **`SUM(amount) tot_amt`**:
- Devuelve la suma total de los valores de la columna `amount`.
- En este caso, la suma total es `67416.51`.

#### 5. **`COUNT(*) num_payments`**:
- Devuelve el número total de filas en la tabla `payment`.
- En este caso, hay `16049` filas.

---

### Resultado:
```sql
+---------+---------+----------+----------+--------------+
| max_amt | min_amt | avg_amt  | tot_amt  | num_payments |
+---------+---------+----------+----------+--------------+
|   11.99 |    0.00 | 4.200667 | 67416.51 |        16049 |
+---------+---------+----------+----------+--------------+
```

---

### Resumen:
- **`MAX(amount)`**: Devuelve el valor máximo de la columna `amount`.
- **`MIN(amount)`**: Devuelve el valor mínimo de la columna `amount`.
- **`AVG(amount)`**: Devuelve el valor promedio de la columna `amount`.
- **`SUM(amount)`**: Devuelve la suma total de los valores de la columna `amount`.
- **`COUNT(*)`**: Devuelve el número total de filas en la tabla.

---

### ¿Para qué sirve esto?
- Estas funciones de agregación son útiles cuando necesitas realizar análisis estadísticos o resumir datos en una tabla. Por ejemplo:
    - Encontrar el pago más alto y más bajo.
    - Calcular el promedio de los pagos.
    - Obtener el total de pagos realizados.
    - Contar el número total de transacciones.

---

### Ejemplo adicional:
Si quisieras calcular estas métricas para un cliente específico, podrías agregar una cláusula `WHERE`:

```sql
SELECT MAX(amount) max_amt,
       MIN(amount) min_amt,
       AVG(amount) avg_amt,
       SUM(amount) tot_amt,
       COUNT(*) num_payments
FROM payment
WHERE customer_id = 1;
```

Esto devolvería las métricas solo para el cliente con `customer_id = 1`.

---



---

### **Contando Valores Distintos**

En SQL, la función `COUNT()` se utiliza para contar el número de filas o valores en un conjunto de datos. Sin embargo, a veces es útil contar solo los valores **únicos** (distintos) en una columna, en lugar de contar todas las filas. Esto se logra usando la cláusula `DISTINCT` dentro de la función `COUNT()`.

---

### **Ejemplo de la Consulta**

```sql
SELECT COUNT(customer_id) num_rows,
       COUNT(DISTINCT customer_id) num_customers
FROM payment;
```

---

### **¿Qué hace esta consulta?**

1. **`COUNT(customer_id) num_rows`**:
    - Cuenta el número total de filas en la tabla `payment` donde `customer_id` no es `NULL`.
    - Esto incluye **todas las filas**, incluso si hay valores duplicados en `customer_id`.
    - El alias `num_rows` se usa para dar un nombre descriptivo a esta columna en el resultado.

2. **`COUNT(DISTINCT customer_id) num_customers`**:
    - Cuenta el número de valores **únicos** en la columna `customer_id`.
    - Esto ignora los valores duplicados y cuenta solo una vez cada `customer_id` distinto.
    - El alias `num_customers` se usa para dar un nombre descriptivo a esta columna en el resultado.

---

### **Resultado de la Consulta**

```sql
+----------+---------------+
| num_rows | num_customers |
+----------+---------------+
|    16049 |           599 |
+----------+---------------+
```

- **`num_rows`**: `16049` (el número total de filas en la tabla `payment` donde `customer_id` no es `NULL`).
- **`num_customers`**: `599` (el número de clientes únicos que han realizado pagos).

---

### **¿Por qué son diferentes estos valores?**

- **`num_rows` (16049)**:
    - Representa el número total de pagos registrados en la tabla `payment`.
    - Un mismo cliente (`customer_id`) puede haber realizado múltiples pagos, por lo que hay muchas filas con el mismo `customer_id`.

- **`num_customers` (599)**:
    - Representa el número de clientes únicos que han realizado pagos.
    - Aquí, cada cliente se cuenta solo una vez, independientemente de cuántos pagos haya realizado.

---

### **¿Cómo funciona `DISTINCT` dentro de `COUNT`?**

- **`COUNT(DISTINCT columna)`**:
    - Primero, elimina los valores duplicados en la columna especificada.
    - Luego, cuenta el número de valores únicos restantes.
    - Es útil cuando quieres saber cuántos valores diferentes hay en una columna.

---

### **Ejemplo Práctico**

Supongamos que la tabla `payment` tiene los siguientes datos:

| payment_id | customer_id | amount |
|------------|-------------|--------|
| 1          | 1           | 10.00  |
| 2          | 1           | 15.00  |
| 3          | 2           | 20.00  |
| 4          | 3           | 25.00  |
| 5          | 2           | 30.00  |

- **`COUNT(customer_id)`**:
    - Cuenta todas las filas donde `customer_id` no es `NULL`.
    - Resultado: `5` (hay 5 pagos en total).

- **`COUNT(DISTINCT customer_id)`**:
    - Cuenta los valores únicos en `customer_id`.
    - Valores únicos: `1`, `2`, `3`.
    - Resultado: `3` (hay 3 clientes únicos).

---

### **¿Cuándo usar `COUNT(DISTINCT columna)`?**

- Cuando necesitas contar el número de valores únicos en una columna.
- Ejemplos comunes:
    - Contar el número de clientes únicos.
    - Contar el número de productos únicos vendidos.
    - Contar el número de países únicos en una tabla.

---

### **Ejemplo Adicional**

Si quisieras contar el número de películas únicas alquiladas en una tabla `rental`, usarías:

```sql
SELECT COUNT(DISTINCT film_id) num_unique_films
FROM rental;
```

---

### **Resumen**

- **`COUNT(columna)`**: Cuenta todas las filas donde `columna` no es `NULL` (incluye duplicados).
- **`COUNT(DISTINCT columna)`**: Cuenta solo los valores únicos en `columna` (ignora duplicados).
- **Uso típico**: Contar clientes únicos, productos únicos, países únicos, etc.

---


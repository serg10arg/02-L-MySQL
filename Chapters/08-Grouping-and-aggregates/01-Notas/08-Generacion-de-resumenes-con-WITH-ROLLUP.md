
---

### **Generación de Resúmenes con `WITH ROLLUP`**

En SQL, la cláusula `WITH ROLLUP` es una extensión de `GROUP BY` que permite generar resúmenes adicionales en los resultados de una consulta. Estos resúmenes incluyen subtotales y totales generales, lo que facilita el análisis de datos agrupados.

---

### **Ejemplo de la Consulta con `WITH ROLLUP`**

Supongamos que tienes una consulta que cuenta el número de películas para cada combinación de actor y clasificación (`rating`):

```sql
SELECT fa.actor_id, f.rating, COUNT(*)
FROM film_actor fa
  INNER JOIN film f
  ON fa.film_id = f.film_id
GROUP BY fa.actor_id, f.rating WITH ROLLUP
ORDER BY 1, 2;
```

#### **Resultado:**
```sql
+----------+--------+----------+
| actor_id | rating | count(*) |
+----------+--------+----------+
|        1 | NULL   |       19 |
|        1 | G      |        4 |
|        1 | PG     |        6 |
|        1 | PG-13  |        1 |
|        1 | R      |        3 |
|        1 | NC-17  |        5 |
|        2 | NULL   |       25 |
|        2 | G      |        7 |
|        2 | PG     |        6 |
|        2 | PG-13  |        2 |
|        2 | R      |        2 |
|        2 | NC-17  |        8 |
...
|      200 | NULL   |       20 |
|      200 | G      |        5 |
|      200 | PG     |        3 |
|      200 | PG-13  |        2 |
|      200 | R      |        6 |
|      200 | NC-17  |        4 |
|     NULL | NULL   |     5462 |
+----------+--------+----------+
1197 rows in set (0.07 sec)
```

---

### **Explicación del Resultado**

1. **Filas por Actor y Clasificación**:
    - Para cada combinación de `actor_id` y `rating`, se muestra el número de películas.
    - Ejemplo: El actor `1` tiene `4` películas con clasificación `G`, `6` con `PG`, etc.

2. **Subtotales por Actor**:
    - Para cada actor, se agrega una fila adicional donde `rating` es `NULL`.
    - Esta fila muestra el total de películas para ese actor, sumando todas sus clasificaciones.
    - Ejemplo: El actor `1` tiene un total de `19` películas.

3. **Total General**:
    - Al final del resultado, hay una fila donde tanto `actor_id` como `rating` son `NULL`.
    - Esta fila muestra el total general de películas en la tabla `film_actor` (`5462`).

---

### **¿Cómo funciona `WITH ROLLUP`?**

- **`WITH ROLLUP`**:
    - Genera filas adicionales que resumen los datos agrupados.
    - Para cada grupo, agrega una fila con subtotales.
    - Al final, agrega una fila con el total general.

- **Valores `NULL`**:
    - Los valores `NULL` en las columnas de agrupación indican que esa fila es un resumen.
    - Por ejemplo, `actor_id = NULL` y `rating = NULL` indica el total general.

---

### **Ventajas de `WITH ROLLUP`**

1. **Resúmenes Automáticos**:
    - No necesitas ejecutar consultas adicionales o usar herramientas externas para calcular subtotales y totales.

2. **Facilita el Análisis**:
    - Proporciona una vista completa de los datos, incluyendo resúmenes parciales y totales.

3. **Ahorra Tiempo**:
    - Evita la necesidad de combinar manualmente los resultados de varias consultas.

---

### **Uso en Otras Bases de Datos**

- **Oracle Database**:
    - Usa la sintaxis `GROUP BY ROLLUP(col1, col2)`.
    - Ejemplo:
      ```sql
      SELECT fa.actor_id, f.rating, COUNT(*)
      FROM film_actor fa
        INNER JOIN film f
        ON fa.film_id = f.film_id
      GROUP BY ROLLUP(fa.actor_id, f.rating)
      ORDER BY 1, 2;
      ```

- **SQL Server**:
    - También admite `WITH ROLLUP` y `WITH CUBE`.

---

### **`WITH CUBE` (No disponible en MySQL 8.0)**

- **`WITH CUBE`**:
    - Genera resúmenes para todas las combinaciones posibles de las columnas de agrupación.
    - Por ejemplo, si agrupas por `actor_id` y `rating`, `WITH CUBE` generará:
        - Subtotales por `actor_id`.
        - Subtotales por `rating`.
        - El total general.

- **Disponibilidad**:
    - No está disponible en MySQL 8.0, pero sí en SQL Server y Oracle Database.

---

### **Ejemplo Adicional**

Si quisieras calcular subtotales por año y mes para los alquileres, podrías usar:

```sql
SELECT EXTRACT(YEAR FROM rental_date) year,
       EXTRACT(MONTH FROM rental_date) month,
       COUNT(*) how_many
FROM rental
GROUP BY EXTRACT(YEAR FROM rental_date), EXTRACT(MONTH FROM rental_date) WITH ROLLUP
ORDER BY year, month;
```

---

### **Resumen de Conceptos Clave**

1. **`WITH ROLLUP`**:
    - Genera subtotales y totales generales en los resultados de una consulta agrupada.
    - Usa valores `NULL` para indicar filas de resumen.

2. **`WITH CUBE`**:
    - Genera resúmenes para todas las combinaciones posibles de las columnas de agrupación.
    - No está disponible en MySQL 8.0.

3. **Uso en Otras Bases de Datos**:
    - Oracle y SQL Server tienen sintaxis similares para generar resúmenes.

---


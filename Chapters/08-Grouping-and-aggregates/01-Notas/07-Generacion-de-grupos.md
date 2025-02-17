
---

### **Generación de Grupos en SQL**

En SQL, la cláusula `GROUP BY` se utiliza para agrupar filas que tienen los mismos valores en una o más columnas. Esto es especialmente útil cuando necesitas realizar cálculos o agregaciones (como contar, sumar, promediar, etc.) sobre grupos de datos en lugar de sobre toda la tabla.

---

### **¿Por qué agrupar datos?**

Las personas rara vez están interesadas en ver datos sin procesar. En su lugar, prefieren ver resúmenes o análisis que les ayuden a tomar decisiones. Algunos ejemplos comunes de manipulaciones de datos incluyen:
1. **Generar totales**: Por ejemplo, las ventas totales en Europa.
2. **Encontrar valores atípicos**: Por ejemplo, el mejor vendedor del 2020.
3. **Determinar frecuencias**: Por ejemplo, el número de películas alquiladas en cada mes.

Para realizar estos análisis, necesitas agrupar los datos en función de una o más columnas o expresiones.

---

### **Agrupación de una Sola Columna**

La agrupación más simple es aquella que se basa en una sola columna. Por ejemplo, si deseas saber cuántas películas ha actuado cada actor, puedes agrupar por la columna `actor_id`:

```sql
SELECT actor_id, COUNT(*)
FROM film_actor
GROUP BY actor_id;
```

#### **Resultado:**
```sql
+----------+----------+
| actor_id | count(*) |
+----------+----------+
|        1 |       19 |
|        2 |       25 |
|        3 |       22 |
|        4 |       22 |
...
|      200 |       20 |
+----------+----------+
200 rows in set (0.11 sec)
```

#### **Explicación:**
- **`GROUP BY actor_id`**: Agrupa las filas de la tabla `film_actor` por el valor de `actor_id`.
- **`COUNT(*)`**: Cuenta el número de filas (películas) para cada grupo (actor).
- **Resultado**: Devuelve 200 filas, una por cada actor, con el número de películas en las que ha actuado.

---

### **Agrupación de Múltiples Columnas**

En algunos casos, es útil agrupar por más de una columna. Por ejemplo, si deseas saber cuántas películas de cada clasificación (`rating`) ha actuado cada actor, puedes agrupar por `actor_id` y `rating`:

```sql
SELECT fa.actor_id, f.rating, COUNT(*)
FROM film_actor fa
  INNER JOIN film f
  ON fa.film_id = f.film_id
GROUP BY fa.actor_id, f.rating
ORDER BY 1, 2;
```

#### **Resultado:**
```sql
+----------+--------+----------+
| actor_id | rating | count(*) |
+----------+--------+----------+
|        1 | G      |        4 |
|        1 | PG     |        6 |
|        1 | PG-13  |        1 |
|        1 | R      |        3 |
|        1 | NC-17  |        5 |
|        2 | G      |        7 |
|        2 | PG     |        6 |
|        2 | PG-13  |        2 |
|        2 | R      |        2 |
|        2 | NC-17  |        8 |
...
|      200 | NC-17  |        4 |
+----------+--------+----------+
996 rows in set (0.01 sec)
```

#### **Explicación:**
- **`GROUP BY fa.actor_id, f.rating`**: Agrupa las filas por `actor_id` y `rating`.
- **`COUNT(*)`**: Cuenta el número de películas para cada combinación de actor y clasificación.
- **Resultado**: Devuelve 996 filas, una por cada combinación única de `actor_id` y `rating`.

---

### **Agrupación mediante Expresiones**

También puedes agrupar datos basados en expresiones, no solo en columnas. Por ejemplo, si deseas agrupar los alquileres por año, puedes usar la función `EXTRACT` para obtener el año de la fecha de alquiler:

```sql
SELECT EXTRACT(YEAR FROM rental_date) year,
       COUNT(*) how_many
FROM rental
GROUP BY EXTRACT(YEAR FROM rental_date);
```

#### **Resultado:**
```sql
+------+----------+
| year | how_many |
+------+----------+
| 2005 |    15862 |
| 2006 |      182 |
+------+----------+
2 rows in set (0.01 sec)
```

#### **Explicación:**
- **`EXTRACT(YEAR FROM rental_date)`**: Extrae el año de la columna `rental_date`.
- **`GROUP BY EXTRACT(YEAR FROM rental_date)`**: Agrupa las filas por el año extraído.
- **`COUNT(*)`**: Cuenta el número de alquileres para cada año.
- **Resultado**: Devuelve 2 filas, una para cada año (2005 y 2006), con el número de alquileres en ese año.

---

### **Resumen de Conceptos Clave**

1. **`GROUP BY`**:
    - Se utiliza para agrupar filas que tienen los mismos valores en una o más columnas.
    - Permite realizar cálculos (como `COUNT`, `SUM`, `AVG`, etc.) sobre cada grupo.

2. **Agrupación de una columna**:
    - Útil para resumir datos basados en una sola columna (por ejemplo, contar películas por actor).

3. **Agrupación de múltiples columnas**:
    - Útil para resumir datos basados en combinaciones de columnas (por ejemplo, contar películas por actor y clasificación).

4. **Agrupación mediante expresiones**:
    - Útil para resumir datos basados en valores derivados (por ejemplo, agrupar por año extraído de una fecha).

---

### **¿Cuándo usar `GROUP BY`?**

- Cuando necesitas resumir datos (por ejemplo, contar, sumar, promediar).
- Cuando necesitas agrupar datos basados en una o más columnas o expresiones.
- Cuando necesitas filtrar grupos usando `HAVING` (por ejemplo, mostrar solo grupos con más de 10 elementos).

---

### **Ejemplo Adicional**

Si quisieras contar el número de alquileres por mes y año, podrías usar:

```sql
SELECT EXTRACT(YEAR FROM rental_date) year,
       EXTRACT(MONTH FROM rental_date) month,
       COUNT(*) how_many
FROM rental
GROUP BY EXTRACT(YEAR FROM rental_date), EXTRACT(MONTH FROM rental_date)
ORDER BY year, month;
```

---



---

### **Subconsultas Multicolumna**

Las subconsultas multicolumna son aquellas que devuelven **múltiples columnas** en lugar de una sola. Estas subconsultas son útiles cuando necesitas comparar múltiples columnas de una tabla con los resultados de una subconsulta.

---

### **Ejemplo con Subconsultas de Una Columna**

Supongamos que queremos encontrar todas las combinaciones de `actor_id` y `film_id` donde:
- El actor tenga el apellido `MONROE`.
- La película tenga una clasificación `PG`.

Podemos usar dos subconsultas de una sola columna para lograr esto:

```sql
SELECT fa.actor_id, fa.film_id
FROM film_actor fa
WHERE fa.actor_id IN (
  SELECT actor_id
  FROM actor
  WHERE last_name = 'MONROE'
)
AND fa.film_id IN (
  SELECT film_id
  FROM film
  WHERE rating = 'PG'
);
```

#### **Resultado:**
```sql
+----------+---------+
| actor_id | film_id |
+----------+---------+
|      120 |      63 |
|      120 |     144 |
|      120 |     414 |
|      120 |     590 |
|      120 |     715 |
|      120 |     894 |
|      178 |     164 |
|      178 |     194 |
|      178 |     273 |
|      178 |     311 |
|      178 |     983 |
+----------+---------+
11 rows in set (0.00 sec)
```

#### **Explicación:**
1. **Primera subconsulta**:
    - `SELECT actor_id FROM actor WHERE last_name = 'MONROE'`:
        - Devuelve los `actor_id` de los actores con el apellido `MONROE`.

2. **Segunda subconsulta**:
    - `SELECT film_id FROM film WHERE rating = 'PG'`:
        - Devuelve los `film_id` de las películas con clasificación `PG`.

3. **Consulta contenedora**:
    - Filtra las filas de `film_actor` donde `actor_id` y `film_id` coinciden con los resultados de las subconsultas.

---

### **Mejora con Subconsultas Multicolumna**

En lugar de usar dos subconsultas de una sola columna, podemos usar una **subconsulta multicolumna** para lograr el mismo resultado de manera más eficiente:

```sql
SELECT actor_id, film_id
FROM film_actor
WHERE (actor_id, film_id) IN (
  SELECT a.actor_id, f.film_id
  FROM actor a
    CROSS JOIN film f
  WHERE a.last_name = 'MONROE'
    AND f.rating = 'PG'
);
```

#### **Resultado:**
```sql
+----------+---------+
| actor_id | film_id |
+----------+---------+
|      120 |      63 |
|      120 |     144 |
|      120 |     414 |
|      120 |     590 |
|      120 |     715 |
|      120 |     894 |
|      178 |     164 |
|      178 |     194 |
|      178 |     273 |
|      178 |     311 |
|      178 |     983 |
+----------+---------+
11 rows in set (0.00 sec)
```

#### **Explicación:**
1. **Subconsulta multicolumna**:
    - `SELECT a.actor_id, f.film_id FROM actor a CROSS JOIN film f WHERE a.last_name = 'MONROE' AND f.rating = 'PG'`:
        - Devuelve todas las combinaciones de `actor_id` y `film_id` donde el actor tiene el apellido `MONROE` y la película tiene clasificación `PG`.
        - Usa un `CROSS JOIN` para generar todas las combinaciones posibles entre actores y películas que cumplen las condiciones.

2. **Consulta contenedora**:
    - `SELECT actor_id, film_id FROM film_actor WHERE (actor_id, film_id) IN (...)`:
        - Filtra las filas de `film_actor` donde la combinación de `actor_id` y `film_id` está en el conjunto devuelto por la subconsulta.

---

### **Ventajas de las Subconsultas Multicolumna**

1. **Eficiencia**:
    - Reducen la necesidad de múltiples subconsultas.
    - Simplifican la lógica de la consulta.

2. **Legibilidad**:
    - Hacen que la consulta sea más fácil de entender y mantener.

3. **Flexibilidad**:
    - Permiten comparar múltiples columnas simultáneamente.

---

### **Resumen de Conceptos Clave**

1. **Subconsultas Multicolumna**:
    - Devuelven múltiples columnas en lugar de una sola.
    - Útiles para comparar múltiples columnas de una tabla con los resultados de una subconsulta.

2. **Uso de `CROSS JOIN`**:
    - Genera todas las combinaciones posibles entre dos tablas.
    - Útil para crear conjuntos de datos que luego se pueden filtrar.

3. **Comparación con `IN`**:
    - Permite comparar múltiples columnas usando una sola subconsulta.

---

### **Ejemplo Adicional**

Supongamos que queremos encontrar todas las combinaciones de `customer_id` y `film_id` donde:
- El cliente vive en una ciudad específica.
- La película tiene una duración mayor a 120 minutos.

Podemos usar una subconsulta multicolumna para lograr esto:

```sql
SELECT r.customer_id, r.film_id
FROM rental r
WHERE (r.customer_id, r.film_id) IN (
  SELECT c.customer_id, f.film_id
  FROM customer c
    CROSS JOIN film f
  WHERE c.city = 'New York'
    AND f.length > 120
);
```

#### **Explicación:**
1. **Subconsulta multicolumna**:
    - `SELECT c.customer_id, f.film_id FROM customer c CROSS JOIN film f WHERE c.city = 'New York' AND f.length > 120`:
        - Devuelve todas las combinaciones de `customer_id` y `film_id` donde el cliente vive en `New York` y la película dura más de 120 minutos.

2. **Consulta contenedora**:
    - `SELECT r.customer_id, r.film_id FROM rental r WHERE (r.customer_id, r.film_id) IN (...)`:
        - Filtra las filas de `rental` donde la combinación de `customer_id` y `film_id` está en el conjunto devuelto por la subconsulta.

---


### **1. ¿Es posible usar `INNER JOIN` en lugar de `CROSS JOIN` en este ejemplo?**

Sí, es posible usar `INNER JOIN` en lugar de `CROSS JOIN`, pero **no sería equivalente** en este caso específico. Aquí está la razón:

#### **Diferencia entre `CROSS JOIN` e `INNER JOIN`**:
- **`CROSS JOIN`**:
    - Genera el **producto cartesiano** de dos tablas, es decir, todas las combinaciones posibles entre las filas de ambas tablas.
    - No requiere una condición de unión (`ON` o `USING`).
    - En este ejemplo, `CROSS JOIN` se usa para generar todas las combinaciones posibles entre actores con el apellido `MONROE` y películas con clasificación `PG`.

- **`INNER JOIN`**:
    - Combina filas de dos tablas **solo si hay una coincidencia** según una condición de unión (`ON` o `USING`).
    - Si no hay una condición de unión, `INNER JOIN` se comporta como un `CROSS JOIN`, pero esto no es una práctica recomendada.

#### **¿Por qué no usar `INNER JOIN` aquí?**
En este caso, no hay una relación directa entre las tablas `actor` y `film` que justifique un `INNER JOIN`. La consulta está diseñada para generar **todas las combinaciones posibles** entre actores y películas que cumplen ciertas condiciones (`last_name = 'MONROE'` y `rating = 'PG'`). Por lo tanto, `CROSS JOIN` es la opción correcta.

Si intentaras usar `INNER JOIN` sin una condición de unión, obtendrías el mismo resultado que con `CROSS JOIN`, pero no sería una práctica recomendada porque `INNER JOIN` está diseñado para unir tablas basadas en una relación específica.

---

### **2. ¿Por qué debemos referenciar las columnas `actor_id` y `film_id` con los alias de las tablas `film` y `actor` dentro de la subconsulta?**

La referencia a las columnas con los alias de las tablas (`a.actor_id` y `f.film_id`) es necesaria por las siguientes razones:

#### **Evitar Ambigüedad**
- En la subconsulta, estamos seleccionando columnas de dos tablas diferentes (`actor` y `film`).
- Ambas tablas tienen una columna llamada `actor_id` y `film_id`.
- Si no usamos alias, el motor de la base de datos no sabría a qué tabla pertenece cada columna, lo que generaría un error de ambigüedad.

#### **Mejorar la Legibilidad**
- Usar alias (`a` para `actor` y `f` para `film`) hace que la consulta sea más clara y fácil de entender.
- Permite identificar rápidamente de qué tabla proviene cada columna.

#### **Ejemplo sin Alias (Error de Ambigüedad)**

Si no usamos alias, la consulta generaría un error:

```sql
SELECT actor_id, film_id
FROM film_actor
WHERE (actor_id, film_id) IN (
  SELECT actor_id, film_id  -- ¿De qué tabla son estas columnas?
  FROM actor
    CROSS JOIN film
  WHERE last_name = 'MONROE'
    AND rating = 'PG'
);
```

El motor de la base de datos no sabría si `actor_id` y `film_id` pertenecen a la tabla `actor` o a la tabla `film`, lo que resultaría en un error como:

```
ERROR 1052 (23000): Column 'actor_id' in field list is ambiguous
```

#### **Ejemplo con Alias (Correcto)**

Usando alias, la consulta es clara y funciona correctamente:

```sql
SELECT actor_id, film_id
FROM film_actor
WHERE (actor_id, film_id) IN (
  SELECT a.actor_id, f.film_id  -- Claramente referenciadas
  FROM actor a
    CROSS JOIN film f
  WHERE a.last_name = 'MONROE'
    AND f.rating = 'PG'
);
```

---

### **Resumen**

1. **`CROSS JOIN` vs. `INNER JOIN`**:
    - En este caso, `CROSS JOIN` es la opción correcta porque necesitamos generar todas las combinaciones posibles entre actores y películas.
    - `INNER JOIN` no es adecuado aquí porque no hay una relación directa entre las tablas `actor` y `film`.

2. **Uso de Alias**:
    - Los alias (`a` y `f`) son necesarios para evitar ambigüedades y mejorar la legibilidad de la consulta.
    - Sin alias, el motor de la base de datos no sabría a qué tabla pertenecen las columnas `actor_id` y `film_id`.

---

### **¿Es válido pensar que siempre debes usar `CROSS JOIN` en subconsultas multicolumna?**

La respuesta es **no**. No siempre es necesario o apropiado usar `CROSS JOIN` en subconsultas multicolumna. El uso de `CROSS JOIN` depende del **contexto** y de la **relación entre las tablas** que estás consultando.

---

### **¿Cuándo usar `CROSS JOIN` en subconsultas multicolumna?**

`CROSS JOIN` es útil cuando necesitas generar **todas las combinaciones posibles** entre dos conjuntos de datos. Esto es común en escenarios donde no hay una relación directa entre las tablas, pero necesitas combinar sus datos de manera exhaustiva.

#### **Ejemplo de Uso Correcto de `CROSS JOIN`**

Supongamos que queremos encontrar todas las combinaciones de `actor_id` y `film_id` donde:
- El actor tenga el apellido `MONROE`.
- La película tenga una clasificación `PG`.

En este caso, no hay una relación directa entre `actor` y `film`, por lo que usamos `CROSS JOIN` para generar todas las combinaciones posibles:

```sql
SELECT actor_id, film_id
FROM film_actor
WHERE (actor_id, film_id) IN (
  SELECT a.actor_id, f.film_id
  FROM actor a
    CROSS JOIN film f
  WHERE a.last_name = 'MONROE'
    AND f.rating = 'PG'
);
```

---

### **¿Cuándo no usar `CROSS JOIN` en subconsultas multicolumna?**

En muchos casos, las tablas tienen una **relación directa** (por ejemplo, a través de una clave primaria y una clave foránea). En estos casos, usar `CROSS JOIN` sería incorrecto, ya que generaría combinaciones innecesarias y posiblemente incorrectas. En su lugar, debes usar `INNER JOIN`, `LEFT JOIN`, u otros tipos de uniones que reflejen la relación entre las tablas.

#### **Ejemplo de Uso Incorrecto de `CROSS JOIN`**

Supongamos que queremos encontrar todas las películas alquiladas por un cliente específico. Aquí hay una relación directa entre `customer` y `rental` a través de `customer_id`. Usar `CROSS JOIN` sería incorrecto porque generaría combinaciones innecesarias:

```sql
-- Incorrecto: Usar CROSS JOIN aquí no tiene sentido
SELECT r.rental_id, r.customer_id, r.film_id
FROM rental r
WHERE (r.customer_id, r.film_id) IN (
  SELECT c.customer_id, f.film_id
  FROM customer c
    CROSS JOIN film f  -- Esto generaría combinaciones innecesarias
  WHERE c.customer_id = 5
);
```

En su lugar, debes usar `INNER JOIN` para reflejar la relación entre `customer` y `rental`:

```sql
-- Correcto: Usar INNER JOIN para reflejar la relación
SELECT r.rental_id, r.customer_id, r.film_id
FROM rental r
  INNER JOIN customer c ON r.customer_id = c.customer_id
WHERE c.customer_id = 5;
```

---

### **¿Cuándo usar subconsultas multicolumna sin `CROSS JOIN`?**

Las subconsultas multicolumna no siempre requieren `CROSS JOIN`. Puedes usarlas en combinación con otros tipos de uniones (`INNER JOIN`, `LEFT JOIN`, etc.) o incluso sin uniones si las columnas provienen de la misma tabla.

#### **Ejemplo sin `CROSS JOIN`**

Supongamos que queremos encontrar todas las combinaciones de `customer_id` y `film_id` donde:
- El cliente vive en una ciudad específica.
- La película tiene una duración mayor a 120 minutos.

En este caso, no necesitamos `CROSS JOIN` porque las columnas provienen de tablas relacionadas:

```sql
SELECT r.customer_id, r.film_id
FROM rental r
  INNER JOIN customer c ON r.customer_id = c.customer_id
  INNER JOIN film f ON r.film_id = f.film_id
WHERE c.city = 'New York'
  AND f.length > 120;
```

---

### **Resumen de Buenas Prácticas**

1. **Usa `CROSS JOIN`**:
    - Cuando necesitas generar **todas las combinaciones posibles** entre dos conjuntos de datos.
    - Cuando no hay una relación directa entre las tablas.

2. **No uses `CROSS JOIN`**:
    - Cuando las tablas tienen una **relación directa** (por ejemplo, a través de claves primarias y foráneas).
    - En su lugar, usa `INNER JOIN`, `LEFT JOIN`, u otros tipos de uniones que reflejen la relación.

3. **Subconsultas multicolumna**:
    - No siempre requieren `CROSS JOIN`.
    - Pueden usarse con otros tipos de uniones o incluso sin uniones si las columnas provienen de la misma tabla.

---

### **Conclusión**

No es una buena práctica pensar que siempre debes usar `CROSS JOIN` en subconsultas multicolumna. El uso de `CROSS JOIN` depende del contexto y de la relación entre las tablas. En general:
- Usa `CROSS JOIN` cuando necesitas generar combinaciones exhaustivas.
- Usa `INNER JOIN` u otras uniones cuando las tablas tienen una relación directa.

---


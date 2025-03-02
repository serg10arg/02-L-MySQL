
---

### **Joins Externos de Tres Vías**

Un **join externo de tres vías** es una operación en la que se realiza un `LEFT OUTER JOIN` (o `RIGHT OUTER JOIN`) entre una tabla principal y dos tablas adicionales. Esto es útil cuando deseas incluir datos de múltiples tablas relacionadas, incluso si no hay coincidencias en alguna de ellas.

---

### **Ejemplo Práctico**

En el ejemplo proporcionado, se realiza un join externo de tres vías entre las tablas `film`, `inventory`, y `rental`. El objetivo es obtener información sobre las películas, sus copias en inventario y las fechas de alquiler de esas copias.

#### Consulta:
```sql
SELECT f.film_id, f.title, i.inventory_id, r.rental_date
FROM film f
  LEFT OUTER JOIN inventory i ON f.film_id = i.film_id
  LEFT OUTER JOIN rental r ON i.inventory_id = r.inventory_id
WHERE f.film_id BETWEEN 13 AND 15;
```

---

### **Desglose de la Consulta**

1. **Primer Join (`film` y `inventory`):**
    - **Tipo de Join:** `LEFT OUTER JOIN`.
    - **Condición:** `f.film_id = i.film_id`.
    - **Propósito:** Incluir todas las películas en el rango especificado, incluso si no tienen copias en el inventario.

2. **Segundo Join (`inventory` y `rental`):**
    - **Tipo de Join:** `LEFT OUTER JOIN`.
    - **Condición:** `i.inventory_id = r.inventory_id`.
    - **Propósito:** Incluir todas las copias en inventario, incluso si no han sido alquiladas.

3. **Filtro (`WHERE`):**
    - **Condición:** `f.film_id BETWEEN 13 AND 15`.
    - **Propósito:** Limitar los resultados a las películas con `film_id` entre 13 y 15.

---

### **Resultados**

La consulta devuelve 32 filas, que incluyen:

1. **Películas con Copias y Alquileres:**
    - Ejemplo: `Ali Forever` y `Alien Center`.
    - Estas películas tienen copias en inventario (`inventory_id` no es `NULL`) y han sido alquiladas (`rental_date` no es `NULL`).

2. **Películas sin Copias:**
    - Ejemplo: `Alice Fantasia`.
    - Esta película no tiene copias en inventario, por lo que `inventory_id` y `rental_date` son `NULL`.

---

### **Explicación Detallada**

1. **Primer Join (`film` y `inventory`):**
    - Se incluyen todas las películas en el rango especificado (`film_id` entre 13 y 15).
    - Para las películas sin copias en inventario (como `Alice Fantasia`), las columnas de `inventory` (`inventory_id`) serán `NULL`.

2. **Segundo Join (`inventory` y `rental`):**
    - Se incluyen todas las copias en inventario, incluso si no han sido alquiladas.
    - Para las copias que no han sido alquiladas, las columnas de `rental` (`rental_date`) serán `NULL`.

3. **Filtro (`WHERE`):**
    - Limita los resultados a las películas con `film_id` entre 13 y 15.

---

### **Ejemplo de Resultados**

```sql
+---------+----------------+--------------+---------------------+
| film_id | title          | inventory_id | rental_date         |
+---------+----------------+--------------+---------------------+
|      13 | ALI FOREVER    |           67 | 2005-07-31 18:11:17 |
|      13 | ALI FOREVER    |           67 | 2005-08-22 21:59:29 |
|      13 | ALI FOREVER    |           68 | 2005-07-28 15:26:20 |
|      13 | ALI FOREVER    |           68 | 2005-08-23 05:02:31 |
|      13 | ALI FOREVER    |           69 | 2005-08-01 23:36:10 |
|      13 | ALI FOREVER    |           69 | 2005-08-22 02:12:44 |
|      13 | ALI FOREVER    |           70 | 2005-07-12 10:51:09 |
|      13 | ALI FOREVER    |           70 | 2005-07-29 01:29:51 |
|      13 | ALI FOREVER    |           70 | 2006-02-14 15:16:03 |
|      14 | ALICE FANTASIA |         NULL | NULL                |
|      15 | ALIEN CENTER   |           71 | 2005-05-28 02:06:37 |
|      15 | ALIEN CENTER   |           71 | 2005-06-17 16:40:03 |
|      15 | ALIEN CENTER   |           71 | 2005-07-11 05:47:08 |
|      15 | ALIEN CENTER   |           71 | 2005-08-02 13:58:55 |
|      15 | ALIEN CENTER   |           71 | 2005-08-23 05:13:09 |
|      15 | ALIEN CENTER   |           72 | 2005-05-27 22:49:27 |
|      15 | ALIEN CENTER   |           72 | 2005-06-19 13:29:28 |
|      15 | ALIEN CENTER   |           72 | 2005-07-07 23:05:53 |
|      15 | ALIEN CENTER   |           72 | 2005-08-01 05:55:13 |
|      15 | ALIEN CENTER   |           72 | 2005-08-20 15:11:48 |
|      15 | ALIEN CENTER   |           73 | 2005-07-06 15:51:58 |
|      15 | ALIEN CENTER   |           73 | 2005-07-30 14:48:24 |
|      15 | ALIEN CENTER   |           73 | 2005-08-20 22:32:11 |
|      15 | ALIEN CENTER   |           74 | 2005-07-27 00:15:18 |
|      15 | ALIEN CENTER   |           74 | 2005-08-23 19:21:22 |
|      15 | ALIEN CENTER   |           75 | 2005-07-09 02:58:41 |
|      15 | ALIEN CENTER   |           75 | 2005-07-29 23:52:01 |
|      15 | ALIEN CENTER   |           75 | 2005-08-18 21:55:01 |
|      15 | ALIEN CENTER   |           76 | 2005-06-15 08:01:29 |
|      15 | ALIEN CENTER   |           76 | 2005-07-07 18:31:50 |
|      15 | ALIEN CENTER   |           76 | 2005-08-01 01:49:36 |
|      15 | ALIEN CENTER   |           76 | 2005-08-17 07:26:47 |
+---------+----------------+--------------+---------------------+
```

---

### **Conclusión**

- **Joins Externos de Tres Vías:** Permiten combinar datos de múltiples tablas, incluso si no hay coincidencias en alguna de ellas.
- **Uso de `LEFT OUTER JOIN`:** Garantiza que todas las filas de la tabla principal (`film`) se incluyan en los resultados, independientemente de si hay coincidencias en las tablas secundarias (`inventory` y `rental`).
- **Valores Nulos:** Indican la ausencia de coincidencias en las tablas secundarias.

Este enfoque es especialmente útil cuando necesitas incluir datos "incompletos" o sin coincidencias, como películas sin copias en inventario o copias que nunca han sido alquiladas. 

### **Consulta SQL**

```sql
SELECT title, rating, rental_duration
FROM film
WHERE (rating = 'G' AND rental_duration >= 7)
   OR (rating = 'PG-13' AND rental_duration < 4);
```

### **Explicación Paso a Paso**

---

#### 1. **`SELECT`**
```sql
SELECT title, rating, rental_duration
```
- **`title`**: Selecciona la columna `title` de la tabla `film`, que contiene el título de la película.
- **`rating`**: Selecciona la columna `rating`, que indica la clasificación de la película (por ejemplo, `G`, `PG-13`, etc.).
- **`rental_duration`**: Selecciona la columna `rental_duration`, que indica cuántos días se puede alquilar la película.

---

#### 2. **`FROM`**
```sql
FROM film
```
- Especificamos que estamos trabajando con la tabla `film`, que contiene información sobre las películas.

---

#### 3. **`WHERE`**
```sql
WHERE (rating = 'G' AND rental_duration >= 7)
   OR (rating = 'PG-13' AND rental_duration < 4)
```
- Esta es la parte más importante de la consulta. Aquí estamos aplicando condiciones para filtrar las películas que cumplen con ciertos criterios. Vamos a desglosarla:

    - **`(rating = 'G' AND rental_duration >= 7)`**:
        - **`rating = 'G'`**: Filtra las películas que tienen una clasificación `G` (apta para todos los públicos).
        - **`rental_duration >= 7`**: Filtra las películas que tienen una duración de alquiler de **7 días o más**.
        - **`AND`**: Ambas condiciones deben ser verdaderas al mismo tiempo. Es decir, la película debe ser `G` **y** tener una duración de alquiler de 7 días o más.

    - **`OR`**: Indica que también se incluirán las películas que cumplan la siguiente condición:
        - **`(rating = 'PG-13' AND rental_duration < 4)`**:
            - **`rating = 'PG-13'`**: Filtra las películas que tienen una clasificación `PG-13` (no recomendada para menores de 13 años).
            - **`rental_duration < 4`**: Filtra las películas que tienen una duración de alquiler de **menos de 4 días**.
            - **`AND`**: Ambas condiciones deben ser verdaderas al mismo tiempo. Es decir, la película debe ser `PG-13` **y** tener una duración de alquiler de menos de 4 días.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Selecciona columnas específicas**: Obtenemos el título (`title`), la clasificación (`rating`) y la duración del alquiler (`rental_duration`) de las películas.
2. **Filtra las películas**:
    - Películas con clasificación `G` y duración de alquiler de **7 días o más**.
    - **O** películas con clasificación `PG-13` y duración de alquiler de **menos de 4 días**.

---

### **Ejemplo Práctico**

Imagina que tienes la siguiente tabla `film`:

| title               | rating | rental_duration |
|---------------------|--------|-----------------|
| Toy Story           | G      | 7               |
| Frozen              | G      | 5               |
| The Dark Knight     | PG-13  | 3               |
| Inception           | PG-13  | 5               |
| Finding Nemo        | G      | 10              |
| Jurassic Park       | PG-13  | 2               |

---

#### Resultado de la Consulta
La consulta devolverá:

| title               | rating | rental_duration |
|---------------------|--------|-----------------|
| Toy Story           | G      | 7               |
| Finding Nemo        | G      | 10              |
| The Dark Knight     | PG-13  | 3               |
| Jurassic Park       | PG-13  | 2               |

- **`Toy Story` y `Finding Nemo`**: Son películas con clasificación `G` y duración de alquiler de 7 días o más.
- **`The Dark Knight` y `Jurassic Park`**: Son películas con clasificación `PG-13` y duración de alquiler de menos de 4 días.
- **Nota**: `Frozen` no se incluye porque, aunque es `G`, su duración de alquiler es 5 (no cumple `>= 7`). `Inception` no se incluye porque, aunque es `PG-13`, su duración de alquiler es 5 (no cumple `< 4`).

---

### **Resumen**

- **`SELECT`**: Selecciona el título, la clasificación y la duración del alquiler de las películas.
- **`WHERE`**: Filtra las películas que cumplen una de las dos condiciones:
    1. Clasificación `G` y duración de alquiler de 7 días o más.
    2. Clasificación `PG-13` y duración de alquiler de menos de 4 días.


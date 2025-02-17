
### **Consulta SQL**

```sql
SELECT f.title, f_prnt.title AS prequel
FROM film f
  INNER JOIN film f_prnt
  ON f_prnt.film_id = f.prequel_film_id
WHERE f.prequel_film_id IS NOT NULL;
```

### **Explicación Paso a Paso**

---

#### 1. **`SELECT`**
```sql
SELECT f.title, f_prnt.title AS prequel
```
- **`SELECT`**: Indica que queremos seleccionar datos de la base de datos.
- **`f.title`**: Especificamos que queremos seleccionar la columna `title` de la tabla `film`, que contiene el título de la película.
- **`f_prnt.title AS prequel`**: También seleccionamos la columna `title` de la tabla `film`, pero le damos un alias (`prequel`) para indicar que es el título de la precuela.

---

#### 2. **`FROM`**
```sql
FROM film f
```
- Especificamos que estamos trabajando con la tabla `film`, que contiene información sobre las películas. Le damos un alias (`f`) para simplificar la consulta.

---

#### 3. **`INNER JOIN`**
```sql
INNER JOIN film f_prnt
ON f_prnt.film_id = f.prequel_film_id
```
- **`INNER JOIN film f_prnt`**: Combinamos la tabla `film` consigo misma. Esto se conoce como un **self-join** y se utiliza para relacionar filas dentro de la misma tabla.
- **`ON f_prnt.film_id = f.prequel_film_id`**: Especificamos cómo se relacionan las filas. En este caso, la columna `film_id` en la tabla `f_prnt` debe coincidir con la columna `prequel_film_id` en la tabla `f`. Esto nos permite relacionar una película con su precuela.
- **Alias `f_prnt`**: Le damos un alias (`f_prnt`) a la segunda instancia de la tabla `film` para simplificar la consulta.

---

#### 4. **`WHERE`**
```sql
WHERE f.prequel_film_id IS NOT NULL
```
- **`f.prequel_film_id IS NOT NULL`**: Filtra las filas donde la columna `prequel_film_id` no es `NULL`. Esto significa que solo se incluirán las películas que tienen una precuela definida.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Combina la tabla `film` consigo misma**: Usando un `INNER JOIN`, relacionamos una película con su precuela.
2. **Filtra por películas con precuela**: Solo se incluyen las películas que tienen una precuela definida (`prequel_film_id IS NOT NULL`).
3. **Selecciona el título de la película y su precuela**: Obtenemos el título de la película y el título de su precuela.

---

### **Ejemplo Práctico**

Imagina que tienes la siguiente tabla `film`:

| film_id | title               | prequel_film_id |
|---------|---------------------|-----------------|
| 1       | Movie A             | NULL            |
| 2       | Movie B             | 1               |
| 3       | Movie C             | 2               |
| 4       | Movie D             | NULL            |

---

#### Resultado de la Consulta
La consulta devolverá:

| title     | prequel  |
|-----------|----------|
| Movie B   | Movie A  |
| Movie C   | Movie B  |

- **Explicación**:
    - `'Movie B'` tiene una precuela (`prequel_film_id = 1`), que es `'Movie A'`.
    - `'Movie C'` tiene una precuela (`prequel_film_id = 2`), que es `'Movie B'`.
    - `'Movie A'` y `'Movie D'` no tienen una precuela definida (`prequel_film_id IS NULL`), por lo que no se incluyen en los resultados.

---

### **Resumen**

- **`SELECT`**: Selecciona el título de la película y el título de su precuela.
- **`FROM`**: Especifica que estamos trabajando con la tabla `film`.
- **`INNER JOIN`**: Combina la tabla `film` consigo misma para relacionar una película con su precuela.
- **`WHERE`**: Filtra las películas que tienen una precuela definida.

---

### **Notas Importantes**

1. **Self-Join**:
    - Un **self-join** es una técnica en SQL que permite combinar una tabla consigo misma. Es útil cuando una tabla tiene una relación consigo misma, como en este caso, donde una película puede tener una precuela que también está en la misma tabla.

2. **Alias**:
    - Los alias (`f` y `f_prnt`) se utilizan para diferenciar las dos instancias de la tabla `film` y hacer la consulta más legible.

3. **`IS NOT NULL`**:
    - La condición `IS NOT NULL` se utiliza para filtrar filas donde una columna tiene un valor asignado (no es `NULL`).

---


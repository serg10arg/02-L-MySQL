
### **Consulta SQL**

```sql
SELECT f.title
FROM film f
  INNER JOIN film_actor fa
  ON f.film_id = fa.film_id
  INNER JOIN actor a
  ON fa.actor_id = a.actor_id
WHERE ((a.first_name = 'CATE' AND a.last_name = 'MCQUEEN')
    OR (a.first_name = 'CUBA' AND a.last_name = 'BIRCH'));
```

### **Explicación Paso a Paso**

---

#### 1. **`SELECT`**
```sql
SELECT f.title
```
- **`SELECT`**: Indica que queremos seleccionar datos de la base de datos.
- **`f.title`**: Especificamos que queremos seleccionar la columna `title` de la tabla `film`, que contiene el título de la película.

---

#### 2. **`FROM`**
```sql
FROM film f
```
- Especificamos que estamos trabajando con la tabla `film`, que contiene información sobre las películas. Le damos un alias (`f`) para simplificar la consulta.

---

#### 3. **Primer `INNER JOIN`**
```sql
INNER JOIN film_actor fa
ON f.film_id = fa.film_id
```
- **`INNER JOIN film_actor fa`**: Combinamos la tabla `film` con la tabla `film_actor`. Un `INNER JOIN` solo incluye las filas que tienen coincidencias en ambas tablas.
- **`ON f.film_id = fa.film_id`**: Especificamos cómo se relacionan las tablas. En este caso, la columna `film_id` en la tabla `film` debe coincidir con la columna `film_id` en la tabla `film_actor`.
- **Alias `fa`**: Le damos un alias (`fa`) a la tabla `film_actor` para simplificar la consulta.

---

#### 4. **Segundo `INNER JOIN`**
```sql
INNER JOIN actor a
ON fa.actor_id = a.actor_id
```
- **`INNER JOIN actor a`**: Combinamos la tabla `film_actor` con la tabla `actor`. Un `INNER JOIN` solo incluye las filas que tienen coincidencias en ambas tablas.
- **`ON fa.actor_id = a.actor_id`**: Especificamos cómo se relacionan las tablas. En este caso, la columna `actor_id` en la tabla `film_actor` debe coincidir con la columna `actor_id` en la tabla `actor`.
- **Alias `a`**: Le damos un alias (`a`) a la tabla `actor` para simplificar la consulta.

---

#### 5. **`WHERE`**
```sql
WHERE ((a.first_name = 'CATE' AND a.last_name = 'MCQUEEN')
    OR (a.first_name = 'CUBA' AND a.last_name = 'BIRCH'))
```
- **`a.first_name = 'CATE' AND a.last_name = 'MCQUEEN'`**: Filtra los actores cuyo nombre es `'CATE'` y apellido es `'MCQUEEN'`.
- **`a.first_name = 'CUBA' AND a.last_name = 'BIRCH'`**: Filtra los actores cuyo nombre es `'CUBA'` y apellido es `'BIRCH'`.
- **`OR`**: Indica que se deben incluir las películas en las que actúe **cualquiera** de los dos actores.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Combina las tablas `film`, `film_actor` y `actor`**: Usando `INNER JOIN`, relacionamos las películas con los actores que participan en ellas.
2. **Filtra por actores específicos**: Solo se incluyen las películas en las que actúan `'CATE MCQUEEN'` o `'CUBA BIRCH'`.
3. **Selecciona el título de las películas**: Obtenemos el título de las películas que cumplen con las condiciones anteriores.

---

### **Ejemplo Práctico**

Imagina que tienes las siguientes tablas:

#### Tabla `film`
| film_id | title               |
|---------|---------------------|
| 1       | Movie A             |
| 2       | Movie B             |
| 3       | Movie C             |

#### Tabla `film_actor`
| film_id | actor_id |
|---------|----------|
| 1       | 101      |
| 2       | 102      |
| 3       | 101      |

#### Tabla `actor`
| actor_id | first_name | last_name |
|----------|------------|-----------|
| 101      | CATE       | MCQUEEN   |
| 102      | CUBA       | BIRCH     |

---

#### Resultado de la Consulta
La consulta devolverá:

| title     |
|-----------|
| Movie A   |
| Movie B   |
| Movie C   |

- **Explicación**:
    - `'CATE MCQUEEN'` actúa en `'Movie A'` y `'Movie C'`.
    - `'CUBA BIRCH'` actúa en `'Movie B'`.
    - Por lo tanto, se incluyen las películas `'Movie A'`, `'Movie B'` y `'Movie C'`.

---

### **Resumen**

- **`SELECT`**: Selecciona el título de las películas.
- **`FROM`**: Especifica que estamos trabajando con la tabla `film`.
- **`INNER JOIN`**: Combina las tablas `film`, `film_actor` y `actor` para relacionar películas con actores.
- **`WHERE`**: Filtra las películas en las que actúan `'CATE MCQUEEN'` o `'CUBA BIRCH'`.

---

### **Notas Importantes**

1. **Uso de `INNER JOIN`**:
    - Los `INNER JOIN` aseguran que solo se incluyan las películas que tienen actores asociados en las tablas `film_actor` y `actor`.

2. **Condiciones en `WHERE`**:
    - Las condiciones en `WHERE` pueden combinarse usando `AND` y `OR` para filtrar resultados más específicos.

3. **Alias**:
    - Los alias (`f`, `fa`, `a`) se utilizan para simplificar la consulta y hacerla más legible.

---


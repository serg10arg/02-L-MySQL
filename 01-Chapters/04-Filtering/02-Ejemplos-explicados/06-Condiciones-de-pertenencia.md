
### **Primera Consulta SQL**

```sql
SELECT title, rating
FROM film
WHERE rating = 'G' OR rating = 'PG';
```

### **Explicación Paso a Paso**

---

#### 1. **`SELECT`**
```sql
SELECT title, rating
```
- **`SELECT`**: Indica que queremos seleccionar datos de la base de datos.
- **`title`**: Especificamos que queremos seleccionar la columna `title`, que contiene el título de la película.
- **`rating`**: También seleccionamos la columna `rating`, que contiene la clasificación de la película (por ejemplo, `G`, `PG`, etc.).

---

#### 2. **`FROM`**
```sql
FROM film
```
- Especificamos que estamos trabajando con la tabla `film`, que contiene información sobre las películas.

---

#### 3. **`WHERE`**
```sql
WHERE rating = 'G' OR rating = 'PG'
```
- **`rating = 'G'`**: Filtra las películas que tienen una clasificación `G` (apta para todos los públicos).
- **`rating = 'PG'`**: Filtra las películas que tienen una clasificación `PG` (recomendada para niños bajo supervisión de padres).
- **`OR`**: Indica que se deben incluir las películas que cumplen **cualquiera** de las dos condiciones.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Selecciona columnas específicas**: Obtenemos el título (`title`) y la clasificación (`rating`) de las películas.
2. **Filtra por clasificación**: Solo se incluyen las películas que tienen una clasificación `G` o `PG`.

---

### **Segunda Consulta SQL**

```sql
SELECT title, rating
FROM film
WHERE rating IN ('G', 'PG');
```

### **Explicación Paso a Paso**

---

#### 1. **`SELECT`**
```sql
SELECT title, rating
```
- **`SELECT`**: Indica que queremos seleccionar datos de la base de datos.
- **`title`**: Especificamos que queremos seleccionar la columna `title`, que contiene el título de la película.
- **`rating`**: También seleccionamos la columna `rating`, que contiene la clasificación de la película (por ejemplo, `G`, `PG`, etc.).

---

#### 2. **`FROM`**
```sql
FROM film
```
- Especificamos que estamos trabajando con la tabla `film`, que contiene información sobre las películas.

---

#### 3. **`WHERE`**
```sql
WHERE rating IN ('G', 'PG')
```
- **`IN ('G', 'PG')`**: Esta cláusula filtra las películas cuya clasificación (`rating`) esté en la lista proporcionada (`'G'` o `'PG'`). Es equivalente a usar `rating = 'G' OR rating = 'PG'`, pero es más conciso y fácil de leer.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Selecciona columnas específicas**: Obtenemos el título (`title`) y la clasificación (`rating`) de las películas.
2. **Filtra por clasificación**: Solo se incluyen las películas que tienen una clasificación `G` o `PG`.

---

### **Ejemplo Práctico**

Imagina que tienes la siguiente tabla `film`:

| title               | rating |
|---------------------|--------|
| Toy Story           | G      |
| Frozen              | PG     |
| The Dark Knight     | PG-13  |
| Inception           | PG-13  |
| Finding Nemo        | G      |
| Jurassic Park       | PG-13  |

---

#### Resultado de Ambas Consultas
Ambas consultas devolverán:

| title               | rating |
|---------------------|--------|
| Toy Story           | G      |
| Frozen              | PG     |
| Finding Nemo        | G      |

- **Explicación**:
    - Las películas `'Toy Story'`, `'Frozen'` y `'Finding Nemo'` tienen una clasificación `G` o `PG`, por lo que se incluyen en los resultados.
    - Las películas `'The Dark Knight'`, `'Inception'` y `'Jurassic Park'` tienen una clasificación `PG-13`, por lo que no se incluyen.

---

### **Resumen**

- **Primera Consulta**:
    - **`SELECT`**: Selecciona el título y la clasificación de las películas.
    - **`FROM`**: Especifica que estamos trabajando con la tabla `film`.
    - **`WHERE`**: Filtra las películas que tienen una clasificación `G` o `PG` usando `OR`.

- **Segunda Consulta**:
    - **`SELECT`**: Selecciona el título y la clasificación de las películas.
    - **`FROM`**: Especifica que estamos trabajando con la tabla `film`.
    - **`WHERE`**: Filtra las películas que tienen una clasificación `G` o `PG` usando `IN`.

---

### **Diferencia entre `OR` y `IN`**

- **`OR`**: Es útil cuando tienes pocas condiciones y quieres ser explícito.
- **`IN`**: Es más conciso y fácil de leer cuando tienes múltiples valores para comparar.

Ambas consultas son equivalentes y devuelven los mismos resultados. La elección entre una y otra depende de la preferencia personal y la claridad del código.

---


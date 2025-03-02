
### **Consulta SQL**

```sql
SELECT title, rating
FROM film
WHERE rating NOT IN ('PG-13', 'R', 'NC-17');
```

### **Explicación Paso a Paso**

---

#### 1. **`SELECT`**
```sql
SELECT title, rating
```
- **`SELECT`**: Indica que queremos seleccionar datos de la base de datos.
- **`title`**: Especificamos que queremos seleccionar la columna `title`, que contiene el título de la película.
- **`rating`**: También seleccionamos la columna `rating`, que contiene la clasificación de la película (por ejemplo, `G`, `PG`, `PG-13`, etc.).

---

#### 2. **`FROM`**
```sql
FROM film
```
- Especificamos que estamos trabajando con la tabla `film`, que contiene información sobre las películas.

---

#### 3. **`WHERE`**
```sql
WHERE rating NOT IN ('PG-13', 'R', 'NC-17')
```
- **`NOT IN`**: Esta cláusula filtra los resultados para **excluir** las películas cuya clasificación (`rating`) esté en la lista proporcionada (`'PG-13'`, `'R'`, `'NC-17'`).
- **`'PG-13'`, `'R'`, `'NC-17'`**: Estas son las clasificaciones que queremos excluir de los resultados.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Selecciona columnas específicas**: Obtenemos el título (`title`) y la clasificación (`rating`) de las películas.
2. **Filtra por clasificación**: Excluye las películas que tienen una clasificación `PG-13`, `R` o `NC-17`. Es decir, solo se incluyen las películas con clasificaciones **diferentes** a estas.

---

### **Ejemplo Práctico**

Imagina que tienes la siguiente tabla `film`:

| title               | rating  |
|---------------------|---------|
| Toy Story           | G       |
| Frozen              | PG      |
| The Dark Knight     | PG-13   |
| Inception           | PG-13   |
| Finding Nemo        | G       |
| Jurassic Park       | PG-13   |
| The Matrix          | R       |
| Pulp Fiction        | R       |
| Saw                 | NC-17   |

---

#### Resultado de la Consulta
La consulta devolverá:

| title               | rating  |
|---------------------|---------|
| Toy Story           | G       |
| Frozen              | PG      |
| Finding Nemo        | G       |

- **Explicación**:
    - Las películas `'Toy Story'`, `'Frozen'` y `'Finding Nemo'` tienen clasificaciones `G` y `PG`, que **no** están en la lista de exclusiones (`'PG-13'`, `'R'`, `'NC-17'`), por lo que se incluyen en los resultados.
    - Las películas `'The Dark Knight'`, `'Inception'`, `'Jurassic Park'`, `'The Matrix'`, `'Pulp Fiction'` y `'Saw'` tienen clasificaciones `PG-13`, `R` o `NC-17`, por lo que **no** se incluyen.

---

### **Resumen**

- **`SELECT`**: Selecciona el título y la clasificación de las películas.
- **`FROM`**: Especifica que estamos trabajando con la tabla `film`.
- **`WHERE`**: Filtra las películas para excluir aquellas con clasificaciones `PG-13`, `R` o `NC-17`.

---

### **Notas Importantes**

1. **Uso de `NOT IN`**:
    - La cláusula `NOT IN` es útil para excluir valores específicos de una lista.
    - En este caso, se utiliza para excluir películas con clasificaciones `PG-13`, `R` o `NC-17`.

2. **Clasificaciones comunes**:
    - `G`: Apta para todos los públicos.
    - `PG`: Recomendada para niños bajo supervisión de padres.
    - `PG-13`: No recomendada para menores de 13 años.
    - `R`: Restringida a mayores de 17 años (o con acompañante adulto).
    - `NC-17**: Solo para mayores de 18 años.

---


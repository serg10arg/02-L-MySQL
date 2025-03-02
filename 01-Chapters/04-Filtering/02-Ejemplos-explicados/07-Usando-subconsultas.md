
### **Consulta SQL**

```sql
SELECT title, rating
FROM film
WHERE rating IN (SELECT rating FROM film WHERE title LIKE '%PET%');
```

### **Explicación Paso a Paso**

---

#### 1. **Subconsulta**
```sql
(SELECT rating FROM film WHERE title LIKE '%PET%')
```
- **`SELECT rating`**: Selecciona la columna `rating` de la tabla `film`.
- **`FROM film`**: Especifica que estamos trabajando con la tabla `film`.
- **`WHERE title LIKE '%PET%'`**: Filtra las películas cuyo título (`title`) contiene la cadena `'PET'` en cualquier parte del título. El operador `%` es un comodín que significa "cualquier secuencia de caracteres".
- **Resultado de la subconsulta**: Esta subconsulta devuelve una lista de clasificaciones (`rating`) de las películas cuyo título contiene `'PET'`.

---

#### 2. **Consulta Principal**
```sql
SELECT title, rating
FROM film
WHERE rating IN (...)
```
- **`SELECT title, rating`**: Selecciona las columnas `title` (título) y `rating` (clasificación) de la tabla `film`.
- **`FROM film`**: Especifica que estamos trabajando con la tabla `film`.
- **`WHERE rating IN (...)`**: Filtra las películas cuya clasificación (`rating`) esté en la lista devuelta por la subconsulta. Es decir, solo se incluirán las películas que tengan la misma clasificación que las películas cuyo título contiene `'PET'`.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Subconsulta**: Encuentra las clasificaciones (`rating`) de las películas cuyo título contiene `'PET'`.
2. **Consulta Principal**: Selecciona el título y la clasificación de todas las películas que tienen la misma clasificación que las películas encontradas en la subconsulta.

---

### **Ejemplo Práctico**

Imagina que tienes la siguiente tabla `film`:

| title               | rating |
|---------------------|--------|
| Pet Detective       | PG     |
| Pet Stories         | G      |
| The Dark Knight     | PG-13  |
| Inception           | PG-13  |
| Finding Nemo        | G      |
| Jurassic Park       | PG-13  |

---

#### Resultado de la Subconsulta
La subconsulta:

```sql
SELECT rating FROM film WHERE title LIKE '%PET%'
```

Devolverá:

| rating |
|--------|
| PG     |
| G      |

- **Explicación**:
    - Las películas `'Pet Detective'` y `'Pet Stories'` contienen `'PET'` en su título, por lo que sus clasificaciones (`PG` y `G`) se incluyen en el resultado de la subconsulta.

---

#### Resultado de la Consulta Principal
La consulta principal:

```sql
SELECT title, rating
FROM film
WHERE rating IN ('PG', 'G');
```

Devolverá:

| title               | rating |
|---------------------|--------|
| Pet Detective       | PG     |
| Pet Stories         | G      |
| Finding Nemo        | G      |

- **Explicación**:
    - Las películas `'Pet Detective'`, `'Pet Stories'` y `'Finding Nemo'` tienen una clasificación `PG` o `G`, que son las clasificaciones devueltas por la subconsulta.

---

### **Resumen**

- **Subconsulta**: Encuentra las clasificaciones de las películas cuyo título contiene `'PET'`.
- **Consulta Principal**: Selecciona el título y la clasificación de todas las películas que tienen la misma clasificación que las películas encontradas en la subconsulta.

---

### **Notas Importantes**

1. **Uso de `IN`**:
    - La cláusula `IN` es útil para comparar un valor con una lista de valores devueltos por una subconsulta.
    - En este caso, se utiliza para filtrar las películas que tienen la misma clasificación que las películas cuyo título contiene `'PET'`.

2. **Comodín `%`**:
    - El operador `%` en `LIKE '%PET%'` significa "cualquier secuencia de caracteres". Por lo tanto, `'%PET%'` coincide con cualquier título que contenga `'PET'` en cualquier parte del texto.

---


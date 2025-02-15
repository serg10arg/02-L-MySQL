
### **Consulta SQL**

```sql
SELECT a.first_name, a.last_name
FROM actor a
WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
EXCEPT
SELECT c.first_name, c.last_name
FROM customer c
WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%';
```

### **Explicación Paso a Paso**

---

#### 1. **Primera Consulta**
```sql
SELECT a.first_name, a.last_name
FROM actor a
WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
```
- **`SELECT a.first_name, a.last_name`**: Selecciona las columnas `first_name` y `last_name` de la tabla `actor`.
- **`FROM actor a`**: Especifica que estamos trabajando con la tabla `actor`, que contiene información sobre los actores. Le damos un alias (`a`) para simplificar la consulta.
- **`WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'`**: Filtra los actores cuyo nombre comienza con la letra `'J'` y cuyo apellido comienza con la letra `'D'`.

---

#### 2. **Segunda Consulta**
```sql
SELECT c.first_name, c.last_name
FROM customer c
WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'
```
- **`SELECT c.first_name, c.last_name`**: Selecciona las columnas `first_name` y `last_name` de la tabla `customer`.
- **`FROM customer c`**: Especifica que estamos trabajando con la tabla `customer`, que contiene información sobre los clientes. Le damos un alias (`c`) para simplificar la consulta.
- **`WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'`**: Filtra los clientes cuyo nombre comienza con la letra `'J'` y cuyo apellido comienza con la letra `'D'`.

---

#### 3. **`EXCEPT`**
```sql
EXCEPT
```
- **`EXCEPT`**: Es un operador de conjuntos que devuelve las filas de la primera consulta que **no** están presentes en la segunda consulta. En otras palabras, devuelve los resultados de la primera consulta **menos** los resultados de la segunda consulta.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Primera Consulta**: Selecciona los nombres y apellidos de los actores cuyo nombre comienza con `'J'` y cuyo apellido comienza con `'D'`.
2. **Segunda Consulta**: Selecciona los nombres y apellidos de los clientes cuyo nombre comienza con `'J'` y cuyo apellido comienza con `'D'`.
3. **`EXCEPT`**: Devuelve los actores que cumplen con las condiciones de la primera consulta, pero **no** están presentes en los resultados de la segunda consulta.

---

### **Ejemplo Práctico**

Imagina que tienes las siguientes tablas:

#### Tabla `actor`
| actor_id | first_name | last_name |
|----------|------------|-----------|
| 101      | Johnny     | Depp      |
| 102      | Julia      | Roberts   |
| 103      | James      | Dean      |

#### Tabla `customer`
| customer_id | first_name | last_name |
|-------------|------------|-----------|
| 1           | John       | Doe       |
| 2           | Jane       | Smith     |
| 3           | James      | Dean      |

---

#### Resultado de la Primera Consulta
La primera consulta:

```sql
SELECT a.first_name, a.last_name
FROM actor a
WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
```

Devolverá:

| first_name | last_name |
|------------|-----------|
| Johnny     | Depp      |
| James      | Dean      |

---

#### Resultado de la Segunda Consulta
La segunda consulta:

```sql
SELECT c.first_name, c.last_name
FROM customer c
WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'
```

Devolverá:

| first_name | last_name |
|------------|-----------|
| James      | Dean      |

---

#### Resultado Final con `EXCEPT`
La consulta completa devolverá:

| first_name | last_name |
|------------|-----------|
| Johnny     | Depp      |

- **Explicación**:
    - `'Johnny Depp'` es un actor que cumple con las condiciones de la primera consulta, pero no está presente en los resultados de la segunda consulta.
    - `'James Dean'` es un actor que cumple con las condiciones de la primera consulta, pero también está presente en los resultados de la segunda consulta, por lo que se excluye del resultado final.

---

### **Resumen**

- **Primera Consulta**: Selecciona nombres y apellidos de actores que cumplen con las condiciones.
- **Segunda Consulta**: Selecciona nombres y apellidos de clientes que cumplen con las condiciones.
- **`EXCEPT`**: Devuelve los actores que cumplen con las condiciones de la primera consulta, pero no están presentes en los resultados de la segunda consulta.

---

### **Notas Importantes**

1. **`LIKE 'J%'` y `LIKE 'D%'`**:
    - `LIKE 'J%'` filtra nombres que comienzan con la letra `'J'`.
    - `LIKE 'D%'` filtra apellidos que comienzan con la letra `'D'`.

2. **`EXCEPT`**:
    - El operador `EXCEPT` es útil para encontrar diferencias entre dos conjuntos de resultados.
    - Solo devuelve filas que están en la primera consulta pero no en la segunda.

3. **Alias**:
    - Los alias (`a` y `c`) se utilizan para simplificar la consulta y hacerla más legible.

---


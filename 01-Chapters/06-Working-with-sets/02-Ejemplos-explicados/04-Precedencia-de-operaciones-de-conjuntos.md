
### **Consulta SQL**

```sql
SELECT a.first_name, a.last_name
FROM actor a
WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
UNION ALL
SELECT a.first_name, a.last_name
FROM actor a
WHERE a.first_name LIKE 'M%' AND a.last_name LIKE 'T%'
UNION
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
SELECT a.first_name, a.last_name
FROM actor a
WHERE a.first_name LIKE 'M%' AND a.last_name LIKE 'T%'
```
- **`SELECT a.first_name, a.last_name`**: Selecciona las columnas `first_name` y `last_name` de la tabla `actor`.
- **`FROM actor a`**: Especifica que estamos trabajando con la tabla `actor`, que contiene información sobre los actores. Le damos un alias (`a`) para simplificar la consulta.
- **`WHERE a.first_name LIKE 'M%' AND a.last_name LIKE 'T%'`**: Filtra los actores cuyo nombre comienza con la letra `'M'` y cuyo apellido comienza con la letra `'T'`.

---

#### 3. **Tercera Consulta**
```sql
SELECT c.first_name, c.last_name
FROM customer c
WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'
```
- **`SELECT c.first_name, c.last_name`**: Selecciona las columnas `first_name` y `last_name` de la tabla `customer`.
- **`FROM customer c`**: Especifica que estamos trabajando con la tabla `customer`, que contiene información sobre los clientes. Le damos un alias (`c`) para simplificar la consulta.
- **`WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'`**: Filtra los clientes cuyo nombre comienza con la letra `'J'` y cuyo apellido comienza con la letra `'D'`.

---

#### 4. **`UNION ALL` y `UNION`**
```sql
UNION ALL
UNION
```
- **`UNION ALL`**: Combina los resultados de la primera y segunda consulta. A diferencia de `UNION`, `UNION ALL` no elimina duplicados, por lo que si hay filas idénticas en ambas consultas, se incluirán en el resultado final.
- **`UNION`**: Combina los resultados de las dos primeras consultas (ya combinadas con `UNION ALL`) con los resultados de la tercera consulta. `UNION` elimina duplicados, por lo que solo incluirá filas únicas.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Primera Consulta**: Selecciona los nombres y apellidos de los actores cuyo nombre comienza con `'J'` y cuyo apellido comienza con `'D'`.
2. **Segunda Consulta**: Selecciona los nombres y apellidos de los actores cuyo nombre comienza con `'M'` y cuyo apellido comienza con `'T'`.
3. **Tercera Consulta**: Selecciona los nombres y apellidos de los clientes cuyo nombre comienza con `'J'` y cuyo apellido comienza con `'D'`.
4. **`UNION ALL`**: Combina los resultados de la primera y segunda consulta, incluyendo duplicados si los hay.
5. **`UNION`**: Combina los resultados de las dos primeras consultas (ya combinadas con `UNION ALL`) con los resultados de la tercera consulta, eliminando duplicados.

---

### **Ejemplo Práctico**

Imagina que tienes las siguientes tablas:

#### Tabla `actor`
| actor_id | first_name | last_name |
|----------|------------|-----------|
| 101      | Johnny     | Depp      |
| 102      | Julia      | Roberts   |
| 103      | James      | Dean      |
| 104      | Michael    | Thompson  |

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
SELECT a.first_name, a.last_name
FROM actor a
WHERE a.first_name LIKE 'M%' AND a.last_name LIKE 'T%'
```

Devolverá:

| first_name | last_name |
|------------|-----------|
| Michael    | Thompson  |

---

#### Resultado de la Tercera Consulta
La tercera consulta:

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

#### Resultado Final con `UNION ALL` y `UNION`
La consulta completa devolverá:

| first_name | last_name |
|------------|-----------|
| Johnny     | Depp      |
| James      | Dean      |
| Michael    | Thompson  |

- **Explicación**:
    - `'Johnny Depp'` y `'James Dean'` son actores que cumplen con las condiciones de la primera consulta.
    - `'Michael Thompson'` es un actor que cumple con las condiciones de la segunda consulta.
    - `'James Dean'` es un cliente que cumple con las condiciones de la tercera consulta, pero ya está presente en los resultados de la primera consulta, por lo que `UNION` lo elimina como duplicado.

---

### **Resumen**

- **Primera Consulta**: Selecciona nombres y apellidos de actores que cumplen con las condiciones.
- **Segunda Consulta**: Selecciona nombres y apellidos de actores que cumplen con otras condiciones.
- **Tercera Consulta**: Selecciona nombres y apellidos de clientes que cumplen con las condiciones.
- **`UNION ALL`**: Combina los resultados de la primera y segunda consulta, incluyendo duplicados.
- **`UNION`**: Combina los resultados de las dos primeras consultas (ya combinadas con `UNION ALL`) con los resultados de la tercera consulta, eliminando duplicados.

---

### **Notas Importantes**

1. **`LIKE 'J%'`, `LIKE 'D%'`, `LIKE 'M%'`, `LIKE 'T%'`**:
    - `LIKE 'J%'` filtra nombres que comienzan con la letra `'J'`.
    - `LIKE 'D%'` filtra apellidos que comienzan con la letra `'D'`.
    - `LIKE 'M%'` filtra nombres que comienzan con la letra `'M'`.
    - `LIKE 'T%'` filtra apellidos que comienzan con la letra `'T'`.

2. **`UNION ALL` vs `UNION`**:
    - `UNION ALL` incluye todas las filas de ambas consultas, incluso si hay duplicados.
    - `UNION` elimina duplicados, por lo que solo incluiría filas únicas.

3. **Alias**:
    - Los alias (`a` y `c`) se utilizan para simplificar la consulta y hacerla más legible.

---


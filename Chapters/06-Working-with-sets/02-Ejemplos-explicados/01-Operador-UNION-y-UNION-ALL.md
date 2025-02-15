
### **Consulta SQL**

```sql
SELECT c.first_name, c.last_name
FROM customer c
WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'
UNION ALL
SELECT a.first_name, a.last_name
FROM actor a
WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%';
```

### **Explicación Paso a Paso**

---

#### 1. **Primera Consulta**
```sql
SELECT c.first_name, c.last_name
FROM customer c
WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'
```
- **`SELECT c.first_name, c.last_name`**: Selecciona las columnas `first_name` y `last_name` de la tabla `customer`.
- **`FROM customer c`**: Especifica que estamos trabajando con la tabla `customer`, que contiene información sobre los clientes. Le damos un alias (`c`) para simplificar la consulta.
- **`WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'`**: Filtra los clientes cuyo nombre comienza con la letra `'J'` y cuyo apellido comienza con la letra `'D'`.

---

#### 2. **Segunda Consulta**
```sql
SELECT a.first_name, a.last_name
FROM actor a
WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
```
- **`SELECT a.first_name, a.last_name`**: Selecciona las columnas `first_name` y `last_name` de la tabla `actor`.
- **`FROM actor a`**: Especifica que estamos trabajando con la tabla `actor`, que contiene información sobre los actores. Le damos un alias (`a`) para simplificar la consulta.
- **`WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'`**: Filtra los actores cuyo nombre comienza con la letra `'J'` y cuyo apellido comienza con la letra `'D'`.

---

#### 3. **`UNION ALL`**
```sql
UNION ALL
```
- **`UNION ALL`**: Combina los resultados de las dos consultas anteriores. A diferencia de `UNION`, `UNION ALL` no elimina duplicados, por lo que si hay filas idénticas en ambas consultas, se incluirán en el resultado final.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Primera Consulta**: Selecciona los nombres y apellidos de los clientes cuyo nombre comienza con `'J'` y cuyo apellido comienza con `'D'`.
2. **Segunda Consulta**: Selecciona los nombres y apellidos de los actores cuyo nombre comienza con `'J'` y cuyo apellido comienza con `'D'`.
3. **`UNION ALL`**: Combina los resultados de ambas consultas en un solo conjunto de resultados, incluyendo duplicados si los hay.

---

### **Ejemplo Práctico**

Imagina que tienes las siguientes tablas:

#### Tabla `customer`
| customer_id | first_name | last_name |
|-------------|------------|-----------|
| 1           | John       | Doe       |
| 2           | Jane       | Smith     |
| 3           | James      | Dean      |

#### Tabla `actor`
| actor_id | first_name | last_name |
|----------|------------|-----------|
| 101      | Julia      | Roberts   |
| 102      | Johnny     | Depp      |
| 103      | James      | Dean      |

---

#### Resultado de la Primera Consulta
La primera consulta:

```sql
SELECT c.first_name, c.last_name
FROM customer c
WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'
```

Devolverá:

| first_name | last_name |
|------------|-----------|
| John       | Doe       |
| James      | Dean      |

---

#### Resultado de la Segunda Consulta
La segunda consulta:

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

#### Resultado Final con `UNION ALL`
La consulta completa devolverá:

| first_name | last_name |
|------------|-----------|
| John       | Doe       |
| James      | Dean      |
| Johnny     | Depp      |
| James      | Dean      |

- **Explicación**:
    - `'John Doe'` y `'James Dean'` son clientes que cumplen con las condiciones.
    - `'Johnny Depp'` y `'James Dean'` son actores que cumplen con las condiciones.
    - `'James Dean'` aparece dos veces porque está presente en ambas tablas y `UNION ALL` no elimina duplicados.

---

### **Resumen**

- **Primera Consulta**: Selecciona nombres y apellidos de clientes que cumplen con las condiciones.
- **Segunda Consulta**: Selecciona nombres y apellidos de actores que cumplen con las condiciones.
- **`UNION ALL`**: Combina los resultados de ambas consultas, incluyendo duplicados.

---

### **Notas Importantes**

1. **`LIKE 'J%'` y `LIKE 'D%'`**:
    - `LIKE 'J%'` filtra nombres que comienzan con la letra `'J'`.
    - `LIKE 'D%'` filtra apellidos que comienzan con la letra `'D'`.

2. **`UNION ALL` vs `UNION`**:
    - `UNION ALL` incluye todas las filas de ambas consultas, incluso si hay duplicados.
    - `UNION` elimina duplicados, por lo que solo incluiría filas únicas.

3. **Alias**:
    - Los alias (`c` y `a`) se utilizan para simplificar la consulta y hacerla más legible.

---

### 1. **Estructura General**

La sentencia que tienes aquí es una combinación de dos consultas `SELECT` unidas con la palabra clave `UNION`. Esto significa que estamos combinando los resultados de dos consultas diferentes en una sola tabla de resultados.

### 2. **Primera Consulta**

```sql
SELECT c.first_name, c.last_name
FROM customer c
WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%';
```

### **SELECT**

- `SELECT c.first_name, c.last_name`: Esto nos dice que queremos obtener dos columnas: `first_name` (nombre) y `last_name` (apellido) de una tabla llamada `customer`.
- `c` es un alias para la tabla `customer`. Es como un apodo que usamos para referirnos a la tabla de manera más corta en la consulta.

### **FROM**

- `FROM customer c`: Indica que los datos provienen de la tabla `customer`.

### **WHERE**

- `WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'`: Esta es una condición que filtra los resultados.
    - `c.first_name LIKE 'J%'`: Esto significa que queremos nombres que comiencen con la letra "J". El símbolo `%` es un comodín que representa cualquier secuencia de caracteres después de la "J".
    - `c.last_name LIKE 'D%'`: Esto significa que queremos apellidos que comiencen con la letra "D".
    - `AND`: Ambas condiciones deben cumplirse al mismo tiempo. Es decir, buscamos clientes cuyos nombres comiencen con "J" y sus apellidos comiencen con "D".

### 3. **Segunda Consulta**

```sql
SELECT a.first_name, a.last_name
FROM actor a
WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%';
```

Esta segunda consulta es muy similar a la primera, pero se ejecuta en una tabla diferente llamada `actor`.

- `a` es un alias para la tabla `actor`.
- La condición `WHERE` es la misma: buscamos nombres que comiencen con "J" y apellidos que comiencen con "D".

### 4. **UNION**

`UNION`

La palabra clave `UNION` se utiliza para combinar los resultados de las dos consultas. Aquí es importante tener en cuenta dos cosas:

- **Columnas**: Ambas consultas deben devolver el mismo número de columnas y tipos de datos compatibles. En este caso, ambas devuelven dos columnas: `first_name` y `last_name`.
- **Duplicados**: `UNION` por defecto elimina los duplicados. Si no quieres eliminar duplicados, puedes usar `UNION ALL`.

### **Resumen de la Consulta**

Esta sentencia SQL está buscando:

1. Todos los clientes en la tabla `customer` cuyos nombres comiencen con "J" y apellidos comiencen con "D".
2. Todos los actores en la tabla `actor` cuyos nombres comiencen con "J" y apellidos comiencen con "D".
3. Luego combina estos resultados en una sola tabla, eliminando cualquier duplicado.

### **Ejemplo de Resultados**

Supongamos que tenemos estos datos en nuestras tablas:

### Tabla `customer`:

| first_name | last_name |
| --- | --- |
| John | Doe |
| Jane | Davis |
| Jack | Smith |

### Tabla `actor`:

| first_name | last_name |
| --- | --- |
| John | Doe |
| Jack | Daniels |
| Julia | Davis |

La consulta nos devolverá:

| first_name | last_name |
| --- | --- |
| John | Doe |
| Jane | Davis |
| Jack | Daniels |

Como puedes ver, "John Doe" aparece en ambas tablas, pero solo se muestra una vez en el resultado final debido a `UNION`.
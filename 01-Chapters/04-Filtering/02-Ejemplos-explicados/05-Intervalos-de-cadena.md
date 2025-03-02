
### **Consulta SQL**

```sql
SELECT last_name, first_name
FROM customer
WHERE last_name BETWEEN 'FA' AND 'FR';
```

### **Explicación Paso a Paso**

---

#### 1. **`SELECT`**
```sql
SELECT last_name, first_name
```
- **`SELECT`**: Indica que queremos seleccionar datos de la base de datos.
- **`last_name` y `first_name`**: Especificamos que queremos seleccionar las columnas `last_name` (apellido) y `first_name` (nombre) de la tabla `customer`.

---

#### 2. **`FROM`**
```sql
FROM customer
```
- Especificamos que estamos trabajando con la tabla `customer`, que contiene información sobre los clientes.

---

#### 3. **`WHERE`**
```sql
WHERE last_name BETWEEN 'FA' AND 'FR'
```
- **`BETWEEN 'FA' AND 'FR'`**: Esta cláusula filtra los resultados para incluir solo los clientes cuyo apellido (`last_name`) esté en el rango alfabético entre `'FA'` y `'FR'`, **inclusive**. Es decir, incluye los apellidos que comienzan con `'FA'`, `'FB'`, `'FC'`, ..., hasta `'FR'`.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Selecciona columnas específicas**: Obtenemos el apellido (`last_name`) y el nombre (`first_name`) de los clientes.
2. **Filtra por rango alfabético**: Solo se incluyen los clientes cuyo apellido esté en el rango alfabético entre `'FA'` y `'FR'`.

---

### **Ejemplo Práctico**

Imagina que tienes la siguiente tabla `customer`:

| customer_id | first_name | last_name |
|-------------|------------|-----------|
| 1           | John       | Faber     |
| 2           | Jane       | Fallon    |
| 3           | Alice      | Frost     |
| 4           | Bob        | Franklin  |
| 5           | Carol      | Fox       |
| 6           | David      | Garcia    |
| 7           | Eve        | Fisher    |

---

#### Resultado de la Consulta
La consulta devolverá:

| last_name | first_name |
|-----------|------------|
| Faber     | John       |
| Fallon    | Jane       |
| Franklin  | Bob        |
| Fisher    | Eve        |

- **Explicación**:
    - Los apellidos `'Faber'`, `'Fallon'`, `'Franklin'` y `'Fisher'` están dentro del rango alfabético entre `'FA'` y `'FR'`.
    - Los apellidos `'Frost'`, `'Fox'` y `'Garcia'` no están dentro de este rango, por lo que no se incluyen.

---

### **Resumen**

- **`SELECT`**: Selecciona el apellido y el nombre de los clientes.
- **`FROM`**: Especifica que estamos trabajando con la tabla `customer`.
- **`WHERE`**: Filtra los resultados para incluir solo los clientes cuyo apellido esté en el rango alfabético entre `'FA'` y `'FR'`.

---

### **Notas Importantes**

1. **Rango Alfabético**:
    - `BETWEEN` funciona comparando cadenas de texto alfabéticamente. En este caso, se incluyen todos los apellidos que comienzan con `'FA'`, `'FB'`, `'FC'`, ..., hasta `'FR'`.
    - Si un apellido comienza con `'FR'` pero es más largo (por ejemplo, `'Frost'`), no se incluirá porque `'Frost'` es alfabéticamente mayor que `'FR'`.

2. **Inclusividad**:
    - `BETWEEN` es inclusivo, lo que significa que incluye los valores límite (`'FA'` y `'FR'`).

---

### **Equivalencia con `>=` y `<=`**

Esta consulta es equivalente a:

```sql
SELECT last_name, first_name
FROM customer
WHERE last_name >= 'FA'
  AND last_name <= 'FR';
```

Ambas consultas devuelven los mismos resultados. La única diferencia es que `BETWEEN` es más conciso y fácil de leer cuando se trabaja con rangos.

---


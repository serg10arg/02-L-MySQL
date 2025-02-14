
### **Consulta SQL**

```sql
SELECT last_name, first_name
FROM customer
WHERE left(last_name, 1) = 'Q';
```

### **Explicación Paso a Paso**

---

#### 1. **`SELECT`**
```sql
SELECT last_name, first_name
```
- **`SELECT`**: Indica que queremos seleccionar datos de la base de datos.
- **`last_name`**: Especificamos que queremos seleccionar la columna `last_name`, que contiene el apellido del cliente.
- **`first_name`**: También seleccionamos la columna `first_name`, que contiene el nombre del cliente.

---

#### 2. **`FROM`**
```sql
FROM customer
```
- Especificamos que estamos trabajando con la tabla `customer`, que contiene información sobre los clientes.

---

#### 3. **`WHERE`**
```sql
WHERE left(last_name, 1) = 'Q'
```
- **`left(last_name, 1)`**: Esta función toma la columna `last_name` y extrae el primer carácter (el carácter más a la izquierda) del apellido.
- **`= 'Q'`**: Compara el primer carácter del apellido con la letra `'Q'`.
- **Condición**: Solo se incluirán los clientes cuyo apellido comience con la letra `'Q'`.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Selecciona columnas específicas**: Obtenemos el apellido (`last_name`) y el nombre (`first_name`) de los clientes.
2. **Filtra por la primera letra del apellido**: Solo se incluyen los clientes cuyo apellido comienza con la letra `'Q'`.

---

### **Ejemplo Práctico**

Imagina que tienes la siguiente tabla `customer`:

| customer_id | first_name | last_name |
|-------------|------------|-----------|
| 1           | John       | Quinn     |
| 2           | Jane       | Quigley   |
| 3           | Alice      | Smith     |
| 4           | Bob        | Quintero  |
| 5           | Carol      | Johnson   |
| 6           | David      | Quiroz    |
| 7           | Eve        | Brown     |

---

#### Resultado de la Consulta
La consulta devolverá:

| last_name | first_name |
|-----------|------------|
| Quinn     | John       |
| Quigley   | Jane       |
| Quintero  | Bob        |
| Quiroz    | David      |

- **Explicación**:
    - Los apellidos `'Quinn'`, `'Quigley'`, `'Quintero'` y `'Quiroz'` comienzan con la letra `'Q'`, por lo que se incluyen en los resultados.
    - Los apellidos `'Smith'`, `'Johnson'` y `'Brown'` no comienzan con `'Q'`, por lo que no se incluyen.

---

### **Resumen**

- **`SELECT`**: Selecciona el apellido y el nombre de los clientes.
- **`FROM`**: Especifica que estamos trabajando con la tabla `customer`.
- **`WHERE`**: Filtra los resultados para incluir solo los clientes cuyo apellido comienza con la letra `'Q'`.

---

### **Notas Importantes**

1. **Función `LEFT`**:
    - La función `LEFT(columna, n)` extrae los primeros `n` caracteres de la columna especificada.
    - En este caso, `LEFT(last_name, 1)` extrae el primer carácter del apellido.

2. **Comparación de cadenas**:
    - La comparación `= 'Q'` es sensible a mayúsculas y minúsculas. Si la base de datos distingue entre mayúsculas y minúsculas, solo coincidirán los apellidos que comiencen con `'Q'` (mayúscula).

---

### **Alternativa con `LIKE`**

Esta consulta también se puede escribir usando el operador `LIKE`:

```sql
SELECT last_name, first_name
FROM customer
WHERE last_name LIKE 'Q%';
```

- **`LIKE 'Q%'`**: Filtra los apellidos que comienzan con `'Q'`. El símbolo `%` es un comodín que significa "cualquier secuencia de caracteres".

Ambas consultas son equivalentes y devuelven los mismos resultados. La elección entre una y otra depende de la preferencia personal y la claridad del código.

---


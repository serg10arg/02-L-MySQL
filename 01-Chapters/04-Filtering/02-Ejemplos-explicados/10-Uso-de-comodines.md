
### **Consulta SQL**

```sql
SELECT last_name, first_name
FROM customer
WHERE last_name LIKE '_A_T%S';
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
WHERE last_name LIKE '_A_T%S'
```
- **`LIKE`**: Es un operador que se utiliza para buscar patrones en cadenas de texto.
- **`_A_T%S`**: Este es el patrón que estamos buscando en la columna `last_name`. Vamos a desglosarlo:
    - `_`: Representa **un solo carácter** (cualquier carácter).
    - `A`: Representa la letra `'A'`.
    - `_`: Representa **un solo carácter** (cualquier carácter).
    - `T`: Representa la letra `'T'`.
    - `%`: Representa **cualquier secuencia de caracteres** (incluyendo una secuencia vacía).
    - `S`: Representa la letra `'S'`.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Selecciona columnas específicas**: Obtenemos el apellido (`last_name`) y el nombre (`first_name`) de los clientes.
2. **Filtra por patrón en el apellido**: Solo se incluyen los clientes cuyo apellido cumple con el patrón `_A_T%S`. Es decir, el apellido debe:
    - Tener cualquier carácter en la primera posición.
    - Seguido de la letra `'A'`.
    - Seguido de cualquier carácter en la tercera posición.
    - Seguido de la letra `'T'`.
    - Seguido de cualquier secuencia de caracteres (o ninguno).
    - Terminar con la letra `'S'`.

---

### **Ejemplo Práctico**

Imagina que tienes la siguiente tabla `customer`:

| customer_id | first_name | last_name  |
|-------------|------------|------------|
| 1           | John       | Matthews   |
| 2           | Jane       | Walters    |
| 3           | Alice      | Bates      |
| 4           | Bob        | Cartwright |
| 5           | Carol      | Adams      |
| 6           | David      | Patters    |
| 7           | Eve        | Watts      |

---

#### Resultado de la Consulta
La consulta devolverá:

| last_name  | first_name |
|------------|------------|
| Matthews   | John       |
| Walters    | Jane       |
| Patters    | David      |

- **Explicación**:
    - **`Matthews`**: Cumple con el patrón `_A_T%S`:
        - `M` (cualquier carácter).
        - `A` (letra `'A'`).
        - `T` (cualquier carácter).
        - `T` (letra `'T'`).
        - `hews` (cualquier secuencia de caracteres).
        - Termina con `'S'`.
    - **`Walters`**: Cumple con el patrón `_A_T%S`:
        - `W` (cualquier carácter).
        - `A` (letra `'A'`).
        - `L` (cualquier carácter).
        - `T` (letra `'T'`).
        - `ers` (cualquier secuencia de caracteres).
        - Termina con `'S'`.
    - **`Patters`**: Cumple con el patrón `_A_T%S`:
        - `P` (cualquier carácter).
        - `A` (letra `'A'`).
        - `T` (cualquier carácter).
        - `T` (letra `'T'`).
        - `ers` (cualquier secuencia de caracteres).
        - Termina con `'S'`.

---

### **Resumen**

- **`SELECT`**: Selecciona el apellido y el nombre de los clientes.
- **`FROM`**: Especifica que estamos trabajando con la tabla `customer`.
- **`WHERE`**: Filtra los resultados para incluir solo los clientes cuyo apellido cumple con el patrón `_A_T%S`.

---

### **Notas Importantes**

1. **Patrón `LIKE`**:
    - `_`: Representa un solo carácter (cualquier carácter).
    - `%`: Representa cualquier secuencia de caracteres (incluyendo una secuencia vacía).
    - En este caso, el patrón `_A_T%S` busca apellidos que:
        - Tienen cualquier carácter en la primera posición.
        - Seguido de la letra `'A'`.
        - Seguido de cualquier carácter en la tercera posición.
        - Seguido de la letra `'T'`.
        - Seguido de cualquier secuencia de caracteres.
        - Terminan con la letra `'S'`.

2. **Sensibilidad a mayúsculas y minúsculas**:
    - La comparación `LIKE` es sensible a mayúsculas y minúsculas en la mayoría de las bases de datos. Si la base de datos distingue entre mayúsculas y minúsculas, solo coincidirán los apellidos que cumplan exactamente con el patrón.

---


### **Consulta SQL**

```sql
SELECT last_name, first_name
FROM customer
WHERE last_name LIKE 'Q%' OR last_name LIKE 'Y%';
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
WHERE last_name LIKE 'Q%' OR last_name LIKE 'Y%'
```
- **`LIKE 'Q%'`**: Filtra los apellidos que comienzan con la letra `'Q'`. El símbolo `%` es un comodín que significa "cualquier secuencia de caracteres".
- **`LIKE 'Y%'`**: Filtra los apellidos que comienzan con la letra `'Y'`. El símbolo `%` es un comodín que significa "cualquier secuencia de caracteres".
- **`OR`**: Indica que se deben incluir los clientes que cumplan **cualquiera** de las dos condiciones. Es decir, se incluirán los clientes cuyo apellido comience con `'Q'` **o** con `'Y'`.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Selecciona columnas específicas**: Obtenemos el apellido (`last_name`) y el nombre (`first_name`) de los clientes.
2. **Filtra por la primera letra del apellido**: Solo se incluyen los clientes cuyo apellido comienza con la letra `'Q'` o con la letra `'Y'`.

---

### **Ejemplo Práctico**

Imagina que tienes la siguiente tabla `customer`:

| customer_id | first_name | last_name  |
|-------------|------------|------------|
| 1           | John       | Quinn      |
| 2           | Jane       | Yates      |
| 3           | Alice      | Smith      |
| 4           | Bob        | Quintero   |
| 5           | Carol      | Yang       |
| 6           | David      | Johnson    |
| 7           | Eve        | Young      |

---

#### Resultado de la Consulta
La consulta devolverá:

| last_name  | first_name |
|------------|------------|
| Quinn      | John       |
| Yates      | Jane       |
| Quintero   | Bob        |
| Yang       | Carol      |
| Young      | Eve        |

- **Explicación**:
    - Los apellidos `'Quinn'`, `'Quintero'`, `'Yates'`, `'Yang'` y `'Young'` comienzan con `'Q'` o `'Y'`, por lo que se incluyen en los resultados.
    - Los apellidos `'Smith'` y `'Johnson'` no comienzan con `'Q'` ni con `'Y'`, por lo que no se incluyen.

---

### **Resumen**

- **`SELECT`**: Selecciona el apellido y el nombre de los clientes.
- **`FROM`**: Especifica que estamos trabajando con la tabla `customer`.
- **`WHERE`**: Filtra los resultados para incluir solo los clientes cuyo apellido comienza con `'Q'` o con `'Y'`.

---

### **Notas Importantes**

1. **Uso de `LIKE`**:
    - El operador `LIKE` se utiliza para buscar patrones en cadenas de texto.
    - El símbolo `%` es un comodín que significa "cualquier secuencia de caracteres".

2. **Sensibilidad a mayúsculas y minúsculas**:
    - La comparación `LIKE` es sensible a mayúsculas y minúsculas en la mayoría de las bases de datos. Si la base de datos distingue entre mayúsculas y minúsculas, solo coincidirán los apellidos que comiencen con `'Q'` o `'Y'` (mayúsculas).

3. **Alternativa con `LEFT`**:
    - Esta consulta también se puede escribir usando la función `LEFT`:

```sql
SELECT last_name, first_name
FROM customer
WHERE LEFT(last_name, 1) = 'Q' OR LEFT(last_name, 1) = 'Y';
```

Ambas consultas son equivalentes y devuelven los mismos resultados. La elección entre una y otra depende de la preferencia personal y la claridad del código.

---


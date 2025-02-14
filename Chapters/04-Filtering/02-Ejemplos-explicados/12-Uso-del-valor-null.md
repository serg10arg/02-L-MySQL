
### **Consulta SQL**

```sql
SELECT rental_id, customer_id
FROM rental
WHERE return_date IS NULL;
```

### **Explicación Paso a Paso**

---

#### 1. **`SELECT`**
```sql
SELECT rental_id, customer_id
```
- **`SELECT`**: Indica que queremos seleccionar datos de la base de datos.
- **`rental_id`**: Especificamos que queremos seleccionar la columna `rental_id`, que contiene el identificador único de cada alquiler.
- **`customer_id`**: También seleccionamos la columna `customer_id`, que contiene el identificador único del cliente que realizó el alquiler.

---

#### 2. **`FROM`**
```sql
FROM rental
```
- Especificamos que estamos trabajando con la tabla `rental`, que contiene información sobre los alquileres realizados por los clientes.

---

#### 3. **`WHERE`**
```sql
WHERE return_date IS NULL
```
- **`return_date`**: Es la columna que contiene la fecha en que se devolvió el artículo alquilado.
- **`IS NULL`**: Esta condición filtra las filas donde la columna `return_date` no tiene un valor asignado, es decir, donde la fecha de devolución es `NULL`. Esto significa que el artículo alquilado **no ha sido devuelto todavía**.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Selecciona columnas específicas**: Obtenemos el identificador del alquiler (`rental_id`) y el identificador del cliente (`customer_id`).
2. **Filtra por devolución pendiente**: Solo se incluyen los alquileres que no han sido devueltos (es decir, donde `return_date` es `NULL`).

---

### **Ejemplo Práctico**

Imagina que tienes la siguiente tabla `rental`:

| rental_id | customer_id | rental_date        | return_date        |
|-----------|-------------|--------------------|--------------------|
| 1         | 101         | 2023-10-01 10:00:00| 2023-10-05 15:00:00|
| 2         | 102         | 2023-10-02 12:00:00| NULL               |
| 3         | 103         | 2023-10-03 14:00:00| 2023-10-07 11:00:00|
| 4         | 104         | 2023-10-04 16:00:00| NULL               |
| 5         | 105         | 2023-10-05 18:00:00| 2023-10-10 09:00:00|

---

#### Resultado de la Consulta
La consulta devolverá:

| rental_id | customer_id |
|-----------|-------------|
| 2         | 102         |
| 4         | 104         |

- **Explicación**:
    - Los alquileres con `rental_id = 2` y `rental_id = 4` tienen `return_date` igual a `NULL`, lo que significa que no han sido devueltos todavía.
    - Los alquileres con `rental_id = 1`, `3` y `5` tienen una fecha de devolución (`return_date`), por lo que no se incluyen en los resultados.

---

### **Resumen**

- **`SELECT`**: Selecciona el identificador del alquiler y el identificador del cliente.
- **`FROM`**: Especifica que estamos trabajando con la tabla `rental`.
- **`WHERE`**: Filtra los resultados para incluir solo los alquileres que no han sido devueltos (`return_date IS NULL`).

---

### **Notas Importantes**

1. **Uso de `IS NULL`**:
    - La condición `IS NULL` se utiliza para verificar si una columna no tiene un valor asignado.
    - En este caso, se utiliza para identificar los alquileres que aún no han sido devueltos.

2. **Diferencia entre `NULL` y valores vacíos**:
    - `NULL` significa que no hay ningún valor asignado en la columna.
    - Un valor vacío (`''`) es diferente de `NULL` y no coincidirá con la condición `IS NULL`.

3. **Alternativa con `IS NOT NULL`**:
    - Si quisieras seleccionar los alquileres que **sí** han sido devueltos, podrías usar la condición `IS NOT NULL`:

```sql
SELECT rental_id, customer_id
FROM rental
WHERE return_date IS NOT NULL;
```

---

### Sentencia SQL:

```sql
SELECT rental_id, customer_id, return_date
FROM rental
WHERE return_date NOT BETWEEN '2005-05-01' AND '2005-09-01';
```

---

### **Paso 1: `SELECT rental_id, customer_id, return_date`**

- **`SELECT`**: Esta palabra clave se utiliza para especificar qué columnas queremos obtener de la tabla.
- **`rental_id, customer_id, return_date`**: Aquí, estamos seleccionando tres columnas específicas:
    - `rental_id`: Identificador único del alquiler.
    - `customer_id`: Identificador del cliente que realizó el alquiler.
    - `return_date`: Fecha en la que el alquiler fue devuelto.

### **Explicación:**

Esta parte de la sentencia nos dice que queremos obtener los valores de las columnas `rental_id`, `customer_id` y `return_date` de cada fila que cumpla con los criterios que definiremos más adelante.

---

### **Paso 2: `FROM rental`**

- **`FROM`**: Esta palabra clave se utiliza para especificar la tabla de la que queremos obtener los datos.
- **`rental`**: Este es el nombre de la tabla que contiene los datos de los alquileres.

### **Explicación:**

La parte `FROM rental` nos dice que los datos que queremos seleccionar provienen de la tabla llamada `rental`.

---

### **Paso 3: `WHERE return_date NOT BETWEEN '2005-05-01' AND '2005-09-01'`**

- **`WHERE`**: Esta palabra clave se utiliza para filtrar los datos según una condición específica. Solo se seleccionarán las filas que cumplan con esta condición.
- **`return_date NOT BETWEEN '2005-05-01' AND '2005-09-01'`**: Esta condición utiliza el operador `NOT BETWEEN` para filtrar las filas donde la fecha de devolución (`return_date`) no esté entre el 1 de mayo de 2005 y el 1 de septiembre de 2005.

### **Explicación:**

La parte `WHERE return_date NOT BETWEEN '2005-05-01' AND '2005-09-01'` nos dice que solo queremos las filas donde la fecha de devolución no esté dentro del rango especificado. En otras palabras, nos interesa obtener los alquileres que fueron devueltos antes del 1 de mayo de 2005 o después del 1 de septiembre de 2005.

---

### **Ejemplo Práctico:**

Supongamos que la tabla `rental` tiene los siguientes datos:

| rental_id | customer_id | return_date |
| --- | --- | --- |
| 1 | 101 | 2005-04-30 |
| 2 | 102 | 2005-06-15 |
| 3 | 103 | 2005-08-20 |
| 4 | 104 | 2005-09-05 |
| 5 | 105 | 2005-10-01 |

### **Ejecución de la consulta:**

La consulta `SELECT rental_id, customer_id, return_date FROM rental WHERE return_date NOT BETWEEN '2005-05-01' AND '2005-09-01';` filtrará solo los alquileres que fueron devueltos antes del 1 de mayo de 2005 o después del 1 de septiembre de 2005.

### **Resultado:**

| rental_id | customer_id | return_date |
| --- | --- | --- |
| 1 | 101 | 2005-04-30 |
| 4 | 104 | 2005-09-05 |
| 5 | 105 | 2005-10-01 |

---

### **Resumen:**

La sentencia SQL:

```sql
SELECT rental_id, customer_id, return_date
FROM rental
WHERE return_date NOT BETWEEN '2005-05-01' AND '2005-09-01';
```

realiza lo siguiente:

1. Selecciona las columnas `rental_id`, `customer_id` y `return_date` de la tabla `rental`.
2. Filtra las filas donde la fecha de devolución (`return_date`) no esté entre el 1 de mayo de 2005 y el 1 de septiembre de 2005.
3. Devuelve los resultados que cumplen con esta condición.

---

### **Notas Importantes:**

1. **Operador `NOT BETWEEN`:**
    - `NOT BETWEEN` es el opuesto de `BETWEEN`. Mientras `BETWEEN` selecciona valores dentro de un rango, `NOT BETWEEN` selecciona valores fuera de ese rango.
    - En este caso, `NOT BETWEEN '2005-05-01' AND '2005-09-01'` selecciona fechas antes del 1 de mayo de 2005 o después del 1 de septiembre de 2005.
2. **Formato de Fecha:**
    - En SQL, las fechas se escriben en formato `YYYY-MM-DD`. Asegúrate de que las fechas en tu consulta estén en este formato para evitar errores.
3. **Alternativa con `OR`:**
    - En lugar de `NOT BETWEEN`, también puedes usar el operador `OR` para lograr el mismo resultado:

        ```sql
        SELECT rental_id, customer_id, return_date
        FROM rental
        WHERE return_date < '2005-05-01' OR return_date > '2005-09-01';
        ```

    - Esta consulta es equivalente y puede ser más intuitiva para algunos.
4. **Indexación y Rendimiento:**
    - Si la columna `return_date` está indexada, la consulta puede ser más eficiente. Si no está indexada y la tabla es grande, puede ser útil crear un índice en la columna `return_date` para mejorar el rendimiento.

---

### **Conclusión:**

Esta sentencia SQL es útil para filtrar datos basados en rangos de fechas específicos. En este caso, nos ayuda a encontrar alquileres que fueron devueltos fuera de un rango de fechas determinado.

---

### Sentencia SQL:

```sql
SELECT rental_id, customer_id, return_date
FROM rental
WHERE return_date IS NULL
OR return_date NOT BETWEEN '2005-05-01' AND '2005-09-01';
```

---

### **Paso 1: `SELECT rental_id, customer_id, return_date`**

- **`SELECT`**: Esta palabra clave se utiliza para especificar qué columnas queremos obtener de la tabla.
- **`rental_id, customer_id, return_date`**: Estamos seleccionando tres columnas:
    - `rental_id`: Identificador único del alquiler.
    - `customer_id`: Identificador del cliente que realizó el alquiler.
    - `return_date`: Fecha en la que el alquiler fue devuelto.

### **Explicación:**

Esta parte de la sentencia nos dice que queremos obtener los valores de las columnas `rental_id`, `customer_id` y `return_date` de cada fila que cumpla con los criterios que definiremos más adelante.

---

### **Paso 2: `FROM rental`**

- **`FROM`**: Esta palabra clave se utiliza para especificar la tabla de la que queremos obtener los datos.
- **`rental`**: Este es el nombre de la tabla que contiene los datos de los alquileres.

### **Explicación:**

La parte `FROM rental` nos dice que los datos que queremos seleccionar provienen de la tabla llamada `rental`.

---

### **Paso 3: `WHERE return_date IS NULL OR return_date NOT BETWEEN '2005-05-01' AND '2005-09-01'`**

- **`WHERE`**: Esta palabra clave se utiliza para filtrar los datos según una condición específica. Solo se seleccionarán las filas que cumplan con esta condición.
- **`return_date IS NULL`**: Esta condición verifica si la fecha de devolución (`return_date`) es `NULL`. En SQL, `NULL` significa "valor desconocido" o "sin valor". En este contexto, significa que el alquiler aún no ha sido devuelto.
- **`OR`**: Este operador lógico combina dos condiciones. Una fila será seleccionada si cumple con cualquiera de las dos condiciones.
- **`return_date NOT BETWEEN '2005-05-01' AND '2005-09-01'`**: Esta condición verifica si la fecha de devolución no está entre el 1 de mayo de 2005 y el 1 de septiembre de 2005.

### **Explicación:**

La parte `WHERE return_date IS NULL OR return_date NOT BETWEEN '2005-05-01' AND '2005-09-01'` nos dice que queremos seleccionar dos tipos de alquileres:

1. Alquileres que aún no han sido devueltos (`return_date IS NULL`).
2. Alquileres que fueron devueltos antes del 1 de mayo de 2005 o después del 1 de septiembre de 2005 (`return_date NOT BETWEEN '2005-05-01' AND '2005-09-01'`).

---

### **Ejemplo Práctico:**

Supongamos que la tabla `rental` tiene los siguientes datos:

| rental_id | customer_id | return_date |
| --- | --- | --- |
| 1 | 101 | 2005-04-30 |
| 2 | 102 | 2005-06-15 |
| 3 | 103 | 2005-08-20 |
| 4 | 104 | NULL |
| 5 | 105 | 2005-09-05 |
| 6 | 106 | 2005-10-01 |

### **Ejecución de la consulta:**

La consulta seleccionará:

1. Alquileres que aún no han sido devueltos (`return_date IS NULL`).
2. Alquileres que fueron devueltos antes del 1 de mayo de 2005 o después del 1 de septiembre de 2005.

### **Resultado:**

| rental_id | customer_id | return_date |
| --- | --- | --- |
| 1 | 101 | 2005-04-30 |
| 4 | 104 | NULL |
| 5 | 105 | 2005-09-05 |
| 6 | 106 | 2005-10-01 |

---

### **Resumen:**

La sentencia SQL:

```sql
SELECT rental_id, customer_id, return_date
FROM rental
WHERE return_date IS NULL
OR return_date NOT BETWEEN '2005-05-01' AND '2005-09-01';
```

realiza lo siguiente:

1. Selecciona las columnas `rental_id`, `customer_id` y `return_date` de la tabla `rental`.
2. Filtra las filas donde:
    - La fecha de devolución es `NULL` (alquileres no devueltos).
    - La fecha de devolución no está entre el 1 de mayo de 2005 y el 1 de septiembre de 2005.
3. Devuelve los resultados que cumplen con cualquiera de las dos condiciones.

---

### **Notas Importantes:**

1. **Operador `OR`**:
    - El operador `OR` combina dos condiciones. Una fila será seleccionada si cumple con cualquiera de las dos condiciones. Si ambas condiciones son verdaderas, también se seleccionará la fila.
    - En este caso, la fila se selecciona si el alquiler no ha sido devuelto (`return_date IS NULL`) o si fue devuelto fuera del rango especificado.
2. **Formato de Fecha**:
    - Las fechas en SQL deben estar en formato `YYYY-MM-DD`. Asegúrate de que las fechas en tu consulta estén en este formato para evitar errores.
3. **Indexación y Rendimiento**:
    - Si la columna `return_date` está indexada, la consulta puede ser más eficiente. Si la tabla es grande y no está indexada, considera crear un índice en la columna `return_date` para mejorar el rendimiento.
4. **Alternativa con `IN`**:
    - En lugar de `NOT BETWEEN`, también puedes combinar condiciones con `IN` o `NOT IN` para lograr resultados similares. Por ejemplo:

        ```sql
        SELECT rental_id, customer_id, return_date
        FROM rental
        WHERE return_date IS NULL
        OR return_date < '2005-05-01'
        OR return_date > '2005-09-01';
        ```

5. **Consideraciones sobre `NULL`**:
    - `NULL` es un valor especial en SQL que representa un valor desconocido o ausente. No se puede comparar directamente con otros valores usando operadores como `=` o `<>`. En su lugar, se usa `IS NULL` o `IS NOT NULL`.

---

### **Conclusión:**

Esta sentencia SQL es útil para filtrar alquileres que no han sido devueltos o que fueron devueltos fuera de un rango de fechas específico. Combina dos condiciones con el operador `OR`, lo que la hace versátil para obtener resultados que cumplan con cualquiera de los criterios.
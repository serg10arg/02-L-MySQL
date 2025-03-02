
### **Consulta SQL**

```sql
SELECT customer_id, rental_date
FROM rental
WHERE rental_date <= '2005-06-16'
  AND rental_date >= '2005-06-14';
```

### **Explicación Paso a Paso**

---

#### 1. **`SELECT`**
```sql
SELECT customer_id, rental_date
```
- **`SELECT`**: Indica que queremos seleccionar datos de la base de datos.
- **`customer_id`**: Especificamos que queremos seleccionar la columna `customer_id`, que contiene el identificador único de cada cliente.
- **`rental_date`**: También seleccionamos la columna `rental_date`, que contiene la fecha y hora en que se realizó el alquiler.

---

#### 2. **`FROM`**
```sql
FROM rental
```
- Especificamos que estamos trabajando con la tabla `rental`, que contiene información sobre los alquileres realizados por los clientes.

---

#### 3. **`WHERE`**
```sql
WHERE rental_date <= '2005-06-16'
  AND rental_date >= '2005-06-14'
```
- **`rental_date <= '2005-06-16'`**: Filtramos los resultados para incluir solo los alquileres que ocurrieron **antes o en** la fecha `16 de junio de 2005`.
- **`rental_date >= '2005-06-14'`**: Filtramos los resultados para incluir solo los alquileres que ocurrieron **después o en** la fecha `14 de junio de 2005`.
- **`AND`**: Ambas condiciones deben ser verdaderas al mismo tiempo. Es decir, estamos buscando alquileres que ocurrieron entre el `14 de junio de 2005` y el `16 de junio de 2005`, inclusive.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Selecciona columnas específicas**: Obtenemos el identificador del cliente (`customer_id`) y la fecha del alquiler (`rental_date`).
2. **Filtra por rango de fechas**: Solo se incluyen los alquileres que ocurrieron entre el `14 de junio de 2005` y el `16 de junio de 2005`, inclusive.

---

### **Ejemplo Práctico**

Imagina que tienes la siguiente tabla `rental`:

| rental_id | customer_id | rental_date        |
|-----------|-------------|--------------------|
| 1         | 101         | 2005-06-14 10:30:00|
| 2         | 102         | 2005-06-15 15:45:00|
| 3         | 103         | 2005-06-16 09:00:00|
| 4         | 104         | 2005-06-13 12:00:00|
| 5         | 105         | 2005-06-17 18:30:00|

---

#### Resultado de la Consulta
La consulta devolverá:

| customer_id | rental_date        |
|-------------|--------------------|
| 101         | 2005-06-14 10:30:00|
| 102         | 2005-06-15 15:45:00|
| 103         | 2005-06-16 09:00:00|

- **Explicación**:
    - Los alquileres con `rental_id = 1`, `2` y `3` ocurrieron entre el `14 de junio de 2005` y el `16 de junio de 2005`, por lo que se incluyen en los resultados.
    - Los alquileres con `rental_id = 4` y `5` ocurrieron fuera de este rango de fechas, por lo que no se incluyen.

---

### **Resumen**

- **`SELECT`**: Selecciona el identificador del cliente y la fecha del alquiler.
- **`FROM`**: Especifica que estamos trabajando con la tabla `rental`.
- **`WHERE`**: Filtra los resultados para incluir solo los alquileres que ocurrieron entre el `14 de junio de 2005` y el `16 de junio de 2005`.

---

### **Consulta SQL**

```sql
SELECT customer_id, rental_date
FROM rental
WHERE rental_date BETWEEN '2005-06-14' AND '2005-06-16';
```

### **Explicación Paso a Paso**

---

#### 1. **`SELECT`**
```sql
SELECT customer_id, rental_date
```
- **`SELECT`**: Indica que queremos seleccionar datos de la base de datos.
- **`customer_id`**: Especificamos que queremos seleccionar la columna `customer_id`, que contiene el identificador único de cada cliente.
- **`rental_date`**: También seleccionamos la columna `rental_date`, que contiene la fecha y hora en que se realizó el alquiler.

---

#### 2. **`FROM`**
```sql
FROM rental
```
- Especificamos que estamos trabajando con la tabla `rental`, que contiene información sobre los alquileres realizados por los clientes.

---

#### 3. **`WHERE`**
```sql
WHERE rental_date BETWEEN '2005-06-14' AND '2005-06-16'
```
- **`BETWEEN '2005-06-14' AND '2005-06-16'`**: Esta cláusula filtra los resultados para incluir solo los alquileres que ocurrieron entre el `14 de junio de 2005` y el `16 de junio de 2005`, **inclusive**. Es decir, incluye las fechas límite (`14 de junio` y `16 de junio`).

---

### **¿Qué hace esta consulta en conjunto?**

1. **Selecciona columnas específicas**: Obtenemos el identificador del cliente (`customer_id`) y la fecha del alquiler (`rental_date`).
2. **Filtra por rango de fechas**: Solo se incluyen los alquileres que ocurrieron entre el `14 de junio de 2005` y el `16 de junio de 2005`, inclusive.

---

### **Ejemplo Práctico**

Imagina que tienes la siguiente tabla `rental`:

| rental_id | customer_id | rental_date        |
|-----------|-------------|--------------------|
| 1         | 101         | 2005-06-14 10:30:00|
| 2         | 102         | 2005-06-15 15:45:00|
| 3         | 103         | 2005-06-16 09:00:00|
| 4         | 104         | 2005-06-13 12:00:00|
| 5         | 105         | 2005-06-17 18:30:00|

---

#### Resultado de la Consulta
La consulta devolverá:

| customer_id | rental_date        |
|-------------|--------------------|
| 101         | 2005-06-14 10:30:00|
| 102         | 2005-06-15 15:45:00|
| 103         | 2005-06-16 09:00:00|

- **Explicación**:
    - Los alquileres con `rental_id = 1`, `2` y `3` ocurrieron entre el `14 de junio de 2005` y el `16 de junio de 2005`, por lo que se incluyen en los resultados.
    - Los alquileres con `rental_id = 4` y `5` ocurrieron fuera de este rango de fechas, por lo que no se incluyen.

---

### **Resumen**

- **`SELECT`**: Selecciona el identificador del cliente y la fecha del alquiler.
- **`FROM`**: Especifica que estamos trabajando con la tabla `rental`.
- **`WHERE`**: Filtra los resultados para incluir solo los alquileres que ocurrieron entre el `14 de junio de 2005` y el `16 de junio de 2005`.

---

### **Diferencia entre `BETWEEN` y `>=`/`<=`**

La consulta anterior es equivalente a:

```sql
SELECT customer_id, rental_date
FROM rental
WHERE rental_date >= '2005-06-14'
  AND rental_date <= '2005-06-16';
```

Ambas consultas devuelven los mismos resultados. La única diferencia es que `BETWEEN` es más conciso y fácil de leer cuando se trabaja con rangos de fechas o valores.

---


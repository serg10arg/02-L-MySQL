
### **Consulta SQL**

```sql
SELECT customer.first_name, customer.last_name,
       time(rental.rental_date) rental_time
FROM customer
  INNER JOIN rental
  ON customer.customer_id = rental.customer_id
WHERE date(rental.rental_date) = '2005-06-14';
```

### **Explicación Paso a Paso**

---

#### 1. **`SELECT`**
```sql
SELECT customer.first_name, customer.last_name,
       time(rental.rental_date) rental_time
```
- **`customer.first_name` y `customer.last_name`**: Seleccionamos las columnas `first_name` y `last_name` de la tabla `customer`. Estas columnas contienen el nombre y apellido de los clientes.
- **`time(rental.rental_date) rental_time`**: Aquí estamos seleccionando la hora (`time`) de la columna `rental_date` de la tabla `rental`. La función `time()` extrae solo la parte de la hora de una fecha y hora completa. Además, le damos un alias (`rental_time`) para que el resultado sea más legible.

---

#### 2. **`FROM`**
```sql
FROM customer
```
- Especificamos que estamos trabajando con la tabla `customer`, que contiene información sobre los clientes.

---

#### 3. **`INNER JOIN`**
```sql
INNER JOIN rental
ON customer.customer_id = rental.customer_id
```
- **`INNER JOIN rental`**: Combinamos la tabla `customer` con la tabla `rental`. Un `INNER JOIN` solo incluye las filas que tienen coincidencias en ambas tablas.
- **`ON customer.customer_id = rental.customer_id`**: Especificamos cómo se relacionan las tablas. En este caso, la columna `customer_id` en la tabla `customer` debe coincidir con la columna `customer_id` en la tabla `rental`. Esto nos permite obtener información sobre los alquileres (`rental`) realizados por cada cliente (`customer`).

---

#### 4. **`WHERE`**
```sql
WHERE date(rental.rental_date) = '2005-06-14'
```
- **`date(rental.rental_date)`**: La función `date()` extrae solo la parte de la fecha de la columna `rental_date` (que probablemente contiene una fecha y hora completa).
- **`= '2005-06-14'`**: Filtramos los resultados para incluir solo los alquileres que ocurrieron en la fecha `2005-06-14`.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Combina las tablas `customer` y `rental`**: Usando `INNER JOIN`, relacionamos a los clientes con sus alquileres basados en el `customer_id`.
2. **Filtra por fecha**: Solo se incluyen los alquileres que ocurrieron el `14 de junio de 2005`.
3. **Selecciona columnas específicas**: Obtenemos el nombre (`first_name`) y apellido (`last_name`) del cliente, junto con la hora (`rental_time`) en que se realizó el alquiler.

---

### **Ejemplo Práctico**

Imagina que tienes las siguientes tablas:

#### Tabla `customer`
| customer_id | first_name | last_name |
|-------------|------------|-----------|
| 1           | John       | Doe       |
| 2           | Jane       | Smith     |

#### Tabla `rental`
| rental_id | customer_id | rental_date        |
|-----------|-------------|--------------------|
| 101       | 1           | 2005-06-14 10:30:00|
| 102       | 2           | 2005-06-14 15:45:00|
| 103       | 1           | 2005-06-15 09:00:00|

---

#### Resultado de la Consulta
La consulta devolverá:

| first_name | last_name | rental_time |
|------------|-----------|-------------|
| John       | Doe       | 10:30:00    |
| Jane       | Smith     | 15:45:00    |

- **John Doe**: Alquiló algo a las `10:30:00` el `14 de junio de 2005`.
- **Jane Smith**: Alquiló algo a las `15:45:00` el mismo día.
- **Nota**: El alquiler con `rental_id = 103` no se incluye porque ocurrió el `15 de junio de 2005`, no el `14`.

---

### **Resumen**

- **`SELECT`**: Selecciona el nombre, apellido y la hora del alquiler.
- **`INNER JOIN`**: Combina las tablas `customer` y `rental` usando `customer_id`.
- **`WHERE`**: Filtra los resultados para incluir solo los alquileres del `14 de junio de 2005`.


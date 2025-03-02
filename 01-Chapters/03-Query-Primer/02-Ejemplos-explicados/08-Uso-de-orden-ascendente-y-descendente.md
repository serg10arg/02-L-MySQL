
### **Consulta SQL**

```sql
SELECT c.first_name, c.last_name,
       time(r.rental_date) rental_time
FROM customer c
  INNER JOIN rental r
  ON c.customer_id = r.customer_id
WHERE date(r.rental_date) = '2005-06-14'
ORDER BY c.last_name ASC, c.first_name DESC;
```

### **Explicación Paso a Paso**

---

#### 1. **`SELECT`**
```sql
SELECT c.first_name, c.last_name,
       time(r.rental_date) rental_time
```
- **`c.first_name` y `c.last_name`**: Seleccionamos las columnas `first_name` y `last_name` de la tabla `customer`. Estas columnas contienen el nombre y apellido de los clientes.
- **`time(r.rental_date) rental_time`**: Aquí estamos seleccionando la hora (`time`) de la columna `rental_date` de la tabla `rental`. La función `time()` extrae solo la parte de la hora de una fecha y hora completa. Además, le damos un alias (`rental_time`) para que el resultado sea más legible.

---

#### 2. **`FROM`**
```sql
FROM customer c
```
- Especificamos que estamos trabajando con la tabla `customer`, que contiene información sobre los clientes. Además, le damos un alias (`c`) para simplificar la consulta.

---

#### 3. **`INNER JOIN`**
```sql
INNER JOIN rental r
ON c.customer_id = r.customer_id
```
- **`INNER JOIN rental r`**: Combinamos la tabla `customer` con la tabla `rental`. Un `INNER JOIN` solo incluye las filas que tienen coincidencias en ambas tablas.
- **`ON c.customer_id = r.customer_id`**: Especificamos cómo se relacionan las tablas. En este caso, la columna `customer_id` en la tabla `customer` debe coincidir con la columna `customer_id` en la tabla `rental`. Esto nos permite obtener información sobre los alquileres (`rental`) realizados por cada cliente (`customer`).
- **Alias `r`**: Le damos un alias (`r`) a la tabla `rental` para simplificar la consulta.

---

#### 4. **`WHERE`**
```sql
WHERE date(r.rental_date) = '2005-06-14'
```
- **`date(r.rental_date)`**: La función `date()` extrae solo la parte de la fecha de la columna `rental_date` (que probablemente contiene una fecha y hora completa).
- **`= '2005-06-14'`**: Filtramos los resultados para incluir solo los alquileres que ocurrieron en la fecha `2005-06-14`.

---

#### 5. **`ORDER BY`**
```sql
ORDER BY c.last_name ASC, c.first_name DESC;
```
- **`ORDER BY`**: Ordena los resultados de la consulta.
- **`c.last_name ASC`**: Ordena los resultados por la columna `last_name` en orden ascendente (`ASC`), es decir, de la A a la Z.
- **`c.first_name DESC`**: Si hay clientes con el mismo apellido, los ordena por la columna `first_name` en orden descendente (`DESC`), es decir, de la Z a la A.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Combina las tablas `customer` y `rental`**: Usando `INNER JOIN`, relacionamos a los clientes con sus alquileres basados en el `customer_id`.
2. **Filtra por fecha**: Solo se incluyen los alquileres que ocurrieron el `14 de junio de 2005`.
3. **Selecciona columnas específicas**: Obtenemos el nombre (`first_name`), apellido (`last_name`) del cliente, junto con la hora (`rental_time`) en que se realizó el alquiler.
4. **Ordena los resultados**: Los resultados se ordenan primero por apellido en orden ascendente y luego por nombre en orden descendente.

---

### **Ejemplo Práctico**

Imagina que tienes las siguientes tablas:

#### Tabla `customer`
| customer_id | first_name | last_name |
|-------------|------------|-----------|
| 1           | John       | Doe       |
| 2           | Jane       | Smith     |
| 3           | Alice      | Johnson   |

#### Tabla `rental`
| rental_id | customer_id | rental_date        |
|-----------|-------------|--------------------|
| 101       | 1           | 2005-06-14 10:30:00|
| 102       | 2           | 2005-06-14 15:45:00|
| 103       | 3           | 2005-06-14 09:00:00|
| 104       | 1           | 2005-06-14 12:00:00|

---

#### Resultado de la Consulta
La consulta devolverá:

| first_name | last_name | rental_time |
|------------|-----------|-------------|
| John       | Doe       | 10:30:00    |
| John       | Doe       | 12:00:00    |
| Alice      | Johnson   | 09:00:00    |
| Jane       | Smith     | 15:45:00    |

- **Ordenación**:
    - Los resultados están ordenados por `last_name` en orden ascendente: `Doe`, `Johnson`, `Smith`.
    - Para los clientes con el mismo apellido (`John Doe`), los resultados están ordenados por `first_name` en orden descendente. Sin embargo, en este caso, solo hay un nombre (`John`), por lo que no hay cambios en el orden.

---

### **Resumen**

- **`SELECT`**: Selecciona el nombre, apellido y la hora del alquiler.
- **`INNER JOIN`**: Combina las tablas `customer` y `rental` usando `customer_id`.
- **`WHERE`**: Filtra los resultados para incluir solo los alquileres del `14 de junio de 2005`.
- **`ORDER BY`**: Ordena los resultados por apellido en orden ascendente y por nombre en orden descendente.


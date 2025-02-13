
### **Consulta SQL**

```sql
SELECT c.email
FROM customer c
  INNER JOIN rental r
  ON c.customer_id = r.customer_id
WHERE date(r.rental_date) = '2005-06-14';
```

### **Explicación Paso a Paso**

---

#### 1. **`SELECT`**
```sql
SELECT c.email
```
- **`SELECT`**: Indica que queremos seleccionar datos de la base de datos.
- **`c.email`**: Especificamos que queremos seleccionar la columna `email` de la tabla `customer`. Esta columna contiene las direcciones de correo electrónico de los clientes.

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
- **`= '2005-06-14'`**: Filtramos los resultados para incluir solo los alquileres que ocurrieron en la fecha `14 de junio de 2005`.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Combina las tablas `customer` y `rental`**: Usando `INNER JOIN`, relacionamos a los clientes con sus alquileres basados en el `customer_id`.
2. **Filtra por fecha**: Solo se incluyen los alquileres que ocurrieron el `14 de junio de 2005`.
3. **Selecciona el correo electrónico**: Obtenemos las direcciones de correo electrónico (`email`) de los clientes que realizaron alquileres en esa fecha.

---

### **Ejemplo Práctico**

Imagina que tienes las siguientes tablas:

#### Tabla `customer`
| customer_id | email               |
|-------------|---------------------|
| 1           | john.doe@example.com|
| 2           | jane.smith@example.com|
| 3           | alice.johnson@example.com|

#### Tabla `rental`
| rental_id | customer_id | rental_date        |
|-----------|-------------|--------------------|
| 101       | 1           | 2005-06-14 10:30:00|
| 102       | 2           | 2005-06-14 15:45:00|
| 103       | 3           | 2005-06-15 09:00:00|
| 104       | 1           | 2005-06-14 12:00:00|

---

#### Resultado de la Consulta
La consulta devolverá:

| email                     |
|---------------------------|
| john.doe@example.com      |
| jane.smith@example.com    |

- **Explicación**:
    - Los alquileres con `rental_id = 101`, `102` y `104` ocurrieron el `14 de junio de 2005`.
    - Los `customer_id` involucrados son `1` y `2`.
    - Por lo tanto, se seleccionan los correos electrónicos de los clientes con `customer_id = 1` y `customer_id = 2`.

---

### **Resumen**

- **`SELECT`**: Selecciona el correo electrónico de los clientes.
- **`INNER JOIN`**: Combina las tablas `customer` y `rental` usando `customer_id`.
- **`WHERE`**: Filtra los resultados para incluir solo los alquileres del `14 de junio de 2005`.


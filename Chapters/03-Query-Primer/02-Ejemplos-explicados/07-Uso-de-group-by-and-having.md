
### **Consulta SQL**

```sql
SELECT c.first_name, c.last_name, COUNT(*)
FROM customer c
  INNER JOIN rental r
  ON c.customer_id = r.customer_id
GROUP BY c.first_name, c.last_name
HAVING COUNT(*) >= 40;
```

### **Explicación Paso a Paso**

---

#### 1. **`SELECT`**
```sql
SELECT c.first_name, c.last_name, COUNT(*)
```
- **`c.first_name` y `c.last_name`**: Seleccionamos las columnas `first_name` y `last_name` de la tabla `customer`. Estas columnas contienen el nombre y apellido de los clientes.
- **`COUNT(*)`**: Esta es una función de agregación que cuenta el número de filas (en este caso, el número de alquileres) para cada grupo de clientes.

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

#### 4. **`GROUP BY`**
```sql
GROUP BY c.first_name, c.last_name
```
- **`GROUP BY`**: Agrupamos los resultados por el nombre (`first_name`) y apellido (`last_name`) del cliente. Esto significa que la consulta calculará el número de alquileres (`COUNT(*)`) para cada cliente individualmente.

---

#### 5. **`HAVING`**
```sql
HAVING COUNT(*) >= 40
```
- **`HAVING`**: Filtra los grupos (en este caso, los clientes) que cumplen con una condición específica. Aquí, estamos filtrando para incluir solo los clientes que tienen **40 o más alquileres**.
- **`COUNT(*) >= 40`**: Esta condición asegura que solo se incluyan los clientes con 40 o más alquileres.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Combina las tablas `customer` y `rental`**: Usando `INNER JOIN`, relacionamos a los clientes con sus alquileres basados en el `customer_id`.
2. **Agrupa por cliente**: Agrupamos los resultados por el nombre y apellido del cliente.
3. **Cuenta los alquileres por cliente**: Usamos `COUNT(*)` para contar cuántos alquileres ha realizado cada cliente.
4. **Filtra los clientes con 40 o más alquileres**: Usamos `HAVING` para incluir solo los clientes que han realizado 40 o más alquileres.

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
| rental_id | customer_id |
|-----------|-------------|
| 101       | 1           |
| 102       | 1           |
| 103       | 2           |
| 104       | 1           |
| 105       | 3           |
| ...       | ...         |
| 140       | 1           |

- **John Doe**: Ha realizado 40 alquileres.
- **Jane Smith**: Ha realizado 1 alquiler.
- **Alice Johnson**: Ha realizado 1 alquiler.

---

#### Resultado de la Consulta
La consulta devolverá:

| first_name | last_name | COUNT(*) |
|------------|-----------|----------|
| John       | Doe       | 40       |

- **John Doe**: Es el único cliente que ha realizado 40 o más alquileres.
- **Nota**: Jane y Alice no se incluyen porque no cumplen con la condición de tener 40 o más alquileres.

---

### **Resumen**

- **`SELECT`**: Selecciona el nombre, apellido y el número de alquileres de cada cliente.
- **`INNER JOIN`**: Combina las tablas `customer` y `rental` usando `customer_id`.
- **`GROUP BY`**: Agrupa los resultados por cliente.
- **`HAVING`**: Filtra los clientes que tienen 40 o más alquileres.


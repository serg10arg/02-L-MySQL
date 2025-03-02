
### **Consulta SQL**

```sql
SELECT c.first_name, c.last_name, addr.address, addr.city
FROM customer c
  INNER JOIN
    (SELECT a.address_id, a.address, ct.city
     FROM address a
       INNER JOIN city ct
       ON a.city_id = ct.city_id
     WHERE a.district = 'California'
    ) addr
  ON c.address_id = addr.address_id;
```

### **Explicación Paso a Paso**

---

#### 1. **Subconsulta**
```sql
(SELECT a.address_id, a.address, ct.city
 FROM address a
   INNER JOIN city ct
   ON a.city_id = ct.city_id
 WHERE a.district = 'California'
) addr
```
- **`SELECT a.address_id, a.address, ct.city`**: Selecciona las columnas `address_id`, `address` de la tabla `address` y `city` de la tabla `city`.
- **`FROM address a`**: Especifica que estamos trabajando con la tabla `address`, que contiene información sobre las direcciones. Le damos un alias (`a`) para simplificar la consulta.
- **`INNER JOIN city ct`**: Combinamos la tabla `address` con la tabla `city`. Un `INNER JOIN` solo incluye las filas que tienen coincidencias en ambas tablas.
- **`ON a.city_id = ct.city_id`**: Especificamos cómo se relacionan las tablas. En este caso, la columna `city_id` en la tabla `address` debe coincidir con la columna `city_id` en la tabla `city`.
- **`WHERE a.district = 'California'`**: Filtra los resultados para incluir solo las direcciones que están en el distrito de `'California'`.
- **Resultado de la subconsulta**: Esta subconsulta devuelve una lista de direcciones (`address_id`, `address`, `city`) que están en el distrito de `'California'`. Le damos un alias (`addr`) para usarlo en la consulta principal.

---

#### 2. **Consulta Principal**
```sql
SELECT c.first_name, c.last_name, addr.address, addr.city
FROM customer c
  INNER JOIN addr
  ON c.address_id = addr.address_id;
```
- **`SELECT c.first_name, c.last_name, addr.address, addr.city`**: Selecciona las columnas `first_name` y `last_name` de la tabla `customer`, y `address` y `city` de la subconsulta `addr`.
- **`FROM customer c`**: Especificamos que estamos trabajando con la tabla `customer`, que contiene información sobre los clientes. Le damos un alias (`c`) para simplificar la consulta.
- **`INNER JOIN addr`**: Combinamos la tabla `customer` con el resultado de la subconsulta `addr`. Un `INNER JOIN` solo incluye las filas que tienen coincidencias en ambas tablas.
- **`ON c.address_id = addr.address_id`**: Especificamos cómo se relacionan las tablas. En este caso, la columna `address_id` en la tabla `customer` debe coincidir con la columna `address_id` en la subconsulta `addr`.

---

### **¿Qué hace esta consulta en conjunto?**

1. **Subconsulta**: Encuentra las direcciones (`address_id`, `address`, `city`) que están en el distrito de `'California'`.
2. **Consulta Principal**: Selecciona el nombre (`first_name`), apellido (`last_name`), dirección (`address`) y ciudad (`city`) de los clientes que viven en una dirección que está en el distrito de `'California'`.

---

### **Ejemplo Práctico**

Imagina que tienes las siguientes tablas:

#### Tabla `customer`
| customer_id | first_name | last_name | address_id |
|-------------|------------|-----------|------------|
| 1           | John       | Doe       | 101        |
| 2           | Jane       | Smith     | 102        |
| 3           | Alice      | Johnson   | 103        |

#### Tabla `address`
| address_id | address               | city_id | district   |
|------------|-----------------------|---------|------------|
| 101        | 123 Main St           | 1       | California |
| 102        | 456 Elm St            | 2       | Texas      |
| 103        | 789 Oak St            | 3       | California |

#### Tabla `city`
| city_id | city          |
|---------|---------------|
| 1       | Los Angeles   |
| 2       | Houston       |
| 3       | San Francisco |

---

#### Resultado de la Subconsulta
La subconsulta:

```sql
SELECT a.address_id, a.address, ct.city
FROM address a
  INNER JOIN city ct
  ON a.city_id = ct.city_id
WHERE a.district = 'California'
```

Devolverá:

| address_id | address       | city          |
|------------|---------------|---------------|
| 101        | 123 Main St   | Los Angeles   |
| 103        | 789 Oak St    | San Francisco |

---

#### Resultado de la Consulta Principal
La consulta principal:

```sql
SELECT c.first_name, c.last_name, addr.address, addr.city
FROM customer c
  INNER JOIN addr
  ON c.address_id = addr.address_id;
```

Devolverá:

| first_name | last_name | address       | city          |
|------------|-----------|---------------|---------------|
| John       | Doe       | 123 Main St   | Los Angeles   |
| Alice      | Johnson   | 789 Oak St    | San Francisco |

- **Explicación**:
    - Los clientes `John Doe` y `Alice Johnson` viven en direcciones que están en el distrito de `'California'`, por lo que se incluyen en los resultados.
    - La cliente `Jane Smith` vive en una dirección que no está en `'California'`, por lo que no se incluye.

---

### **Resumen**

- **Subconsulta**: Encuentra las direcciones que están en el distrito de `'California'`.
- **Consulta Principal**: Selecciona el nombre, apellido, dirección y ciudad de los clientes que viven en esas direcciones.

---

### **Notas Importantes**

1. **Uso de subconsultas**:
    - Las subconsultas son útiles para simplificar consultas complejas y reutilizar resultados intermedios.
    - En este caso, la subconsulta filtra las direcciones en `'California'`, y la consulta principal usa ese resultado para encontrar los clientes correspondientes.

2. **Alias**:
    - Los alias (`c`, `a`, `ct`, `addr`) se utilizan para simplificar la consulta y hacerla más legible.

3. **`INNER JOIN`**:
    - El `INNER JOIN` asegura que solo se incluyan los clientes que tienen una dirección válida en el distrito de `'California'`.

---


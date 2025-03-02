
---

### **Manipulación de Datos Usando Subconsultas Correlacionadas**

Las subconsultas correlacionadas no solo son útiles en consultas `SELECT`, sino que también se utilizan ampliamente en declaraciones de manipulación de datos como `UPDATE`, `DELETE` e `INSERT`. Estas subconsultas permiten realizar operaciones complejas basadas en relaciones entre tablas.

---

### **1. Uso de Subconsultas Correlacionadas en `UPDATE`**

#### **Ejemplo: Actualizar la Columna `last_update`**

Supongamos que queremos actualizar la columna `last_update` en la tabla `customer` con la fecha de alquiler más reciente de cada cliente en la tabla `rental`. Podemos usar una subconsulta correlacionada para lograrlo:

```sql
UPDATE customer c
SET c.last_update = (
  SELECT MAX(r.rental_date)
  FROM rental r
  WHERE r.customer_id = c.customer_id
);
```

#### **Explicación:**
1. **Subconsulta Correlacionada**:
    - `SELECT MAX(r.rental_date) FROM rental r WHERE r.customer_id = c.customer_id`:
        - Devuelve la fecha de alquiler más reciente para cada cliente.
        - Hace referencia a `c.customer_id` de la consulta principal.

2. **Actualización**:
    - `SET c.last_update = (...)`:
        - Actualiza la columna `last_update` con la fecha de alquiler más reciente.

---

#### **Problema Potencial: Valores Nulos**

Si un cliente no tiene alquileres, la subconsulta devolverá `NULL`, lo que sobrescribirá la columna `last_update` con `NULL`. Para evitar esto, podemos agregar una cláusula `WHERE` con una segunda subconsulta correlacionada:

```sql
UPDATE customer c
SET c.last_update = (
  SELECT MAX(r.rental_date)
  FROM rental r
  WHERE r.customer_id = c.customer_id
)
WHERE EXISTS (
  SELECT 1
  FROM rental r
  WHERE r.customer_id = c.customer_id
);
```

#### **Explicación:**
1. **Subconsulta en `WHERE`**:
    - `SELECT 1 FROM rental r WHERE r.customer_id = c.customer_id`:
        - Verifica si el cliente tiene al menos un alquiler.
        - Si no hay alquileres, la fila no se actualiza.

2. **Protección contra `NULL`**:
    - Solo se actualizan los clientes que tienen al menos un alquiler.

---

### **2. Uso de Subconsultas Correlacionadas en `DELETE`**

#### **Ejemplo: Eliminar Clientes sin Alquileres Recientes**

Supongamos que queremos eliminar clientes que no han realizado alquileres en el último año. Podemos usar una subconsulta correlacionada para lograrlo:

```sql
DELETE FROM customer
WHERE 365 < ALL (
  SELECT DATEDIFF(NOW(), r.rental_date) days_since_last_rental
  FROM rental r
  WHERE r.customer_id = customer.customer_id
);
```

#### **Explicación:**
1. **Subconsulta Correlacionada**:
    - `SELECT DATEDIFF(NOW(), r.rental_date) FROM rental r WHERE r.customer_id = customer.customer_id`:
        - Calcula la cantidad de días desde el último alquiler para cada cliente.
        - Hace referencia a `customer.customer_id` de la consulta principal.

2. **Condición `ALL`**:
    - `WHERE 365 < ALL (...)`:
        - Filtra los clientes cuyo último alquiler fue hace más de 365 días.

---

#### **Nota sobre Alias en `DELETE` (MySQL)**

En MySQL, no se permiten alias de tabla en declaraciones `DELETE`. Por lo tanto, debes usar el nombre completo de la tabla en la subconsulta:

```sql
DELETE FROM customer
WHERE 365 < ALL (
  SELECT DATEDIFF(NOW(), r.rental_date)
  FROM rental r
  WHERE r.customer_id = customer.customer_id
);
```

En otros sistemas de bases de datos (como PostgreSQL o SQL Server), puedes usar alias:

```sql
DELETE FROM customer c
WHERE 365 < ALL (
  SELECT DATEDIFF(NOW(), r.rental_date)
  FROM rental r
  WHERE r.customer_id = c.customer_id
);
```

---

### **3. Uso de Subconsultas Correlacionadas en `INSERT`**

Aunque no se menciona en el texto, las subconsultas correlacionadas también pueden usarse en declaraciones `INSERT`. Por ejemplo, supongamos que queremos insertar en una tabla `customer_archive` a todos los clientes que no han realizado alquileres en el último año:

```sql
INSERT INTO customer_archive
SELECT *
FROM customer c
WHERE 365 < ALL (
  SELECT DATEDIFF(NOW(), r.rental_date)
  FROM rental r
  WHERE r.customer_id = c.customer_id
);
```

#### **Explicación:**
1. **Subconsulta Correlacionada**:
    - `SELECT DATEDIFF(NOW(), r.rental_date) FROM rental r WHERE r.customer_id = c.customer_id`:
        - Calcula la cantidad de días desde el último alquiler para cada cliente.

2. **Condición `ALL`**:
    - `WHERE 365 < ALL (...)`:
        - Filtra los clientes cuyo último alquiler fue hace más de 365 días.

3. **Inserción**:
    - `INSERT INTO customer_archive SELECT * FROM customer c WHERE ...`:
        - Inserta los clientes que cumplen la condición en la tabla `customer_archive`.

---

### **Resumen de Conceptos Clave**

1. **Subconsultas Correlacionadas en `UPDATE`**:
    - Útiles para actualizar columnas basadas en relaciones entre tablas.
    - Se pueden combinar con `EXISTS` para evitar valores `NULL`.

2. **Subconsultas Correlacionadas en `DELETE`**:
    - Útiles para eliminar filas basadas en condiciones complejas.
    - En MySQL, no se permiten alias de tabla en `DELETE`.

3. **Subconsultas Correlacionadas en `INSERT`**:
    - Útiles para insertar datos basados en condiciones de otras tablas.

4. **Precauciones**:
    - Asegúrate de que las subconsultas no devuelvan resultados inesperados (como `NULL`).
    - Considera el rendimiento, ya que las subconsultas correlacionadas se ejecutan una vez por cada fila.

---



---

### **Subconsultas Correlacionadas**

Una **subconsulta correlacionada** es aquella que depende de la consulta contenedora (la consulta principal). A diferencia de una subconsulta no correlacionada, que se ejecuta una sola vez antes de la consulta principal, una subconsulta correlacionada se ejecuta **una vez por cada fila** que la consulta principal procesa. Esto se debe a que la subconsulta correlacionada hace referencia a una o más columnas de la consulta principal.

---

### **Características de las Subconsultas Correlacionadas**

1. **Dependencia de la Consulta Principal**:
    - La subconsulta correlacionada hace referencia a columnas de la consulta principal.
    - Esto significa que no puede ejecutarse de manera independiente.

2. **Ejecución Repetida**:
    - La subconsulta se ejecuta una vez por cada fila candidata de la consulta principal.
    - Esto puede afectar el rendimiento si la consulta principal devuelve muchas filas.

3. **Uso en Condiciones**:
    - Pueden usarse en condiciones de igualdad, rango, y otras comparaciones.

---

### **Ejemplo 1: Contar Alquileres por Cliente**

Supongamos que queremos encontrar todos los clientes que han alquilado **exactamente 20 películas**. Podemos usar una subconsulta correlacionada para lograrlo:

```sql
SELECT c.first_name, c.last_name
FROM customer c
WHERE 20 = (
  SELECT COUNT(*)
  FROM rental r
  WHERE r.customer_id = c.customer_id
);
```

#### **Resultado:**
```sql
+------------+-------------+
| first_name | last_name   |
+------------+-------------+
| LAUREN     | HUDSON      |
| JEANETTE   | GREENE      |
| TARA       | RYAN        |
| WILMA      | RICHARDS    |
| JO         | FOWLER      |
| KAY        | CALDWELL    |
| DANIEL     | CABRAL      |
| ANTHONY    | SCHWAB      |
| TERRY      | GRISSOM     |
| LUIS       | YANEZ       |
| HERBERT    | KRUGER      |
| OSCAR      | AQUINO      |
| RAUL       | FORTIER     |
| NELSON     | CHRISTENSON |
| ALFREDO    | MCADAMS     |
+------------+-------------+
15 rows in set (0.01 sec)
```

#### **Explicación:**
1. **Consulta Principal**:
    - Selecciona las columnas `first_name` y `last_name` de la tabla `customer`.

2. **Subconsulta Correlacionada**:
    - `SELECT COUNT(*) FROM rental r WHERE r.customer_id = c.customer_id`:
        - Cuenta el número de alquileres para cada cliente.
        - Hace referencia a `c.customer_id` de la consulta principal.

3. **Condición**:
    - `WHERE 20 = (...)`:
        - Filtra los clientes que tienen exactamente 20 alquileres.

---

### **Ejemplo 2: Filtrar por Rango de Pagos**

Supongamos que queremos encontrar todos los clientes cuyos pagos totales por alquiler de películas están entre **$180 y $240**. Podemos usar una subconsulta correlacionada para lograrlo:

```sql
SELECT c.first_name, c.last_name
FROM customer c
WHERE (
  SELECT SUM(p.amount)
  FROM payment p
  WHERE p.customer_id = c.customer_id
) BETWEEN 180 AND 240;
```

#### **Resultado:**
```sql
+------------+-----------+
| first_name | last_name |
+------------+-----------+
| RHONDA     | KENNEDY   |
| CLARA      | SHAW      |
| ELEANOR    | HUNT      |
| MARION     | SNYDER    |
| TOMMY      | COLLAZO   |
| KARL       | SEAL      |
+------------+-----------+
6 rows in set (0.03 sec)
```

#### **Explicación:**
1. **Consulta Principal**:
    - Selecciona las columnas `first_name` y `last_name` de la tabla `customer`.

2. **Subconsulta Correlacionada**:
    - `SELECT SUM(p.amount) FROM payment p WHERE p.customer_id = c.customer_id`:
        - Calcula el total de pagos para cada cliente.
        - Hace referencia a `c.customer_id` de la consulta principal.

3. **Condición**:
    - `WHERE (...) BETWEEN 180 AND 240`:
        - Filtra los clientes cuyo total de pagos está entre $180 y $240.

---

### **Precauciones con Subconsultas Correlacionadas**

1. **Rendimiento**:
    - Las subconsultas correlacionadas se ejecutan una vez por cada fila de la consulta principal.
    - Si la consulta principal devuelve muchas filas, esto puede afectar el rendimiento.

2. **Alternativas**:
    - En algunos casos, es posible reescribir la consulta usando `JOIN` o subconsultas no correlacionadas para mejorar el rendimiento.

---

### **Resumen de Conceptos Clave**

1. **Subconsultas Correlacionadas**:
    - Dependen de la consulta principal.
    - Se ejecutan una vez por cada fila de la consulta principal.

2. **Uso Común**:
    - Para filtrar datos basados en cálculos o agregaciones específicas para cada fila.

3. **Precauciones**:
    - Pueden afectar el rendimiento si la consulta principal devuelve muchas filas.
    - Considera alternativas como `JOIN` o subconsultas no correlacionadas cuando sea posible.

---

### **Ejemplo Adicional**

Supongamos que queremos encontrar todos los clientes que han realizado al menos un pago mayor a $100:

```sql
SELECT c.first_name, c.last_name
FROM customer c
WHERE EXISTS (
  SELECT 1
  FROM payment p
  WHERE p.customer_id = c.customer_id
    AND p.amount > 100
);
```

#### **Explicación:**
1. **Subconsulta Correlacionada**:
    - `SELECT 1 FROM payment p WHERE p.customer_id = c.customer_id AND p.amount > 100`:
        - Verifica si el cliente ha realizado al menos un pago mayor a $100.

2. **Condición**:
    - `WHERE EXISTS (...)`:
        - Filtra los clientes que cumplen la condición de la subconsulta.

---


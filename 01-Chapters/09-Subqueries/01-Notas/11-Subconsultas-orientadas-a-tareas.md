
---

### **Subconsultas Orientadas a Tareas**

En SQL, las subconsultas pueden usarse para dividir una tarea compleja en pasos más manejables. Esto no solo mejora la legibilidad del código, sino que también puede optimizar el rendimiento de la consulta. En este ejemplo, veremos cómo usar subconsultas para generar un informe que muestre el nombre de cada cliente, su ciudad, el número total de alquileres y el monto total de pagos.

---

### **Versión Inicial: Consulta Completa sin Subconsultas**

Primero, veamos la versión inicial de la consulta que une todas las tablas necesarias (`payment`, `customer`, `address`, `city`) y agrupa los datos:

```sql
SELECT c.first_name, c.last_name, ct.city,
       SUM(p.amount) tot_payments, COUNT(*) tot_rentals
FROM payment p
  INNER JOIN customer c
  ON p.customer_id = c.customer_id
  INNER JOIN address a
  ON c.address_id = a.address_id
  INNER JOIN city ct
  ON a.city_id = ct.city_id
GROUP BY c.first_name, c.last_name, ct.city;
```

#### **Resultado:**
```sql
+-------------+------------+-----------------+--------------+-------------+
| first_name  | last_name  | city            | tot_payments | tot_rentals |
+-------------+------------+-----------------+--------------+-------------+
| MARY        | SMITH      | Sasebo          |       118.68 |          32 |
| PATRICIA    | JOHNSON    | San Bernardino  |       128.73 |          27 |
| LINDA       | WILLIAMS   | Athenai         |       135.74 |          26 |
| BARBARA     | JONES      | Myingyan        |        81.78 |          22 |
...
| TERRENCE    | GUNDERSON  | Jinzhou         |       117.70 |          30 |
| ENRIQUE     | FORSYTHE   | Patras          |        96.72 |          28 |
| FREDDIE     | DUGGAN     | Sullana         |        99.75 |          25 |
| WADE        | DELVALLE   | Lausanne        |        83.78 |          22 |
| AUSTIN      | CINTRON    | Tieli           |        83.81 |          19 |
+-------------+------------+-----------------+--------------+-------------+
599 rows in set (0.06 sec)
```

#### **Problemas de esta Versión:**
1. **Complejidad**:
    - La consulta une múltiples tablas y agrupa por varias columnas de texto (`first_name`, `last_name`, `city`), lo que puede ser difícil de leer y mantener.

2. **Rendimiento**:
    - Agrupar por columnas de texto puede ser menos eficiente que agrupar por una columna numérica como `customer_id`.

---

### **Versión Mejorada: Usando Subconsultas**

Para mejorar la consulta, podemos dividirla en dos partes:
1. **Subconsulta**: Agrupar los datos de pagos y alquileres por `customer_id`.
2. **Consulta Principal**: Unir los resultados de la subconsulta con las tablas `customer`, `address` y `city` para obtener los nombres y la ciudad.

#### **Paso 1: Subconsulta de Agrupación**

Primero, creamos una subconsulta que agrupa los datos de la tabla `payment` por `customer_id`:

```sql
SELECT customer_id,
       COUNT(*) tot_rentals, 
       SUM(amount) tot_payments
FROM payment
GROUP BY customer_id;
```

#### **Resultado de la Subconsulta:**
```sql
+-------------+-------------+--------------+
| customer_id | tot_rentals | tot_payments |
+-------------+-------------+--------------+
|           1 |          32 |       118.68 |
|           2 |          27 |       128.73 |
|           3 |          26 |       135.74 |
|           4 |          22 |        81.78 |
...
|         599 |          19 |        83.81 |
+-------------+-------------+--------------+
599 rows in set (0.03 sec)
```

#### **Paso 2: Unir la Subconsulta con Otras Tablas**

Ahora, usamos esta subconsulta en la consulta principal para unirla con las tablas `customer`, `address` y `city`:

```sql
SELECT c.first_name, c.last_name,
       ct.city,
       pymnt.tot_payments, pymnt.tot_rentals
FROM (
  SELECT customer_id,
         COUNT(*) tot_rentals, 
         SUM(amount) tot_payments
  FROM payment
  GROUP BY customer_id
) pymnt
  INNER JOIN customer c
  ON pymnt.customer_id = c.customer_id
  INNER JOIN address a
  ON c.address_id = a.address_id
  INNER JOIN city ct
  ON a.city_id = ct.city_id;
```

#### **Resultado:**
```sql
+-------------+------------+-----------------+--------------+-------------+
| first_name  | last_name  | city            | tot_payments | tot_rentals |
+-------------+------------+-----------------+--------------+-------------+
| MARY        | SMITH      | Sasebo          |       118.68 |          32 |
| PATRICIA    | JOHNSON    | San Bernardino  |       128.73 |          27 |
| LINDA       | WILLIAMS   | Athenai         |       135.74 |          26 |
| BARBARA     | JONES      | Myingyan        |        81.78 |          22 |
...
| TERRENCE    | GUNDERSON  | Jinzhou         |       117.70 |          30 |
| ENRIQUE     | FORSYTHE   | Patras          |        96.72 |          28 |
| FREDDIE     | DUGGAN     | Sullana         |        99.75 |          25 |
| WADE        | DELVALLE   | Lausanne        |        83.78 |          22 |
| AUSTIN      | CINTRON    | Tieli           |        83.81 |          19 |
+-------------+------------+-----------------+--------------+-------------+
599 rows in set (0.06 sec)
```

---

### **Ventajas de la Versión con Subconsultas**

1. **Legibilidad**:
    - La consulta está dividida en partes más pequeñas y manejables.
    - Es más fácil entender y mantener.

2. **Rendimiento**:
    - La agrupación se realiza en una sola columna numérica (`customer_id`), lo que es más eficiente que agrupar por múltiples columnas de texto.

3. **Reutilización**:
    - La subconsulta puede reutilizarse en otras consultas si es necesario.

---

### **Resumen de Conceptos Clave**

1. **Subconsultas Orientadas a Tareas**:
    - Permiten dividir consultas complejas en pasos más simples.
    - Mejoran la legibilidad y el rendimiento.

2. **Agrupación Eficiente**:
    - Agrupar por una columna numérica (`customer_id`) es más eficiente que agrupar por múltiples columnas de texto.

3. **Unión con Otras Tablas**:
    - Las subconsultas pueden unirse con otras tablas para obtener datos adicionales (como nombres y ciudades).

---

### **Ejemplo Adicional**

Supongamos que queremos agregar la columna `country` al informe. Podemos extender la consulta para incluir la tabla `country`:

```sql
SELECT c.first_name, c.last_name,
       ct.city, co.country,
       pymnt.tot_payments, pymnt.tot_rentals
FROM (
  SELECT customer_id,
         COUNT(*) tot_rentals, 
         SUM(amount) tot_payments
  FROM payment
  GROUP BY customer_id
) pymnt
  INNER JOIN customer c
  ON pymnt.customer_id = c.customer_id
  INNER JOIN address a
  ON c.address_id = a.address_id
  INNER JOIN city ct
  ON a.city_id = ct.city_id
  INNER JOIN country co
  ON ct.country_id = co.country_id;
```

#### **Explicación:**
1. **Subconsulta**:
    - Agrupa los datos de pagos y alquileres por `customer_id`.

2. **Consulta Principal**:
    - Une la subconsulta con las tablas `customer`, `address`, `city` y `country` para obtener nombres, ciudad y país.

---


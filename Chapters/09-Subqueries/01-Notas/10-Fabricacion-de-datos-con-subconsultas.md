
---

### **Fabricación de Datos con Subconsultas**

En SQL, no siempre necesitas depender de los datos almacenados en las tablas de tu base de datos. Puedes **fabricar datos** directamente en una consulta utilizando subconsultas. Esto es especialmente útil cuando necesitas definir categorías, rangos o grupos que no están predefinidos en tu base de datos.

---

### **Ejemplo: Clasificación de Clientes por Monto de Pagos**

Supongamos que queremos clasificar a los clientes en tres grupos según el monto total que han gastado en alquileres de películas:

| Nombre del grupo | Límite inferior | Límite superior |
|------------------|-----------------|-----------------|
| Pequeños         | 0               | $74.99          |
| Promedio         | $75             | $149.99         |
| Grandes Gastos   | $150            | $9,999,999.99   |

Para lograr esto, podemos fabricar los grupos directamente en la consulta usando subconsultas.

---

### **Paso 1: Crear los Grupos con `UNION ALL`**

Primero, definimos los grupos utilizando una subconsulta con `UNION ALL`:

```sql
SELECT 'Small Fry' name, 0 low_limit, 74.99 high_limit
UNION ALL
SELECT 'Average Joes' name, 75 low_limit, 149.99 high_limit
UNION ALL
SELECT 'Heavy Hitters' name, 150 low_limit, 9999999.99 high_limit;
```

#### **Resultado:**
```sql
+---------------+-----------+------------+
| name          | low_limit | high_limit |
+---------------+-----------+------------+
| Small Fry     |         0 |      74.99 |
| Average Joes  |        75 |     149.99 |
| Heavy Hitters |       150 | 9999999.99 |
+---------------+-----------+------------+
3 rows in set (0.00 sec)
```

#### **Explicación:**
- Cada `SELECT` define un grupo con un nombre, un límite inferior y un límite superior.
- `UNION ALL` combina los resultados de las tres consultas en un solo conjunto de resultados.

---

### **Paso 2: Unir los Grupos con los Datos de Pagos**

Ahora, usamos esta subconsulta en la cláusula `FROM` de otra consulta para clasificar a los clientes según sus pagos totales:

```sql
SELECT pymnt_grps.name, COUNT(*) num_customers
FROM (
  SELECT customer_id,
         COUNT(*) num_rentals, 
         SUM(amount) tot_payments
  FROM payment
  GROUP BY customer_id
) pymnt
INNER JOIN (
  SELECT 'Small Fry' name, 0 low_limit, 74.99 high_limit
  UNION ALL
  SELECT 'Average Joes' name, 75 low_limit, 149.99 high_limit
  UNION ALL
  SELECT 'Heavy Hitters' name, 150 low_limit, 9999999.99 high_limit
) pymnt_grps
ON pymnt.tot_payments BETWEEN pymnt_grps.low_limit AND pymnt_grps.high_limit
GROUP BY pymnt_grps.name;
```

#### **Resultado:**
```sql
+---------------+---------------+
| name          | num_customers |
+---------------+---------------+
| Average Joes  |           515 |
| Heavy Hitters |            46 |
| Small Fry     |            38 |
+---------------+---------------+
3 rows in set (0.03 sec)
```

---

### **Explicación Paso a Paso**

1. **Subconsulta `pymnt`**:
    - Agrupa los datos de la tabla `payment` por `customer_id` y calcula:
        - `COUNT(*) num_rentals`: El número de alquileres por cliente.
        - `SUM(amount) tot_payments`: El total de pagos por cliente.

   ```sql
   SELECT customer_id,
          COUNT(*) num_rentals, 
          SUM(amount) tot_payments
   FROM payment
   GROUP BY customer_id;
   ```

2. **Subconsulta `pymnt_grps`**:
    - Define los tres grupos de clientes utilizando `UNION ALL`.

   ```sql
   SELECT 'Small Fry' name, 0 low_limit, 74.99 high_limit
   UNION ALL
   SELECT 'Average Joes' name, 75 low_limit, 149.99 high_limit
   UNION ALL
   SELECT 'Heavy Hitters' name, 150 low_limit, 9999999.99 high_limit;
   ```

3. **Unión de las Subconsultas**:
    - La subconsulta `pymnt` se une con `pymnt_grps` usando la condición `BETWEEN`.
    - Esto clasifica a cada cliente en uno de los tres grupos según su total de pagos.

   ```sql
   ON pymnt.tot_payments BETWEEN pymnt_grps.low_limit AND pymnt_grps.high_limit;
   ```

4. **Agrupación y Conteo**:
    - Los resultados se agrupan por el nombre del grupo (`pymnt_grps.name`).
    - Se cuenta el número de clientes en cada grupo (`COUNT(*) num_customers`).

---

### **Ventajas de Fabricar Datos con Subconsultas**

1. **Flexibilidad**:
    - Permite definir categorías o rangos dinámicamente sin necesidad de crear tablas adicionales.

2. **Mantenimiento Simplificado**:
    - Evita la necesidad de llenar la base de datos con tablas pequeñas y específicas que podrían volverse obsoletas.

3. **Legibilidad**:
    - Mantiene la lógica de la consulta en un solo lugar, lo que facilita su comprensión y mantenimiento.

---

### **Consideraciones Importantes**

1. **Rendimiento**:
    - Fabricar datos en una consulta puede afectar el rendimiento si se generan grandes volúmenes de datos.
    - Asegúrate de optimizar las subconsultas y usar índices adecuados.

2. **Reutilización**:
    - Si los grupos o categorías se usan frecuentemente, considera almacenarlos en una tabla para evitar repetir la lógica en múltiples consultas.

---

### **Ejemplo Adicional**

Supongamos que queremos clasificar a los clientes en grupos según la cantidad de alquileres:

| Nombre del grupo | Límite inferior | Límite superior |
|------------------|-----------------|-----------------|
| Ocasionales      | 0               | 10              |
| Frecuentes       | 11              | 20              |
| Adictos          | 21              | 999             |

Podemos usar una subconsulta similar para lograrlo:

```sql
SELECT rental_grps.name, COUNT(*) num_customers
FROM (
  SELECT customer_id,
         COUNT(*) num_rentals
  FROM rental
  GROUP BY customer_id
) rentals
INNER JOIN (
  SELECT 'Ocasionales' name, 0 low_limit, 10 high_limit
  UNION ALL
  SELECT 'Frecuentes' name, 11 low_limit, 20 high_limit
  UNION ALL
  SELECT 'Adictos' name, 21 low_limit, 999 high_limit
) rental_grps
ON rentals.num_rentals BETWEEN rental_grps.low_limit AND rental_grps.high_limit
GROUP BY rental_grps.name;
```

#### **Explicación:**
1. **Subconsulta `rentals`**:
    - Calcula el número de alquileres por cliente.

2. **Subconsulta `rental_grps`**:
    - Define los grupos según la cantidad de alquileres.

3. **Unión y Agrupación**:
    - Clasifica a los clientes en grupos y cuenta cuántos hay en cada uno.

---

### **Resumen de Conceptos Clave**

1. **Fabricación de Datos**:
    - Permite definir categorías o rangos directamente en una consulta.
    - Útil para clasificar datos sin necesidad de tablas adicionales.

2. **Uso de `UNION ALL`**:
    - Combina múltiples conjuntos de resultados en uno solo.
    - Ideal para definir grupos o rangos.

3. **Ventajas**:
    - Flexibilidad, mantenimiento simplificado y legibilidad.

4. **Consideraciones**:
    - Rendimiento y reutilización.

---


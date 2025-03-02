
---

### **Grupos Implícitos vs. Explícitos**

#### **1. Grupos Implícitos**
En el ejemplo anterior, se utilizaron funciones de agregación (`MAX`, `MIN`, `AVG`, `SUM`, `COUNT`) sin una cláusula `GROUP BY`. Esto significa que todas las filas de la tabla `payment` se trataron como un **único grupo implícito**. Las funciones de agregación se aplicaron a **todas las filas** de la tabla, generando un solo resultado para cada función.

Por ejemplo:
```sql
SELECT MAX(amount) max_amt,
       MIN(amount) min_amt,
       AVG(amount) avg_amt,
       SUM(amount) tot_amt,
       COUNT(*) num_payments
FROM payment;
```
- Aquí, `MAX(amount)` devuelve el valor máximo de **todos los pagos**.
- `MIN(amount)` devuelve el valor mínimo de **todos los pagos**.
- `AVG(amount)` devuelve el promedio de **todos los pagos**.
- `SUM(amount)` devuelve la suma de **todos los pagos**.
- `COUNT(*)` devuelve el número total de **todos los pagos**.

---

#### **2. Grupos Explícitos**
En la mayoría de los casos, no solo querrás calcular estas métricas para toda la tabla, sino también para **grupos específicos** de datos. Por ejemplo, podrías querer calcular estas métricas **para cada cliente** en lugar de para todos los clientes juntos.

Para hacer esto, necesitas usar la cláusula `GROUP BY`. Esta cláusula divide los datos en grupos basados en una columna específica (en este caso, `customer_id`) y luego aplica las funciones de agregación a **cada grupo por separado**.

---

#### **Ejemplo sin `GROUP BY` (Error común)**
Si intentas ejecutar la siguiente consulta:
```sql
SELECT customer_id,
       MAX(amount) max_amt,
       MIN(amount) min_amt,
       AVG(amount) avg_amt,
       SUM(amount) tot_amt,
       COUNT(*) num_payments
FROM payment;
```
Obtendrás un error:
```
ERROR 1140 (42000): In aggregated query without GROUP BY, 
expression #1 of SELECT list contains nonaggregated column
```
- **¿Por qué ocurre este error?**
    - La consulta intenta devolver `customer_id` junto con funciones de agregación, pero no especifica cómo agrupar los datos.
    - SQL no sabe si `customer_id` debe ser único o si debe agruparse.

---

#### **Solución: Usar `GROUP BY`**
Para corregir este error, debes agregar una cláusula `GROUP BY` que especifique cómo se deben agrupar los datos. En este caso, agrupamos por `customer_id`:

```sql
SELECT customer_id,
       MAX(amount) max_amt,
       MIN(amount) min_amt,
       AVG(amount) avg_amt,
       SUM(amount) tot_amt,
       COUNT(*) num_payments
FROM payment
GROUP BY customer_id;
```

---

#### **¿Qué hace esta consulta?**
1. **Agrupa los datos**: Las filas de la tabla `payment` se dividen en grupos, donde cada grupo contiene todas las filas con el mismo `customer_id`.
2. **Aplica funciones de agregación**: Para cada grupo (es decir, para cada cliente), se calculan las siguientes métricas:
    - `MAX(amount)`: El pago máximo realizado por ese cliente.
    - `MIN(amount)`: El pago mínimo realizado por ese cliente.
    - `AVG(amount)`: El promedio de los pagos realizados por ese cliente.
    - `SUM(amount)`: La suma total de los pagos realizados por ese cliente.
    - `COUNT(*)`: El número total de pagos realizados por ese cliente.

---

#### **Resultado de la consulta**
La consulta devuelve una fila por cada cliente (`customer_id`), con las métricas calculadas para ese cliente. Por ejemplo:

| customer_id | max_amt | min_amt | avg_amt  | tot_amt | num_payments |
|-------------|---------|---------|----------|---------|--------------|
| 1           | 9.99    | 0.99    | 3.708750 | 118.68  | 32           |
| 2           | 10.99   | 0.99    | 4.767778 | 128.73  | 27           |
| 3           | 10.99   | 0.99    | 5.220769 | 135.74  | 26           |
| ...         | ...     | ...     | ...      | ...     | ...          |
| 599         | 9.99    | 0.99    | 4.411053 | 83.81   | 19           |

- **`customer_id`**: El identificador del cliente.
- **`max_amt`**: El pago máximo realizado por ese cliente.
- **`min_amt`**: El pago mínimo realizado por ese cliente.
- **`avg_amt`**: El promedio de los pagos realizados por ese cliente.
- **`tot_amt`**: La suma total de los pagos realizados por ese cliente.
- **`num_payments`**: El número total de pagos realizados por ese cliente.

---

### **Resumen de conceptos clave**

1. **Grupos Implícitos**:
    - Ocurren cuando no hay una cláusula `GROUP BY`.
    - Las funciones de agregación se aplican a **todas las filas** de la tabla.
    - Ejemplo: Calcular métricas para toda la tabla `payment`.

2. **Grupos Explícitos**:
    - Ocurren cuando se usa una cláusula `GROUP BY`.
    - Las funciones de agregación se aplican a **cada grupo** por separado.
    - Ejemplo: Calcular métricas para cada cliente en la tabla `payment`.

3. **`GROUP BY`**:
    - Especifica cómo se deben agrupar los datos.
    - Permite calcular métricas para cada grupo (por ejemplo, por cliente, por categoría, etc.).

4. **Funciones de agregación**:
    - `MAX`, `MIN`, `AVG`, `SUM`, `COUNT`, etc.
    - Se aplican a cada grupo cuando se usa `GROUP BY`.

---

### **¿Cuándo usar `GROUP BY`?**
- Cuando necesitas calcular métricas para grupos específicos de datos (por ejemplo, por cliente, por categoría, por región, etc.).
- Cuando incluyes columnas no agregadas (como `customer_id`) junto con funciones de agregación.

---

### **Ejemplo adicional**
Si quisieras calcular las mismas métricas pero agrupadas por `staff_id` (el empleado que procesó el pago), usarías:

```sql
SELECT staff_id,
       MAX(amount) max_amt,
       MIN(amount) min_amt,
       AVG(amount) avg_amt,
       SUM(amount) tot_amt,
       COUNT(*) num_payments
FROM payment
GROUP BY staff_id;
```

---


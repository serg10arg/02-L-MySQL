
---

### **El Operador `ALL`**

El operador `ALL` se utiliza para comparar un valor con **todos** los valores en un conjunto devuelto por una subconsulta. Es especialmente útil cuando necesitas verificar si un valor cumple una condición con respecto a **todos** los elementos de un conjunto.

---

### **Sintaxis del Operador `ALL`**

```sql
valor operador_comparación ALL (subconsulta)
```

- **`valor`**: El valor que deseas comparar.
- **`operador_comparación`**: Puede ser `=`, `<>`, `<`, `>`, `<=`, `>=`.
- **`subconsulta`**: Devuelve un conjunto de valores con los que se comparará `valor`.

---

### **Ejemplo 1: Uso de `ALL` con `<>`**

Supongamos que queremos encontrar todos los clientes que **nunca** han obtenido un alquiler de película gratuito (es decir, donde el monto del pago sea `0`):

```sql
SELECT first_name, last_name
FROM customer
WHERE customer_id <> ALL (
  SELECT customer_id
  FROM payment
  WHERE amount = 0
);
```

#### **Resultado:**
```sql
+-------------+--------------+
| first_name  | last_name    |
+-------------+--------------+
| MARY        | SMITH        |
| PATRICIA    | JOHNSON      |
| LINDA       | WILLIAMS     |
| BARBARA     | JONES        |
...
| AUSTIN      | CINTRON      |
+-------------+--------------+
576 rows in set (0.01 sec)
```

#### **Explicación:**
1. **Subconsulta**:
    - `SELECT customer_id FROM payment WHERE amount = 0`:
        - Devuelve los `customer_id` de los clientes que han realizado pagos de `0`.

2. **Consulta contenedora**:
    - `SELECT first_name, last_name FROM customer WHERE customer_id <> ALL (...)`:
        - Devuelve los clientes cuyo `customer_id` **no** está en la lista de clientes que han realizado pagos de `0`.

---

### **Alternativa con `NOT IN`**

El mismo resultado se puede lograr usando el operador `NOT IN`, que es más común y fácil de entender:

```sql
SELECT first_name, last_name
FROM customer
WHERE customer_id NOT IN (
  SELECT customer_id
  FROM payment
  WHERE amount = 0
);
```

#### **Explicación:**
- **`NOT IN`**: Verifica si un valor **no** está en un conjunto de valores.
- Es más intuitivo que `<> ALL` y produce el mismo resultado.

---

### **Precaución con Valores Nulos**

Si el conjunto de valores devuelto por la subconsulta contiene un valor `NULL`, el operador `NOT IN` o `<> ALL` puede producir resultados inesperados. Por ejemplo:

```sql
SELECT first_name, last_name
FROM customer
WHERE customer_id NOT IN (122, 452, NULL);
```

#### **Resultado:**
```sql
Empty set (0.00 sec)
```

#### **Explicación:**
- **`NULL`**: Cualquier comparación con `NULL` devuelve `NULL` (desconocido).
- Si el conjunto contiene `NULL`, la condición `NOT IN` o `<> ALL` no devolverá ningún resultado.

---

### **Ejemplo 2: Uso de `ALL` en la Cláusula `HAVING`**

Supongamos que queremos encontrar clientes cuyo número total de alquileres sea mayor que el de **todos** los clientes de América del Norte (Estados Unidos, México y Canadá):

```sql
SELECT customer_id, COUNT(*)
FROM rental
GROUP BY customer_id
HAVING COUNT(*) > ALL (
  SELECT COUNT(*)
  FROM rental r
    INNER JOIN customer c ON r.customer_id = c.customer_id
    INNER JOIN address a ON c.address_id = a.address_id
    INNER JOIN city ct ON a.city_id = ct.city_id
    INNER JOIN country co ON ct.country_id = co.country_id
  WHERE co.country IN ('United States', 'Mexico', 'Canada')
  GROUP BY r.customer_id
);
```

#### **Resultado:**
```sql
+-------------+----------+
| customer_id | count(*) |
+-------------+----------+
|         148 |       46 |
+-------------+----------+
1 row in set (0.01 sec)
```

#### **Explicación:**
1. **Subconsulta**:
    - `SELECT COUNT(*) FROM rental ... GROUP BY r.customer_id`:
        - Devuelve el número total de alquileres para cada cliente de América del Norte.

2. **Consulta contenedora**:
    - `SELECT customer_id, COUNT(*) FROM rental GROUP BY customer_id HAVING COUNT(*) > ALL (...)`:
        - Devuelve los clientes cuyo número total de alquileres es mayor que **todos** los clientes de América del Norte.

---

### **Resumen de Conceptos Clave**

1. **Operador `ALL`**:
    - Compara un valor con **todos** los valores en un conjunto.
    - Útil para condiciones como "mayor que todos" o "diferente de todos".

2. **Alternativa con `NOT IN`**:
    - Es más común y fácil de entender que `<> ALL`.

3. **Precaución con `NULL`**:
    - Si el conjunto contiene `NULL`, `NOT IN` o `<> ALL` pueden no funcionar como se espera.

4. **Uso en `HAVING`**:
    - Permite comparar agregaciones (como `COUNT(*)`) con un conjunto de valores.

---

### **Ejemplo Adicional**

Supongamos que queremos encontrar los productos cuyo precio es mayor que **todos** los precios de los productos en una categoría específica:

```sql
SELECT product_id, product_name, price
FROM products
WHERE price > ALL (
  SELECT price
  FROM products
  WHERE category_id = 5
);
```

#### **Explicación:**
1. **Subconsulta**:
    - `SELECT price FROM products WHERE category_id = 5`:
        - Devuelve los precios de los productos en la categoría `5`.

2. **Consulta contenedora**:
    - `SELECT product_id, product_name, price FROM products WHERE price > ALL (...)`:
        - Devuelve los productos cuyo precio es mayor que **todos** los precios de la categoría `5`.

---


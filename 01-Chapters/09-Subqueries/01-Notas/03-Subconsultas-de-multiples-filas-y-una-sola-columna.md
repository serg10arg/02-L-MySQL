
---

### **Subconsultas de Múltiples Filas y Una Sola Columna**

Cuando una subconsulta devuelve **múltiples filas con una sola columna**, no puedes usar operadores de igualdad (`=`, `<>`, etc.) directamente. Sin embargo, SQL proporciona operadores especiales para trabajar con este tipo de subconsultas. Los más comunes son `IN` y `NOT IN`.

---

### **Operador `IN`**

El operador `IN` se utiliza para verificar si un valor está presente en un conjunto de valores. Es especialmente útil cuando la subconsulta devuelve múltiples filas.

#### **Ejemplo sin Subconsulta**

Supongamos que queremos obtener los `country_id` de Canadá y México:

```sql
SELECT country_id
FROM country
WHERE country IN ('Canada', 'Mexico');
```

#### **Resultado:**
```sql
+------------+
| country_id |
+------------+
|         20 |
|         60 |
+------------+
2 rows in set (0.00 sec)
```

#### **Explicación:**
- La condición `country IN ('Canada', 'Mexico')` verifica si el valor de `country` está en el conjunto `('Canada', 'Mexico')`.
- Equivale a escribir:
  ```sql
  SELECT country_id
  FROM country
  WHERE country = 'Canada' OR country = 'Mexico';
  ```

---

### **Operador `IN` con Subconsultas**

El operador `IN` es aún más útil cuando se combina con subconsultas. Por ejemplo, si queremos obtener todas las ciudades que están en Canadá o México, podemos usar una subconsulta para obtener los `country_id` de esos países y luego usarlos en la consulta principal:

```sql
SELECT city_id, city
FROM city
WHERE country_id IN (
  SELECT country_id
  FROM country
  WHERE country IN ('Canada', 'Mexico')
);
```

#### **Resultado:**
```sql
+---------+----------------------------+
| city_id | city                       |
+---------+----------------------------+
|     179 | Gatineau                   |
|     196 | Halifax                    |
|     300 | Lethbridge                 |
|     313 | London                     |
|     383 | Oshawa                     |
|     430 | Richmond Hill              |
|     565 | Vancouver                  |
...
|     452 | San Juan Bautista Tuxtepec |
|     541 | Torren                     |
|     556 | Uruapan                    |
|     563 | Valle de Santiago          |
|     595 | Zapopan                    |
+---------+----------------------------+
37 rows in set (0.00 sec)
```

#### **Explicación:**
1. **Subconsulta**:
    - `SELECT country_id FROM country WHERE country IN ('Canada', 'Mexico')`:
        - Devuelve los `country_id` de Canadá (`20`) y México (`60`).

2. **Consulta contenedora**:
    - `SELECT city_id, city FROM city WHERE country_id IN (20, 60)`:
        - Devuelve todas las ciudades cuyo `country_id` sea `20` o `60`.

---

### **Operador `NOT IN`**

El operador `NOT IN` es lo opuesto a `IN`. Se utiliza para verificar si un valor **no** está presente en un conjunto de valores.

#### **Ejemplo con `NOT IN`**

Supongamos que queremos obtener todas las ciudades que **no** están en Canadá o México:

```sql
SELECT city_id, city
FROM city
WHERE country_id NOT IN (
  SELECT country_id
  FROM country
  WHERE country IN ('Canada', 'Mexico')
);
```

#### **Resultado:**
```sql
+---------+----------------------------+
| city_id | city                       |
+---------+----------------------------+
|       1 | A Corua (La Corua)         |
|       2 | Abha                       |
|       3 | Abu Dhabi                  |
|       5 | Adana                      |
|       6 | Addis Abeba                |
...
|     596 | Zaria                      |
|     597 | Zeleznogorsk               |
|     598 | Zhezqazghan                |
|     599 | Zhoushan                   |
|     600 | Ziguinchor                 |
+---------+----------------------------+
563 rows in set (0.00 sec)
```

#### **Explicación:**
1. **Subconsulta**:
    - `SELECT country_id FROM country WHERE country IN ('Canada', 'Mexico')`:
        - Devuelve los `country_id` de Canadá (`20`) y México (`60`).

2. **Consulta contenedora**:
    - `SELECT city_id, city FROM city WHERE country_id NOT IN (20, 60)`:
        - Devuelve todas las ciudades cuyo `country_id` **no** sea `20` o `60`.

---

### **Resumen de Conceptos Clave**

1. **Subconsultas de Múltiples Filas y Una Columna**:
    - Devuelven un conjunto de valores en una sola columna.
    - No se pueden usar con operadores de igualdad (`=`, `<>`).

2. **Operador `IN`**:
    - Verifica si un valor está en un conjunto de valores.
    - Útil para filtrar datos basados en subconsultas.

3. **Operador `NOT IN`**:
    - Verifica si un valor **no** está en un conjunto de valores.
    - Útil para excluir datos basados en subconsultas.

4. **Uso con Subconsultas**:
    - Permite filtrar datos dinámicamente basados en los resultados de otra consulta.

---

### **Ejemplo Adicional**

Supongamos que queremos obtener los nombres de los clientes que **no** han realizado ningún pago. Podemos usar una subconsulta con `NOT IN`:

```sql
SELECT first_name, last_name
FROM customer
WHERE customer_id NOT IN (
  SELECT customer_id
  FROM payment
);
```

#### **Explicación:**
1. **Subconsulta**:
    - `SELECT customer_id FROM payment`:
        - Devuelve los `customer_id` de todos los clientes que han realizado pagos.

2. **Consulta contenedora**:
    - `SELECT first_name, last_name FROM customer WHERE customer_id NOT IN (...)`:
        - Devuelve los clientes cuyos `customer_id` **no** están en la lista de pagos.

---


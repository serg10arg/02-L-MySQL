
---

### **Subconsultas Escalares**

Una **subconsulta escalar** es una subconsulta que devuelve un solo valor (una sola columna y una sola fila). Estas subconsultas son extremadamente versátiles y pueden usarse en varias partes de una consulta SQL, como en las cláusulas `SELECT`, `ORDER BY`, e incluso en la cláusula `VALUES` de una instrucción `INSERT`.

---

### **Uso de Subconsultas Escalares en la Cláusula `SELECT`**

En el ejemplo proporcionado, se utiliza una subconsulta escalar en la cláusula `SELECT` para obtener información adicional sobre los clientes, como su nombre, apellido y ciudad, sin necesidad de hacer un `JOIN` explícito con las tablas relacionadas.

#### Consulta:
```sql
SELECT
    (SELECT c.first_name FROM customer c
     WHERE c.customer_id = p.customer_id) first_name,
    (SELECT c.last_name FROM customer c
     WHERE c.customer_id = p.customer_id) last_name,
    (SELECT ct.city
     FROM customer c
     INNER JOIN address a ON c.address_id = a.address_id
     INNER JOIN city ct ON a.city_id = ct.city_id
     WHERE c.customer_id = p.customer_id) city,
    SUM(p.amount) tot_payments,
    COUNT(*) tot_rentals
FROM payment p
GROUP BY p.customer_id;
```

#### Explicación:

1. **Subconsultas en `SELECT`:**
    - Cada subconsulta en la cláusula `SELECT` devuelve un solo valor (escalar).
    - **Primera subconsulta:** Obtiene el `first_name` del cliente desde la tabla `customer`.
    - **Segunda subconsulta:** Obtiene el `last_name` del cliente desde la tabla `customer`.
    - **Tercera subconsulta:** Obtiene la `city` del cliente, uniendo las tablas `customer`, `address` y `city`.

2. **Agrupación y agregación:**
    - La consulta principal agrupa los resultados por `customer_id` y calcula:
        - `SUM(p.amount)`: El total de pagos realizados por cada cliente.
        - `COUNT(*)`: El número total de alquileres realizados por cada cliente.

3. **Resultado:**
    - La consulta devuelve una lista de clientes con su nombre, apellido, ciudad, total de pagos y total de alquileres.

---

### **Diferencias con el Enfoque Tradicional (Usando `JOIN`)**

El texto menciona que hay dos diferencias principales entre este enfoque y el tradicional (usando `JOIN`):

1. **Subconsultas en lugar de `JOIN`:**
    - En lugar de unir las tablas `customer`, `address` y `city` con `payment` usando `JOIN`, se utilizan subconsultas escalares en la cláusula `SELECT`.
    - Esto permite evitar la complejidad de múltiples `JOIN` y mantener la consulta más modular.

2. **Acceso múltiple a la tabla `customer`:**
    - La tabla `customer` se accede tres veces (una por cada subconsulta), ya que cada subconsulta escalar solo puede devolver una columna.
    - Esto puede ser menos eficiente que un `JOIN`, pero es más flexible y fácil de entender en algunos casos.

---

### **Uso de Subconsultas Escalares en la Cláusula `ORDER BY`**

Las subconsultas escalares también pueden usarse en la cláusula `ORDER BY` para ordenar los resultados basados en un valor calculado dinámicamente.

#### Consulta:
```sql
SELECT a.actor_id, a.first_name, a.last_name
FROM actor a
ORDER BY
    (SELECT COUNT(*) FROM film_actor fa
     WHERE fa.actor_id = a.actor_id) DESC;
```

#### Explicación:

1. **Subconsulta en `ORDER BY`:**
    - La subconsulta cuenta el número de películas en las que ha participado cada actor (`COUNT(*)`).
    - Este valor se usa para ordenar los actores de mayor a menor número de películas.

2. **Resultado:**
    - La consulta devuelve una lista de actores ordenados por la cantidad de películas en las que han participado.

---

### **Uso de Subconsultas Escalares en la Cláusula `VALUES` de un `INSERT`**

Las subconsultas escalares también pueden usarse en la cláusula `VALUES` de una instrucción `INSERT` para generar valores dinámicamente.

#### Consulta:
```sql
INSERT INTO film_actor (actor_id, film_id, last_update)
VALUES (
    (SELECT actor_id FROM actor
     WHERE first_name = 'JENNIFER' AND last_name = 'DAVIS'),
    (SELECT film_id FROM film
     WHERE title = 'ACE GOLDFINGER'),
    NOW()
);
```

#### Explicación:

1. **Subconsultas en `VALUES`:**
    - **Primera subconsulta:** Obtiene el `actor_id` de la tabla `actor` para el actor con nombre "JENNIFER DAVIS".
    - **Segunda subconsulta:** Obtiene el `film_id` de la tabla `film` para la película "ACE GOLDFINGER".
    - `NOW()`: Inserta la fecha y hora actuales en la columna `last_update`.

2. **Resultado:**
    - Se inserta una nueva fila en la tabla `film_actor` con los valores obtenidos dinámicamente.

---

### **Ventajas de Usar Subconsultas Escalares**

1. **Modularidad:**
    - Permite dividir una consulta compleja en partes más pequeñas y manejables.
2. **Legibilidad:**
    - Hace que el código sea más fácil de entender y mantener.
3. **Flexibilidad:**
    - Pueden usarse en múltiples partes de una consulta (`SELECT`, `ORDER BY`, `INSERT`, etc.).
4. **Evita `JOIN` innecesarios:**
    - En algunos casos, evita la necesidad de hacer `JOIN` explícitos, lo que puede simplificar la consulta.

---

### **Consideraciones de Rendimiento**

Aunque las subconsultas escalares son poderosas, es importante tener en cuenta que:

1. **Acceso múltiple a tablas:**
    - Si una subconsulta accede a la misma tabla varias veces, puede ser menos eficiente que un `JOIN`.
2. **Optimización:**
    - En consultas muy grandes o complejas, es recomendable analizar el plan de ejecución para asegurar un buen rendimiento.

---

### **Conclusión**

Las subconsultas escalares son una herramienta poderosa en SQL que permite generar valores dinámicos y simplificar consultas complejas. Pueden usarse en diversas partes de una consulta, como `SELECT`, `ORDER BY`, e incluso en `INSERT`, lo que las hace muy versátiles. Sin embargo, es importante equilibrar su uso con consideraciones de rendimiento y legibilidad del código.
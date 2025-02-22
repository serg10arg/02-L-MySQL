
---

### **Conceptos Básicos de Joins**

En SQL, los **joins** se utilizan para combinar filas de dos o más tablas basándose en una condición relacionada entre ellas. Hasta ahora, el texto menciona que ya deberías estar familiarizado con el **Inner Join**, que es el tipo de join más común. Sin embargo, este capítulo se centra en otros tipos de joins, como el **Outer Join** y el **Cross Join**.

---

### **Inner Join vs Outer Join**

#### **Inner Join**
- **Definición:** Un `INNER JOIN` devuelve solo las filas que tienen coincidencias en ambas tablas basándose en la condición de unión.
- **Comportamiento:** Si una fila de la primera tabla no tiene una coincidencia en la segunda tabla, esa fila no se incluye en el resultado.
- **Ejemplo:**
  ```sql
  SELECT f.film_id, f.title, COUNT(*) num_copies
  FROM film f
  INNER JOIN inventory i ON f.film_id = i.film_id
  GROUP BY f.film_id, f.title;
  ```
    - **Resultado:** Devuelve solo las películas que tienen copias en el inventario (958 filas).

#### **Outer Join**
- **Definición:** Un `OUTER JOIN` devuelve todas las filas de una tabla (la tabla "izquierda" o "derecha"), incluso si no hay coincidencias en la otra tabla.
- **Tipos de Outer Join:**
    - **LEFT OUTER JOIN:** Devuelve todas las filas de la tabla izquierda (`film`), y las filas coincidentes de la tabla derecha (`inventory`). Si no hay coincidencias, las columnas de la tabla derecha tendrán valores `NULL`.
    - **RIGHT OUTER JOIN:** Devuelve todas las filas de la tabla derecha, y las filas coincidentes de la tabla izquierda.
    - **FULL OUTER JOIN:** Devuelve todas las filas de ambas tablas, con `NULL` en las columnas donde no hay coincidencias (no soportado en MySQL).

---

### **Ejemplo Práctico: Inner Join vs Left Outer Join**

#### **1. Inner Join**
```sql
SELECT f.film_id, f.title, COUNT(*) num_copies
FROM film f
INNER JOIN inventory i ON f.film_id = i.film_id
GROUP BY f.film_id, f.title;
```
- **Resultado:** 958 filas.
- **Explicación:** Solo se incluyen las películas que tienen al menos una copia en el inventario. Las películas sin copias (como `Alice Fantasia`) no aparecen en los resultados.

#### **2. Left Outer Join**
```sql
SELECT f.film_id, f.title, COUNT(i.inventory_id) num_copies
FROM film f
LEFT OUTER JOIN inventory i ON f.film_id = i.film_id
GROUP BY f.film_id, f.title;
```
- **Resultado:** 1000 filas.
- **Explicación:** Se incluyen todas las películas, incluso aquellas sin copias en el inventario. Para estas películas, `num_copies` es `0`.

---

### **Diferencias Clave Entre Inner Join y Left Outer Join**

1. **Cobertura de Datos:**
    - **Inner Join:** Solo incluye filas con coincidencias en ambas tablas.
    - **Left Outer Join:** Incluye todas las filas de la tabla izquierda (`film`), independientemente de si hay coincidencias en la tabla derecha (`inventory`).

2. **Conteo de Filas:**
    - **Inner Join:** Devuelve 958 filas (solo películas con copias en inventario).
    - **Left Outer Join:** Devuelve 1000 filas (todas las películas, incluyendo las sin copias).

3. **Valores Nulos:**
    - **Inner Join:** No hay valores `NULL` en el resultado.
    - **Left Outer Join:** Las películas sin copias en inventario tendrán `NULL` en las columnas de `inventory`, pero `COUNT(i.inventory_id)` las cuenta como `0`.

---

### **Ejemplo Detallado con Filtros**

#### **1. Inner Join con Filtro**
```sql
SELECT f.film_id, f.title, i.inventory_id
FROM film f
INNER JOIN inventory i ON f.film_id = i.film_id
WHERE f.film_id BETWEEN 13 AND 15;
```
- **Resultado:** 10 filas.
- **Explicación:** Solo se incluyen las películas con copias en inventario (`Ali Forever` y `Alien Center`).

#### **2. Left Outer Join con Filtro**
```sql
SELECT f.film_id, f.title, i.inventory_id
FROM film f
LEFT OUTER JOIN inventory i ON f.film_id = i.film_id
WHERE f.film_id BETWEEN 13 AND 15;
```
- **Resultado:** 11 filas.
- **Explicación:** Se incluyen todas las películas en el rango, incluso `Alice Fantasia`, que no tiene copias en inventario. Para esta película, `inventory_id` es `NULL`.

---

### **Cambios en la Consulta al Usar Outer Join**

1. **Tipo de Join:**
    - Se cambió de `INNER JOIN` a `LEFT OUTER JOIN`.

2. **Conteo de Copias:**
    - Se cambió de `COUNT(*)` a `COUNT(i.inventory_id)`.
    - **Razón:** `COUNT(*)` cuenta todas las filas, incluyendo aquellas con `NULL`. `COUNT(i.inventory_id)` solo cuenta valores no nulos, lo que es más preciso para este caso.

---

### **Conclusión**

- **Inner Join:** Ideal cuando solo necesitas filas con coincidencias en ambas tablas.
- **Left Outer Join:** Útil cuando necesitas incluir todas las filas de una tabla, incluso si no hay coincidencias en la otra.
- **Elección del Join:** Depende de los requisitos de la consulta. Si necesitas incluir datos "incompletos" o sin coincidencias, un `OUTER JOIN` es la mejor opción.

Este enfoque te permite manejar escenarios donde no todas las relaciones entre tablas son perfectas, como películas sin copias en inventario o clientes sin pedidos.

---

### **Joins Externos: Left vs Right**

En SQL, los **joins externos** (outer joins) se utilizan para combinar filas de dos tablas, incluso si no hay coincidencias entre ellas. La principal diferencia entre un **Left Outer Join** y un **Right Outer Join** radica en qué tabla determina el número de filas en el conjunto de resultados.

---

### **Left Outer Join**

- **Definición:** Un `LEFT OUTER JOIN` (o simplemente `LEFT JOIN`) devuelve **todas las filas de la tabla izquierda** (la primera tabla mencionada), y las filas coincidentes de la tabla derecha (la segunda tabla mencionada). Si no hay coincidencias, las columnas de la tabla derecha tendrán valores `NULL`.
- **Uso típico:** Se utiliza cuando quieres incluir todos los registros de la tabla izquierda, independientemente de si tienen coincidencias en la tabla derecha.

#### Ejemplo:
```sql
SELECT f.film_id, f.title, i.inventory_id
FROM film f
LEFT OUTER JOIN inventory i ON f.film_id = i.film_id
WHERE f.film_id BETWEEN 13 AND 15;
```
- **Resultado:** Incluye todas las películas en el rango especificado, incluso si no tienen copias en el inventario (como `Alice Fantasia`).

---

### **Right Outer Join**

- **Definición:** Un `RIGHT OUTER JOIN` (o simplemente `RIGHT JOIN`) devuelve **todas las filas de la tabla derecha**, y las filas coincidentes de la tabla izquierda. Si no hay coincidencias, las columnas de la tabla izquierda tendrán valores `NULL`.
- **Uso típico:** Se utiliza cuando quieres incluir todos los registros de la tabla derecha, independientemente de si tienen coincidencias en la tabla izquierda.

#### Ejemplo:
```sql
SELECT f.film_id, f.title, i.inventory_id
FROM inventory i
RIGHT OUTER JOIN film f ON f.film_id = i.film_id
WHERE f.film_id BETWEEN 13 AND 15;
```
- **Resultado:** Es **idéntico** al del `LEFT OUTER JOIN` anterior, porque se han invertido las tablas en el `JOIN`. La tabla `film` sigue siendo la que determina el número de filas.

---

### **¿Cuál es la Diferencia Real?**

La diferencia entre `LEFT OUTER JOIN` y `RIGHT OUTER JOIN` es **sintáctica** y depende del orden en que se escriben las tablas en la consulta:

1. **Left Outer Join:**
    - La tabla izquierda (`film`) es la principal.
    - Se incluyen todas sus filas, incluso si no hay coincidencias en la tabla derecha (`inventory`).

2. **Right Outer Join:**
    - La tabla derecha (`film`) es la principal.
    - Se incluyen todas sus filas, incluso si no hay coincidencias en la tabla izquierda (`inventory`).

---

### **¿Por qué Preferir Left Outer Join?**

El texto recomienda **usar siempre `LEFT OUTER JOIN`** en lugar de `RIGHT OUTER JOIN`. Aquí las razones:

1. **Legibilidad:**
    - Es más natural pensar en la tabla principal (la que contiene todos los registros) como la primera tabla en la consulta.
    - `LEFT JOIN` es más común y ampliamente utilizado, por lo que es más fácil de entender para otros desarrolladores.

2. **Compatibilidad:**
    - No todos los sistemas de bases de datos soportan `RIGHT OUTER JOIN`, pero todos soportan `LEFT OUTER JOIN`.

3. **Claridad:**
    - Al usar `LEFT JOIN`, el flujo de la consulta es más intuitivo: "Dame todo de la tabla A, y si hay coincidencias en la tabla B, inclúyelas".

---

### **Ejemplo Comparativo**

#### **1. Left Outer Join**
```sql
SELECT f.film_id, f.title, i.inventory_id
FROM film f
LEFT OUTER JOIN inventory i ON f.film_id = i.film_id
WHERE f.film_id BETWEEN 13 AND 15;
```
- **Explicación:** La tabla `film` es la principal. Se incluyen todas las películas en el rango, incluso si no tienen copias en el inventario.

#### **2. Right Outer Join**
```sql
SELECT f.film_id, f.title, i.inventory_id
FROM inventory i
RIGHT OUTER JOIN film f ON f.film_id = i.film_id
WHERE f.film_id BETWEEN 13 AND 15;
```
- **Explicación:** La tabla `film` sigue siendo la principal, pero se ha invertido el orden en el `JOIN`. El resultado es el mismo que el del `LEFT JOIN`.

---

### **¿Cuándo Usar Right Outer Join?**

Aunque no es común, hay casos en los que un `RIGHT OUTER JOIN` puede ser útil:

1. **Legibilidad en Consultas Complejas:**
    - En consultas con múltiples joins, a veces es más claro usar `RIGHT JOIN` para mantener la lógica de la consulta.

2. **Preferencia Personal:**
    - Algunos desarrolladores prefieren usar `RIGHT JOIN` cuando la tabla derecha es la principal en su lógica de negocio.

---

### **Conclusión**

- **Left Outer Join:** Es el estándar recomendado. Es más claro, más compatible y más utilizado.
- **Right Outer Join:** Puede ser útil en casos específicos, pero no es necesario en la mayoría de las situaciones.
- **Intercambiabilidad:** Un `LEFT JOIN` puede convertirse en un `RIGHT JOIN` (y viceversa) simplemente invirtiendo el orden de las tablas.

En resumen, **siempre usa `LEFT OUTER JOIN`** para mantener tu código limpio, legible y compatible con todos los sistemas de bases de datos.
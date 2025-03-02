
---

### **¿Qué es un Natural Join?**

Un **Natural Join** es un tipo de join en SQL que **infiere automáticamente las condiciones de unión** basándose en columnas con el mismo nombre en las tablas que se están uniendo. Es decir, no necesitas escribir explícitamente la condición `ON` o `USING`, ya que el motor de la base de datos la deduce por ti.

- **Características:**
    - **Simplicidad:** No requiere que especifiques las columnas para la unión.
    - **Riesgo:** Puede generar resultados inesperados si las tablas tienen columnas con el mismo nombre que no están relacionadas.

---

### **Ejemplo de Natural Join**

El texto proporciona un ejemplo de un `NATURAL JOIN` entre las tablas `customer` y `rental`:

```sql
SELECT c.first_name, c.last_name, DATE(r.rental_date)
FROM customer c
NATURAL JOIN rental r;
```

#### Resultado:
```sql
Empty set (0.04 sec)
```

- **Explicación:**
    - El `NATURAL JOIN` infiere las condiciones de unión basándose en columnas con el mismo nombre en ambas tablas.
    - En este caso, las tablas `customer` y `rental` tienen dos columnas con el mismo nombre:
        1. `customer_id`: Es la clave foránea en `rental` y la clave primaria en `customer`.
        2. `last_update`: Una columna que indica la última vez que se modificó la fila.
    - El servidor añade automáticamente las condiciones:
      ```sql
      r.customer_id = c.customer_id AND r.last_update = c.last_update
      ```
    - Como es poco probable que `last_update` sea igual en ambas tablas, la consulta no devuelve filas.

---

### **Problema con el Natural Join**

El principal problema con el `NATURAL JOIN` es que **puede inferir condiciones de unión no deseadas** si las tablas tienen columnas con el mismo nombre que no están relacionadas. En el ejemplo:
- La columna `last_update` no está relacionada con la lógica de negocio, pero el `NATURAL JOIN` la incluye en la condición de unión.
- Esto hace que la consulta no devuelva resultados, ya que es improbable que `last_update` coincida en ambas tablas.

---

### **Solución: Usar una Subconsulta**

Para evitar este problema, el texto sugiere usar una **subconsulta** para restringir las columnas de una de las tablas, eliminando así las columnas no deseadas.

#### Consulta Corregida:
```sql
SELECT cust.first_name, cust.last_name, DATE(r.rental_date)
FROM (
  SELECT customer_id, first_name, last_name
  FROM customer
) cust
NATURAL JOIN rental r;
```

#### Resultado:
```sql
+------------+-----------+---------------------+
| first_name | last_name | date(r.rental_date) |
+------------+-----------+---------------------+
| MARY       | SMITH     | 2005-05-25          |
| MARY       | SMITH     | 2005-05-28          |
| MARY       | SMITH     | 2005-06-15          |
| ...        | ...       | ...                 |
| AUSTIN     | CINTRON   | 2005-08-23          |
+------------+-----------+---------------------+
16044 rows in set (0.03 sec)
```

- **Explicación:**
    - La subconsulta selecciona solo las columnas `customer_id`, `first_name`, y `last_name` de la tabla `customer`.
    - Al hacer el `NATURAL JOIN` con `rental`, la única columna común es `customer_id`.
    - El servidor infiere la condición `r.customer_id = cust.customer_id`, que es correcta.
    - La consulta ahora devuelve los resultados esperados.

---

### **¿Vale la Pena Usar Natural Joins?**

El texto concluye que **no vale la pena usar `NATURAL JOIN`** debido a sus riesgos y limitaciones. En su lugar, recomienda usar **`INNER JOIN` con condiciones explícitas**, ya que:

1. **Claridad:** Las condiciones de unión son explícitas y fáciles de entender.
2. **Control:** Evitas problemas causados por columnas con el mismo nombre que no están relacionadas.
3. **Mantenimiento:** Es más fácil modificar y depurar consultas con condiciones explícitas.

#### Ejemplo con `INNER JOIN`:
```sql
SELECT c.first_name, c.last_name, DATE(r.rental_date)
FROM customer c
INNER JOIN rental r ON c.customer_id = r.customer_id;
```

---

### **Conclusión**

- **Natural Join:** Es una herramienta que puede simplificar la escritura de consultas, pero tiene riesgos significativos.
- **Problema Principal:** Infiere condiciones de unión basadas en nombres de columnas, lo que puede generar resultados inesperados.
- **Recomendación:** Usa **`INNER JOIN` con condiciones explícitas** para mayor claridad y control.

En resumen, aunque el `NATURAL JOIN` puede parecer conveniente, es mejor evitarlo y optar por un enfoque más explícito y seguro. 
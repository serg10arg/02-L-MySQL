
---

### **Condiciones de Filtro de Grupo: `WHERE` vs. `HAVING`**

Cuando trabajas con consultas que incluyen agrupaciones (`GROUP BY`), es importante entender cuándo usar la cláusula `WHERE` y cuándo usar la cláusula `HAVING` para filtrar datos. Ambas cláusulas se utilizan para aplicar condiciones de filtro, pero se aplican en diferentes momentos del proceso de consulta.

---

### **Diferencia entre `WHERE` y `HAVING`**

1. **`WHERE`**:
    - Se aplica **antes** de la agrupación (`GROUP BY`).
    - Filtra filas individuales de la tabla.
    - No puede usar funciones de agregación (como `COUNT`, `SUM`, `AVG`, etc.).

2. **`HAVING`**:
    - Se aplica **después** de la agrupación (`GROUP BY`).
    - Filtra grupos completos de datos.
    - Puede usar funciones de agregación (como `COUNT`, `SUM`, `AVG`, etc.).

---

### **Ejemplo de la Consulta**

```sql
SELECT fa.actor_id, f.rating, COUNT(*)
FROM film_actor fa
  INNER JOIN film f
  ON fa.film_id = f.film_id
WHERE f.rating IN ('G', 'PG')
GROUP BY fa.actor_id, f.rating
HAVING COUNT(*) > 9;
```

#### **Resultado:**
```sql
+----------+--------+----------+
| actor_id | rating | count(*) |
+----------+--------+----------+
|      137 | PG     |       10 |
|       37 | PG     |       12 |
|      180 | PG     |       12 |
|        7 | G      |       10 |
|       83 | G      |       14 |
|      129 | G      |       12 |
|      111 | PG     |       15 |
|       44 | PG     |       12 |
|       26 | PG     |       11 |
|       92 | PG     |       12 |
|       17 | G      |       12 |
|      158 | PG     |       10 |
|      147 | PG     |       10 |
|       14 | G      |       10 |
|      102 | PG     |       11 |
|      133 | PG     |       10 |
+----------+--------+----------+
16 rows in set (0.01 sec)
```

---

### **Explicación de la Consulta**

1. **`WHERE f.rating IN ('G', 'PG')`**:
    - Filtra las filas de la tabla `film` para incluir solo las películas con clasificación `G` o `PG`.
    - Este filtro se aplica **antes** de la agrupación.

2. **`GROUP BY fa.actor_id, f.rating`**:
    - Agrupa los datos por `actor_id` y `rating`.
    - Después de este paso, tenemos grupos de películas para cada combinación de actor y clasificación.

3. **`HAVING COUNT(*) > 9`**:
    - Filtra los grupos para incluir solo aquellos con más de 9 películas.
    - Este filtro se aplica **después** de la agrupación.

---

### **¿Qué pasa si usas `WHERE` con una función de agregación?**

Si intentas usar una función de agregación (como `COUNT`) en la cláusula `WHERE`, obtendrás un error:

```sql
SELECT fa.actor_id, f.rating, COUNT(*)
FROM film_actor fa
  INNER JOIN film f
  ON fa.film_id = f.film_id
WHERE f.rating IN ('G', 'PG')
  AND COUNT(*) > 9
GROUP BY fa.actor_id, f.rating;
```

#### **Error:**
```
ERROR 1111 (HY000): Invalid use of group function
```

#### **Explicación:**
- La cláusula `WHERE` se evalúa **antes** de la agrupación, por lo que no puede acceder a los resultados de funciones de agregación como `COUNT`.
- Para filtrar basado en funciones de agregación, debes usar `HAVING`.

---

### **Resumen de Cuándo Usar `WHERE` y `HAVING`**

| Condición de Filtro          | Cláusula a Usar | Momento de Aplicación          |
|------------------------------|-----------------|--------------------------------|
| Filtra filas individuales    | `WHERE`         | Antes de la agrupación (`GROUP BY`). |
| Filtra grupos completos      | `HAVING`        | Después de la agrupación (`GROUP BY`). |
| Usa funciones de agregación  | `HAVING`        | Después de la agrupación (`GROUP BY`). |

---

### **Ejemplo Adicional**

Si quisieras filtrar actores que han actuado en más de 15 películas en total (sin importar la clasificación), usarías:

```sql
SELECT fa.actor_id, COUNT(*)
FROM film_actor fa
  INNER JOIN film f
  ON fa.film_id = f.film_id
GROUP BY fa.actor_id
HAVING COUNT(*) > 15;
```

---

### **Consejos para Escribir Consultas con `GROUP BY`**

1. **Identifica qué filtros aplican a filas individuales**:
    - Usa `WHERE` para estos filtros.
    - Ejemplo: `WHERE f.rating IN ('G', 'PG')`.

2. **Identifica qué filtros aplican a grupos completos**:
    - Usa `HAVING` para estos filtros.
    - Ejemplo: `HAVING COUNT(*) > 9`.

3. **No mezcles `WHERE` y `HAVING` incorrectamente**:
    - Si usas una función de agregación en `WHERE`, obtendrás un error.

---


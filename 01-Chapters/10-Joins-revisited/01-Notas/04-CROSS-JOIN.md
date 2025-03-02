
---

### **¿Qué es un Cross Join?**

Un **Cross Join** (o **Producto Cartesiano**) es un tipo de join en SQL que combina cada fila de una tabla con cada fila de otra tabla. Esto significa que si la primera tabla tiene **N** filas y la segunda tiene **M** filas, el resultado será una tabla con **N × M** filas.

- **Características:**
    - No requiere una condición de unión (`ON` o `USING`).
    - Genera todas las combinaciones posibles entre las filas de las dos tablas.
    - Es útil en situaciones específicas, como la generación de combinaciones o la fabricación de datos.

---

### **Ejemplo Básico de Cross Join**

El texto proporciona un ejemplo básico de un `CROSS JOIN` entre las tablas `category` y `language`:

```sql
SELECT c.name category_name, l.name language_name
FROM category c
CROSS JOIN language l;
```

#### Resultado:
```sql
+---------------+---------------+
| category_name | language_name |
+---------------+---------------+
| Action        | English       |
| Action        | Italian       |
| Action        | Japanese      |
| Action        | Mandarin      |
| Action        | French        |
| Action        | German        |
| Animation     | English       |
| Animation     | Italian       |
| Animation     | Japanese      |
| Animation     | Mandarin      |
| Animation     | French        |
| Animation     | German        |
| ...           | ...           |
| Travel        | English       |
| Travel        | Italian       |
| Travel        | Japanese      |
| Travel        | Mandarin      |
| Travel        | French        |
| Travel        | German        |
+---------------+---------------+
96 rows in set (0.00 sec)
```

- **Explicación:**
    - La tabla `category` tiene 16 filas.
    - La tabla `language` tiene 6 filas.
    - El `CROSS JOIN` genera **16 × 6 = 96** filas, combinando cada categoría con cada idioma.

---

### **¿Para qué sirve un Cross Join?**

Aunque los `CROSS JOIN` no son comunes en consultas diarias, tienen usos específicos:

1. **Generación de Combinaciones:**
    - Útil para crear todas las combinaciones posibles entre dos conjuntos de datos.
    - Ejemplo: Combinar categorías de productos con idiomas para generar una lista de traducciones.

2. **Fabricación de Datos:**
    - Se puede usar para generar datos ficticios o tablas temporales.
    - Ejemplo: Crear una tabla con todos los días del año.

---

### **Uso Avanzado de Cross Join: Generación de Fechas**

El texto muestra un ejemplo avanzado donde se usa un `CROSS JOIN` para generar todas las fechas del año 2020. Este es un caso práctico y útil.

#### Paso 1: Generar Números del 0 al 399

Se crean tres subconsultas que generan números:
- `ones`: Números del 0 al 9.
- `tens`: Números del 0 al 90 (incrementos de 10).
- `hundreds`: Números del 0 al 300 (incrementos de 100).

```sql
SELECT ones.num + tens.num + hundreds.num
FROM
  (SELECT 0 num UNION ALL SELECT 1 num UNION ALL SELECT 2 num UNION ALL SELECT 3 num UNION ALL
   SELECT 4 num UNION ALL SELECT 5 num UNION ALL SELECT 6 num UNION ALL SELECT 7 num UNION ALL
   SELECT 8 num UNION ALL SELECT 9 num) ones
CROSS JOIN
  (SELECT 0 num UNION ALL SELECT 10 num UNION ALL SELECT 20 num UNION ALL SELECT 30 num UNION ALL
   SELECT 40 num UNION ALL SELECT 50 num UNION ALL SELECT 60 num UNION ALL SELECT 70 num UNION ALL
   SELECT 80 num UNION ALL SELECT 90 num) tens
CROSS JOIN
  (SELECT 0 num UNION ALL SELECT 100 num UNION ALL SELECT 200 num UNION ALL SELECT 300 num) hundreds;
```

- **Resultado:** 400 filas con números del 0 al 399.

#### Paso 2: Convertir Números en Fechas

Se usa la función `DATE_ADD` para convertir los números en fechas, comenzando desde el 1 de enero de 2020.

```sql
SELECT DATE_ADD('2020-01-01', INTERVAL (ones.num + tens.num + hundreds.num) DAY) dt
FROM
  (SELECT 0 num UNION ALL SELECT 1 num UNION ALL ... SELECT 9 num) ones
CROSS JOIN
  (SELECT 0 num UNION ALL SELECT 10 num UNION ALL ... SELECT 90 num) tens
CROSS JOIN
  (SELECT 0 num UNION ALL SELECT 100 num UNION ALL ... SELECT 300 num) hundreds
WHERE DATE_ADD('2020-01-01', INTERVAL (ones.num + tens.num + hundreds.num) DAY) < '2021-01-01'
ORDER BY 1;
```

- **Resultado:** 366 filas con todas las fechas del año 2020 (incluyendo el 29 de febrero, ya que 2020 fue bisiesto).

---

### **Uso Práctico: Reporte de Alquileres por Día**

El texto muestra cómo usar esta tabla de fechas generada para crear un reporte que incluya todos los días del año, incluso aquellos sin alquileres.

#### Consulta:
```sql
SELECT days.dt, COUNT(r.rental_id) num_rentals
FROM rental r
RIGHT OUTER JOIN (
  SELECT DATE_ADD('2005-01-01', INTERVAL (ones.num + tens.num + hundreds.num) DAY) dt
  FROM
    (SELECT 0 num UNION ALL SELECT 1 num UNION ALL ... SELECT 9 num) ones
    CROSS JOIN
    (SELECT 0 num UNION ALL SELECT 10 num UNION ALL ... SELECT 90 num) tens
    CROSS JOIN
    (SELECT 0 num UNION ALL SELECT 100 num UNION ALL ... SELECT 300 num) hundreds
  WHERE DATE_ADD('2005-01-01', INTERVAL (ones.num + tens.num + hundreds.num) DAY) < '2006-01-01'
) days
ON days.dt = DATE(r.rental_date)
GROUP BY days.dt
ORDER BY 1;
```

- **Explicación:**
    - Se genera una tabla con todas las fechas del año 2005.
    - Se hace un `RIGHT OUTER JOIN` con la tabla `rental` para incluir todos los días, incluso aquellos sin alquileres.
    - Se cuentan los alquileres por día (`COUNT(r.rental_id)`).

#### Resultado:
```sql
+------------+-------------+
| dt         | num_rentals |
+------------+-------------+
| 2005-01-01 |           0 |
| 2005-01-02 |           0 |
| ...        | ...         |
| 2005-05-24 |           8 |
| 2005-05-25 |         137 |
| ...        | ...         |
| 2005-12-31 |           0 |
+------------+-------------+
365 rows in set (8.99 sec)
```

- **Interpretación:**
    - El reporte muestra todos los días del año 2005.
    - Los días sin alquileres tienen `num_rentals = 0`.

---

### **Conclusión**

- **Cross Join:** Es una herramienta poderosa para generar combinaciones o fabricar datos.
- **Uso Práctico:** Puede ser útil en situaciones específicas, como la generación de fechas o combinaciones de categorías.
- **Flexibilidad:** Combinado con otras funciones y cláusulas (como `DATE_ADD`, `RIGHT OUTER JOIN`, y `GROUP BY`), permite resolver problemas complejos de manera creativa.

Aunque no es una herramienta de uso diario, el `CROSS JOIN` es una adición valiosa a tu kit de herramientas de SQL, especialmente cuando necesitas generar datos o combinaciones que no existen en tus tablas.
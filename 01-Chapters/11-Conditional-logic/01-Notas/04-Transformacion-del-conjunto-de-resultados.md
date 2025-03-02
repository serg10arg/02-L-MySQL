
---

### **¿Qué es una Transformación del Conjunto de Resultados?**

Una **transformación del conjunto de resultados** es una técnica en SQL que permite reorganizar los datos de un formato de filas a un formato de columnas. Esto es útil cuando necesitas mostrar datos agregados en un formato más compacto o específico, como una sola fila con múltiples columnas.

---

### **Ejemplo Inicial: Agregación por Mes**

El texto comienza con un ejemplo en el que se cuenta el número de alquileres de películas para los meses de mayo, junio y julio de 2005.

#### Consulta Original:
```sql
SELECT MONTHNAME(rental_date) rental_month,
       COUNT(*) num_rentals
FROM rental
WHERE rental_date BETWEEN '2005-05-01' AND '2005-08-01'
GROUP BY MONTHNAME(rental_date);
```

#### Resultado:
| rental_month | num_rentals |
|--------------|-------------|
| May          |        1156 |
| June         |        2311 |
| July         |        6709 |

- **Explicación:**
    - La consulta agrupa los alquileres por mes (`MONTHNAME(rental_date)`).
    - Devuelve una fila por mes con el número de alquileres correspondiente.

---

### **Transformación a una Sola Fila con Columnas por Mes**

El texto muestra cómo transformar este resultado en una sola fila con tres columnas: una para cada mes.

#### Consulta Transformada:
```sql
SELECT
    SUM(CASE WHEN MONTHNAME(rental_date) = 'May' THEN 1 ELSE 0 END) May_rentals,
    SUM(CASE WHEN MONTHNAME(rental_date) = 'June' THEN 1 ELSE 0 END) June_rentals,
    SUM(CASE WHEN MONTHNAME(rental_date) = 'July' THEN 1 ELSE 0 END) July_rentals
FROM rental
WHERE rental_date BETWEEN '2005-05-01' AND '2005-08-01';
```

#### Resultado:
| May_rentals | June_rentals | July_rentals |
|-------------|--------------|--------------|
|        1156 |         2311 |         6709 |

---

### **Explicación de la Consulta Transformada**

1. **Expresiones `CASE`:**
    - Cada expresión `CASE` evalúa si el mes del alquiler (`MONTHNAME(rental_date)`) coincide con un mes específico (`'May'`, `'June'`, `'July'`).
    - Si coincide, devuelve `1`; de lo contrario, devuelve `0`.

2. **Función `SUM`:**
    - Suma los valores devueltos por las expresiones `CASE`.
    - Como cada `CASE` devuelve `1` para los alquileres del mes correspondiente, la suma total es el número de alquileres para ese mes.

3. **Resultado:**
    - Cada columna (`May_rentals`, `June_rentals`, `July_rentals`) contiene el número de alquileres para el mes correspondiente.

---

### **Ventajas de esta Transformación**

1. **Formato Compacto:**
    - Los datos se presentan en una sola fila, lo que puede ser más fácil de leer o exportar.

2. **Flexibilidad:**
    - Puedes personalizar las columnas según tus necesidades (por ejemplo, agregar más meses o cambiar los criterios).

3. **Compatibilidad:**
    - Esta técnica funciona en la mayoría de los sistemas de bases de datos, ya que no depende de funciones específicas.

---

### **Limitaciones**

1. **Escalabilidad:**
    - Esta técnica es práctica para un número pequeño de valores (por ejemplo, unos pocos meses).
    - Para un gran número de valores (por ejemplo, todos los días del año), la consulta se volvería larga y tediosa.

2. **Mantenimiento:**
    - Si necesitas agregar o eliminar columnas, debes modificar manualmente la consulta.

---

### **Alternativas Avanzadas: Pivote**

El texto menciona que algunos sistemas de bases de datos, como **SQL Server** y **Oracle**, incluyen cláusulas de **pivote** para realizar este tipo de transformaciones de manera más eficiente.

#### Ejemplo en SQL Server:
```sql
SELECT
    [May] AS May_rentals,
    [June] AS June_rentals,
    [July] AS July_rentals
FROM (
    SELECT MONTHNAME(rental_date) rental_month, rental_id
    FROM rental
    WHERE rental_date BETWEEN '2005-05-01' AND '2005-08-01'
) AS SourceTable
PIVOT (
    COUNT(rental_id)
    FOR rental_month IN ([May], [June], [July])
) AS PivotTable;
```

- **Explicación:**
    - La cláusula `PIVOT` convierte los valores de `rental_month` en columnas.
    - Es más eficiente y escalable que usar múltiples expresiones `CASE`.

---

### **Conclusión**

- **Transformación con `CASE` y `SUM`:**
    - Es una técnica útil para convertir filas en columnas en sistemas de bases de datos que no admiten operaciones de pivote.
    - Ideal para un número pequeño de valores.

- **Pivote:**
    - Es una alternativa más eficiente y escalable en sistemas que la admiten (como SQL Server y Oracle).

En resumen, la transformación del conjunto de resultados es una técnica poderosa para reorganizar datos en SQL. Aunque las expresiones `CASE` y `SUM` son una solución práctica y compatible, las operaciones de pivote ofrecen una alternativa más eficiente en sistemas que las soportan.
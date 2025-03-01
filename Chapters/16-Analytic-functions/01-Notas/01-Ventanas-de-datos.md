
## **Ventanas de Datos: Análisis Avanzado Sin Cambiar la Granularidad de los Resultados**

El texto introduce el concepto de **ventanas de datos** en el contexto de las **funciones analíticas** en SQL.  Las ventanas de datos son una funcionalidad muy poderosa que permite realizar cálculos sobre **conjuntos de filas relacionadas** dentro de un resultado de consulta, **sin afectar la granularidad de las filas individuales** en ese resultado.

**El Problema: Comparaciones Dentro de Grupos y Totales Globales**

El texto comienza con un ejemplo práctico: calcular las **ventas mensuales totales** de alquiler de películas para un período específico (mayo a agosto de 2005).  La consulta inicial que muestra el texto hace precisamente esto:

```sql
mysql> SELECT quarter(payment_date) quarter,
    ->  monthname(payment_date) month_nm,
    ->  sum(amount) monthly_sales
    -> FROM payment
    -> WHERE year(payment_date) = 2005
    -> GROUP BY quarter(payment_date), monthname(payment_date);
+---------+----------+---------------+
| quarter | month_nm | monthly_sales |
+---------+----------+---------------+
|       2 | May      |       4824.43 |
|       2 | June     |       9631.88 |
|       3 | July     |      28373.89 |
|       3 | August   |      24072.13 |
+---------+----------+---------------+
4 rows in set (0.13 sec)
```

**Análisis de la Consulta Inicial:**

*   **`SELECT quarter(payment_date) quarter, monthname(payment_date) month_nm, sum(amount) monthly_sales`**:  Selecciona el trimestre, el nombre del mes y la suma de la columna `amount` (que representa el monto del pago) como `monthly_sales`.
*   **`FROM payment`**:  Indica que la fuente de datos es la tabla `payment`.
*   **`WHERE year(payment_date) = 2005`**:  Filtra los pagos para incluir solo los del año 2005.
*   **`GROUP BY quarter(payment_date), monthname(payment_date)`**:  Agrupa los resultados por trimestre y nombre del mes, de modo que la función de agregación `sum(amount)` calcule las ventas totales para cada combinación de trimestre y mes.

**El Desafío: Añadir Valores Máximos para Comparación**

Después de obtener las ventas mensuales, surge la necesidad de **comparar** estos valores.  El texto plantea el desafío de determinar **programáticamente** el mes con las ventas más altas en general y dentro de cada trimestre.  Para lograr esto, **necesitamos agregar columnas adicionales** a cada fila que muestren:

*   **`max_overall_sales`**: El valor máximo de ventas mensuales **en todo el período de tiempo** (mayo a agosto de 2005 en este caso).
*   **`max_qrtr_sales`**: El valor máximo de ventas mensuales **dentro del mismo trimestre** para cada fila.

**Solución con Funciones Analíticas y Ventanas de Datos**

Para resolver este desafío, el texto introduce el uso de **funciones analíticas (o funciones de ventana)**.  La consulta se modifica para incluir dos nuevas columnas calculadas con funciones analíticas:

```sql
mysql> SELECT quarter(payment_date) quarter,
    ->  monthname(payment_date) month_nm,
    ->  sum(amount) monthly_sales,
    ->  max(sum(amount)) over () max_overall_sales,
    ->  max(sum(amount)) over (partition by quarter(payment_date)) max_qrtr_sales
    -> FROM payment
    -> WHERE year(payment_date) = 2005
    -> GROUP BY quarter(payment_date), monthname(payment_date);
+---------+----------+---------------+-------------------+----------------+
| quarter | month_nm | monthly_sales | max_overall_sales | max_qrtr_sales |
+---------+----------+---------------+-------------------+----------------+
|       2 | May      |       4824.43 |       28373.89 |        9631.88 |
|       2 | June     |       9631.88 |       28373.89 |        9631.88 |
|       3 | July     |      28373.89 |       28373.89 |       28373.89 |
|       3 | August   |      24072.13 |       28373.89 |       28373.89 |
+---------+----------+---------------+-------------------+----------------+
4 rows in set (0.09 sec)
```

**Análisis de la Consulta con Funciones Analíticas:**

Observa las dos nuevas columnas en la sentencia `SELECT`:

*   **`max(sum(amount)) over () max_overall_sales`**:  Esta expresión calcula el **valor máximo de `monthly_sales` en todo el conjunto de resultados**.
    *   `max(sum(amount))`:  Esta parte es similar a una función de agregación regular, que calcula el máximo de la suma de `amount`.
    *   **`over ()`**:  Esta es la **cláusula OVER**, que **convierte la función `max(sum(amount))` en una función analítica (o de ventana)**.
        *   **`()` dentro de `OVER` está vacío**:  Esto significa que **la ventana de datos** para esta función analítica **incluye todas las filas del resultado de la consulta**.  En otras palabras, la función `max()` se aplicará a **todo el conjunto de resultados** para encontrar el valor máximo global.
    *   `max_overall_sales`:  El resultado (el máximo global de ventas mensuales) se aliasa como `max_overall_sales`.  Este valor se **repite en cada fila** del resultado, ya que es el máximo global.

*   **`max(sum(amount)) over (partition by quarter(payment_date)) max_qrtr_sales`**:  Esta expresión calcula el **valor máximo de `monthly_sales` dentro de cada trimestre**.
    *   `max(sum(amount))`:  De nuevo, la función de agregación `max(sum(amount))`.
    *   **`over (partition by quarter(payment_date))`**:  La cláusula `OVER` con la sub-cláusula **`PARTITION BY quarter(payment_date)`** define la **ventana de datos**.
        *   **`partition by quarter(payment_date)`**:  Esto **divide el conjunto de resultados en particiones o "ventanas" basadas en el valor de `quarter(payment_date)`**.  En este caso, habrá una ventana para el trimestre 2 y otra ventana para el trimestre 3.
        *   La función `max()` se aplicará **dentro de cada ventana por separado**.  En la ventana del trimestre 2, calculará el máximo de `monthly_sales` solo para los meses de mayo y junio (que pertenecen al trimestre 2).  En la ventana del trimestre 3, calculará el máximo para julio y agosto (trimestre 3).
    *   `max_qrtr_sales`:  El resultado (el máximo de ventas mensuales por trimestre) se aliasa como `max_qrtr_sales`.  Este valor será **diferente para cada trimestre** y se repetirá para todos los meses dentro del mismo trimestre.

**El Poder de las Ventanas de Datos: Agrupamiento sin `GROUP BY` Secundario**

El punto crucial aquí es que **las funciones analíticas con ventanas de datos operan sobre un "conjunto de filas relacionadas" sin cambiar el conjunto de resultados original**.  A diferencia de la cláusula `GROUP BY` (que **colapsa** filas en grupos y reduce el número de filas en el resultado), las ventanas de datos **mantienen la granularidad original de las filas** pero permiten realizar cálculos agregados *en relación* con otros grupos de filas.

**En el ejemplo:**

*   La cláusula `GROUP BY quarter(payment_date), monthname(payment_date)` **agrupa las filas de la tabla `payment` a nivel mensual**, creando una fila por cada mes.
*   Las funciones analíticas con `OVER()` y `OVER(PARTITION BY quarter(payment_date))` **no cambian el número de filas** (seguimos teniendo una fila por mes).  Sin embargo, **añaden información calculada en diferentes "ventanas" o grupos de filas**:
    *   `max_overall_sales`  calcula el máximo **sobre todas las filas de la consulta**.
    *   `max_qrtr_sales` calcula el máximo **dentro de cada trimestre**.

**Definición de Ventanas con la Cláusula `OVER` y `PARTITION BY`**

El texto resume cómo se definen las ventanas de datos utilizando la cláusula `OVER` y su sub-cláusula `PARTITION BY`:

*   **`OVER()` (Cláusula OVER vacía):**  Define una ventana que **incluye todas las filas del conjunto de resultados**.  La función analítica se aplica a todo el conjunto de resultados como una única ventana.
*   **`OVER(PARTITION BY columna1, columna2, ...)` (Cláusula OVER con `PARTITION BY`):**  Define ventanas **dividiendo el conjunto de resultados en particiones o "ventanas" basadas en los valores de las columnas especificadas en `PARTITION BY`**.  La función analítica se aplica **por separado dentro de cada partición o ventana**.

**Tamaño y Flexibilidad de las Ventanas:**

El texto enfatiza que las ventanas de datos pueden variar en tamaño, **desde una sola fila hasta todas las filas del resultado**.  Además, **diferentes funciones analíticas en la misma consulta pueden definir diferentes ventanas de datos**, lo que permite realizar análisis muy sofisticados combinando diferentes agrupamientos y cálculos en un solo resultado.

**En Resumen, las Ventanas de Datos permiten:**

*   **Realizar cálculos agregados (como `MAX`, `MIN`, `AVG`, `SUM`, `COUNT`, etc.) sobre grupos de filas relacionadas**.
*   **Definir dinámicamente cómo se forman estos grupos (ventanas) utilizando la cláusula `OVER` y `PARTITION BY`**.
*   **Añadir información calculada a cada fila del resultado original sin cambiar el número de filas ni la granularidad de los datos**.
*   **Realizar análisis comparativos complejos dentro de grupos y con respecto a totales globales, todo en una sola consulta**.

Las ventanas de datos son una herramienta fundamental para el análisis avanzado de datos en SQL, permitiendo responder a preguntas complejas sobre tendencias, rankings, y comparaciones dentro de conjuntos de datos, de una manera eficiente y concisa.


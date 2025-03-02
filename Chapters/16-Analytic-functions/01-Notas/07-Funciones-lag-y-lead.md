
## **Funciones `LAG` y `LEAD`: Viajando en el Tiempo Dentro de tus Resultados 🕰️🚀**

Este texto nos presenta dos funciones analíticas muy útiles para **comparar filas entre sí dentro de un mismo conjunto de resultados**: `LAG` y `LEAD`.  Piensa en una lista ordenada de datos, como las ventas mensuales. A menudo, no solo te interesa el valor de ventas de un mes en particular, sino también **compararlo con el mes anterior o el mes siguiente**.  `LAG` y `LEAD` te permiten hacer justo eso, ¡como si pudieras "mirar hacia atrás" (`LAG`) o "mirar hacia adelante" (`LEAD`) en tu lista de resultados!

**¿Para qué sirven `LAG` y `LEAD`?**

*   **`LAG(columna, desplazamiento)` (Mirar hacia atrás):**  Te permite **acceder al valor de una columna en una fila *anterior* a la fila actual** dentro de tu conjunto de resultados ordenado. Es como mirar al pasado en tu secuencia de datos.
*   **`LEAD(columna, desplazamiento)` (Mirar hacia adelante):** Te permite **acceder al valor de una columna en una fila *posterior* a la fila actual** dentro de tu conjunto de resultados ordenado. Es como mirar al futuro en tu secuencia de datos.

**Sintaxis Básica: ¿Cómo se usan?**

Tanto `LAG` como `LEAD` son funciones analíticas, por lo que **necesitan la cláusula `OVER()`** para definir sobre qué conjunto de filas deben operar y en qué orden. La sintaxis básica es similar para ambas:

```sql
LAG(columna_a_mirar, desplazamiento) OVER (ORDER BY columna_de_orden)
LEAD(columna_a_mirar, desplazamiento) OVER (ORDER BY columna_de_orden)
```

*   **`columna_a_mirar`**:  Es la **columna de la que quieres obtener el valor de otra fila**.  Por ejemplo, si quieres comparar las ventas mensuales, esta sería la columna con el total de ventas.
*   **`desplazamiento` (opcional, por defecto es 1):**  Indica **cuántas filas hacia atrás (`LAG`) o hacia adelante (`LEAD`)** quieres "saltar" para obtener el valor.
    *   Si usas `1` (o no especificas nada, ya que es el valor por defecto), `LAG` te dará el valor de la fila **inmediatamente anterior**, y `LEAD` te dará el valor de la fila **inmediatamente siguiente**.
    *   Si usas `2`, `LAG` te dará el valor de la fila **dos posiciones antes**, y `LEAD` te dará el valor de la fila **dos posiciones después**, y así sucesivamente.
*   **`OVER (ORDER BY columna_de_orden)`**:  Define **el orden en el que se consideran las filas** para determinar cuál es la fila "anterior" o "siguiente".  Es **fundamental** especificar un `ORDER BY` dentro de `OVER()` para que `LAG` y `LEAD` tengan sentido. Por ejemplo, si estás trabajando con datos de ventas mensuales, ordenarías por la columna de fecha del mes.

**Ejemplo 1: `LAG` y `LEAD` Juntos - Semana Actual, Semana Anterior y Semana Siguiente 🗓️➡️⬅️**

El texto nos presenta un primer ejemplo muy ilustrativo que usa tanto `LAG` como `LEAD` en la misma consulta:

```sql
mysql> SELECT yearweek(payment_date) payment_week,
    ->  sum(amount) week_total,
    ->  lag(sum(amount), 1)
    ->    OVER (ORDER BY yearweek(payment_date)) prev_wk_tot,
    ->  lead(sum(amount), 1)
    ->    OVER (ORDER BY yearweek(payment_date)) next_wk_tot
    -> FROM payment
    -> GROUP BY yearweek(payment_date)
    -> ORDER BY 1;
```

**Desglose de esta consulta:**

*   **`SELECT yearweek(payment_date) payment_week, sum(amount) week_total, lag(sum(amount), 1) OVER (ORDER BY yearweek(payment_date)) prev_wk_tot, lead(sum(amount), 1) OVER (ORDER BY yearweek(payment_date)) next_wk_tot`**:  Seleccionamos cuatro columnas:
    *   `yearweek(payment_date) payment_week`: La semana del año de pago.
    *   `sum(amount) week_total`: El total de pagos para esa semana.
    *   `lag(sum(amount), 1) OVER (ORDER BY yearweek(payment_date)) prev_wk_tot`: **Función `LAG`**:
        *   `lag(sum(amount), 1)`:  Obtenemos el valor de `sum(amount)` (total de pagos semanales). El `1` indica que queremos el valor de la fila **inmediatamente anterior**.
        *   `OVER (ORDER BY yearweek(payment_date))`:  Definimos que el orden para determinar la fila "anterior" es por `yearweek(payment_date)` (la semana del año).
        *   `prev_wk_tot`:  Le damos el alias `prev_wk_tot` (Total Semana Anterior) a esta columna resultante.
    *   `lead(sum(amount), 1) OVER (ORDER BY yearweek(payment_date)) next_wk_tot`: **Función `LEAD`**:
        *   `lead(sum(amount), 1)`: Obtenemos el valor de `sum(amount)`. El `1` indica que queremos el valor de la fila **inmediatamente siguiente**.
        *   `OVER (ORDER BY yearweek(payment_date))`:  Mismo orden que para `LAG`, por semana del año.
        *   `next_wk_tot`: Alias `next_wk_tot` (Total Semana Siguiente).
*   **`FROM payment`**:  La tabla de origen es `payment`.
*   **`GROUP BY yearweek(payment_date)`**:  Agrupamos por semana para obtener los totales semanales.
*   **`ORDER BY 1`**: Ordenamos el resultado final por la primera columna, `payment_week`, para que las semanas aparezcan en orden cronológico.

**Resultados Simplificados del ejemplo con `LAG` y `LEAD`:**

La consulta nos da un resultado como este (simplificado):

```plaintext
+--------------+------------+-------------+-------------+
| payment_week | week_total | prev_wk_tot | next_wk_tot |
+--------------+------------+-------------+-------------+
|       200521 |    2847.18 |        NULL |     1977.25 |  <- Semana 200521: Total, Semana Anterior (NULL), Semana Siguiente
|       200522 |    1977.25 |     2847.18 |     5605.42 |  <- Semana 200522: Total, Semana Anterior (Semana 200521), Semana Siguiente (Semana 200524)
|       200524 |    5605.42 |     1977.25 |     4026.46 |  <- Semana 200524: Total, Semana Anterior (Semana 200522), Semana Siguiente (Semana 200525)
|       200525 |    4026.46 |     5605.42 |     8490.83 |  <- Semana 200525: Total, Semana Anterior (Semana 200524), Semana Siguiente (Semana 200527)
| ...          | ...        | ...         | ...         |
|       200607 |     514.18 |     7909.16 |        NULL |  <- Semana 200607: Total, Semana Anterior (Semana 200534), Semana Siguiente (NULL)
+--------------+------------+-------------+-------------+
```

**Interpretación de las columnas `prev_wk_tot` y `next_wk_tot`:**

*   **`prev_wk_tot` (Semana Anterior):** Para cada semana, esta columna muestra el `week_total` de la **semana *anterior*** en el orden cronológico.
    *   Para la **primera semana (200521)**, como no hay semana anterior, el valor de `prev_wk_tot` es **`NULL`** (valor nulo, que indica "no hay valor").
    *   Para la **segunda semana (200522)**, `prev_wk_tot` muestra el `week_total` de la semana **200521** (la semana anterior).
    *   Y así sucesivamente.

*   **`next_wk_tot` (Semana Siguiente):**  Similarmente, para cada semana, esta columna muestra el `week_total` de la **semana *siguiente***.
    *   Para la **última semana (200607)**, como no hay semana siguiente, el valor de `next_wk_tot` es **`NULL`**.
    *   Para la **penúltima semana (200534)**, `next_wk_tot` muestra el `week_total` de la semana **200607** (la semana siguiente).
    *   Y así sucesivamente.

¡Ahora, en cada fila, tienes el total semanal actual, el total de la semana anterior y el total de la semana siguiente, todo junto para comparar!

**Parámetro Opcional de Desplazamiento: ¡Saltando Varias Filas!**

Tanto `LAG` como `LEAD` permiten un **segundo parámetro opcional**, el **desplazamiento** (que por defecto es 1).  Si quieres mirar más allá de la fila inmediatamente anterior o siguiente, puedes usar este parámetro. Por ejemplo:

*   `LAG(columna, 2) OVER (ORDER BY ...)`:  Obtiene el valor de la columna de **dos filas antes**.
*   `LEAD(columna, 3) OVER (ORDER BY ...)`:  Obtiene el valor de la columna de **tres filas después**.

**Ejemplo 2: Cálculo del Porcentaje de Diferencia con la Semana Anterior (`LAG` para Comparación Temporal) 📈 %**

Un uso muy común de `LAG` es calcular el **porcentaje de diferencia con respecto al período anterior**, como comparar las ventas de este mes con las del mes pasado, o como en este ejemplo, comparar las ventas de esta semana con las de la semana anterior.  La consulta SQL para calcular el porcentaje de diferencia semanal usando `LAG` es:

```sql
mysql> SELECT yearweek(payment_date) payment_week,
    ->  sum(amount) week_total,
    ->  round((sum(amount) - lag(sum(amount), 1)
    ->    OVER (ORDER BY yearweek(payment_date)))
    ->    / lag(sum(amount), 1)
    ->    OVER (ORDER BY yearweek(payment_date)) * 100, 1) pct_diff
    -> FROM payment
    -> GROUP BY yearweek(payment_date)
    -> ORDER BY 1;
```

**Desglose de la consulta para porcentaje de diferencia semanal:**

*   **`SELECT yearweek(payment_date) payment_week, sum(amount) week_total, round((sum(amount) - lag(sum(amount), 1) OVER (ORDER BY yearweek(payment_date))) / lag(sum(amount), 1) OVER (ORDER BY yearweek(payment_date)) * 100, 1) pct_diff`**:  Seleccionamos:
    *   `yearweek(payment_date) payment_week`, `sum(amount) week_total`:  Semana y total semanal, igual que antes.
    *   `round((sum(amount) - lag(sum(amount), 1) OVER (ORDER BY yearweek(payment_date))) / lag(sum(amount), 1) OVER (ORDER BY yearweek(payment_date)) * 100, 1) pct_diff`: **Cálculo del porcentaje de diferencia**:
        *   `(sum(amount) - lag(sum(amount), 1) OVER (ORDER BY yearweek(payment_date)))`:  Calculamos la **diferencia** entre el `week_total` de la semana actual (`sum(amount)`) y el `week_total` de la semana anterior (`lag(sum(amount), 1) OVER (ORDER BY yearweek(payment_date))`).  Es decir, `Semana Actual - Semana Anterior`.
        *   `/ lag(sum(amount), 1) OVER (ORDER BY yearweek(payment_date))`:  **Dividimos** la diferencia por el `week_total` de la semana anterior (`lag(...)`).  Es decir, `Diferencia / Semana Anterior`.
        *   `* 100`:  Multiplicamos por 100 para expresar el resultado como **porcentaje**.
        *   `round( ..., 1)`:  Redondeamos el porcentaje a **1 decimal** para una mejor presentación.
        *   `pct_diff`:  Le damos el alias `pct_diff` (Porcentaje de Diferencia) a esta columna.
*   **`FROM payment`**: Tabla de pagos.
*   **`GROUP BY yearweek(payment_date)`**: Agrupamos por semana.
*   **`ORDER BY 1`**: Ordenamos por semana.

**Resultados Simplificados del ejemplo de porcentaje de diferencia semanal:**

La consulta de porcentaje de diferencia nos da algo como esto:

```plaintext
+--------------+------------+----------+
| payment_week | week_total | pct_diff |
+--------------+------------+----------+
|       200521 |    2847.18 |     NULL |  <- Semana 200521: Total, Porcentaje de diferencia (NULL, no hay semana anterior)
|       200522 |    1977.25 |    -30.6 |  <- Semana 200522: Total, Porcentaje de diferencia con semana anterior (-30.6%)
|       200524 |    5605.42 |    183.5 |  <- Semana 200524: Total, Porcentaje de diferencia con semana anterior (+183.5%)
|       200525 |    4026.46 |    -28.2 |  <- Semana 200525: Total, Porcentaje de diferencia con semana anterior (-28.2%)
|       200527 |    8490.83 |    110.9 |  <- Semana 200527: Total, Porcentaje de diferencia con semana anterior (+110.9%)
|       200528 |    5983.63 |    -29.5 |  <- Semana 200528: Total, Porcentaje de diferencia con semana anterior (-29.5%)
|       200530 |   11031.22 |     84.4 |  <- Semana 200530: Total, Porcentaje de diferencia con semana anterior (+84.4%)
|       200531 |    8412.07 |    -23.7 |  <- Semana 200531: Total, Porcentaje de diferencia con semana anterior (-23.7%)
|       200533 |   10619.11 |     26.2 |  <- Semana 200533: Total, Porcentaje de diferencia con semana anterior (+26.2%)
|       200534 |    7909.16 |    -25.5 |  <- Semana 200534: Total, Porcentaje de diferencia con semana anterior (-25.5%)
|       200607 |     514.18 |    -93.5 |  <- Semana 200607: Total, Porcentaje de diferencia con semana anterior (-93.5%)
+--------------+------------+----------+
```

**Interpretación de la columna `pct_diff` (Porcentaje de Diferencia):**

*   **`pct_diff`**:  Muestra el **porcentaje de cambio en las ventas semanales en comparación con la semana anterior**.
    *   Para la **primera semana (200521)**, el valor es **`NULL`** porque no hay una semana anterior para comparar.
    *   Para la **segunda semana (200522)**, el porcentaje es **-30.6%**. Esto significa que las ventas de la semana 200522 fueron **30.6% menores** que las ventas de la semana 200521.
    *   Para la **tercera semana (200524)**, el porcentaje es **183.5%**. Esto indica que las ventas de la semana 200524 fueron **183.5% mayores** que las ventas de la semana 200522 (¡un gran aumento!).
    *   Y así sucesivamente, para cada semana, `pct_diff` muestra el cambio porcentual con respecto a la semana inmediatamente anterior.

**Resumen: `LAG` y `LEAD` - Poderosas Herramientas de Comparación Temporal**

En resumen, `LAG` y `LEAD` son funciones analíticas esenciales para:

*   **Comparar valores entre filas** dentro de un conjunto de resultados ordenado.
*   Analizar **tendencias a lo largo del tiempo**, calculando diferencias o cambios porcentuales con respecto a períodos anteriores (`LAG`) o siguientes (`LEAD`).
*   Crear **informes más informativos** que no solo muestran valores absolutos, sino también cómo esos valores se relacionan con datos en otros puntos de la secuencia.


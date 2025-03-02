
## **Ventanas de Datos: Más Allá de `PARTITION BY`, ¡Control Total con "Frames"!**

Ya hemos visto cómo la cláusula `PARTITION BY` es como dividir tus datos en cajones separados para aplicar funciones analíticas por grupo. Pero a veces, necesitas ser aún **más preciso** sobre **qué filas exactamente** quieres incluir en tu "ventana" de datos.  Imagina que no solo quieres analizar por "cajón" (partición), sino también **definir un rango *dentro* de cada cajón**, o incluso sobre **todo el conjunto de resultados ordenado**. ¡Aquí es donde entran en juego los **"frames"**!

**¿Qué son los "Frames" y por qué son tan útiles?**

Un **"frame"** es como una **"sub-ventana"** dentro de tu ventana de datos principal (definida por `PARTITION BY`, si la hay). Los frames te permiten especificar **con exactitud qué filas vecinas a la fila actual** quieres incluir en el cálculo de tu función analítica.  Esto es súper poderoso para hacer cálculos como:

*   **Sumas acumulativas**:  Calcular un total que se va acumulando fila por fila, como un saldo que crece con cada transacción.
*   **Promedios móviles**:  Calcular el promedio de un valor considerando un "vecindario" de filas alrededor de la fila actual, como suavizar tendencias en datos con fluctuaciones.
*   **Comparaciones con filas previas o siguientes**:  Analizar cómo un valor actual se relaciona con valores en filas cercanas en el orden.

**Ejemplo 1: Suma Acumulativa Semanal de Pagos (`ROWS UNBOUNDED PRECEDING`) 📈**

El texto nos presenta un primer ejemplo para entender los frames: calcular una **suma acumulativa de los pagos semanales**.  Imagina que quieres ver cómo las ventas totales se acumulan semana tras semana a lo largo del tiempo. La consulta SQL es:

```sql
mysql> SELECT yearweek(payment_date) payment_week,
    ->  sum(amount) week_total,
    ->  sum(sum(amount))
    ->    over (order by yearweek(payment_date)
    ->      rows unbounded preceding) rolling_sum
    -> FROM payment
    -> GROUP BY yearweek(payment_date)
    -> ORDER BY 1;
```

**Desglose de la consulta de suma acumulativa semanal:**

*   **`SELECT yearweek(payment_date) payment_week, sum(amount) week_total, sum(sum(amount)) over (order by yearweek(payment_date) rows unbounded preceding) rolling_sum`**:  Seleccionamos:
    *   `yearweek(payment_date) payment_week`:  Extraemos el año y la semana del año de la fecha de pago usando `yearweek()`.  Esto nos sirve para agrupar por semana. Lo llamamos `payment_week`.
    *   `sum(amount) week_total`: Calculamos la suma de los montos de pago (`amount`) para cada semana (usando `GROUP BY yearweek(payment_date)`).  Este es el total de pagos **por semana**. Lo llamamos `week_total`.
    *   `sum(sum(amount)) over (order by yearweek(payment_date) rows unbounded preceding) rolling_sum`: **¡Aquí está el frame!**  Esta es la función analítica que calcula la **suma acumulativa**.
        *   `sum(sum(amount))`:  Calculamos la suma de los totales semanales (`sum(amount)`, que es `week_total`).  Usamos `sum(sum(amount))` porque la columna que estamos "acumulando" ya es una suma (el total semanal).
        *   `over (order by yearweek(payment_date) rows unbounded preceding)`:  La cláusula `OVER()` define la ventana y, dentro, el **frame**:
            *   `order by yearweek(payment_date)`:  **¡Importante! Para usar frames, *siempre* necesitas un `ORDER BY` dentro de `OVER()`**.  Aquí, ordenamos las filas **por semana del año** (`yearweek(payment_date)`).  El orden es crucial para que la suma sea *acumulativa* a lo largo del tiempo.
            *   `rows unbounded preceding`:  **¡Esta es la definición del frame!**  `ROWS` indica que vamos a definir el frame **en términos de filas** (no rangos de valores). `UNBOUNDED PRECEDING` significa que el frame **empieza desde la *primera fila* de la partición (o de todo el conjunto de resultados si no hay `PARTITION BY`) y *se extiende hasta la fila actual***.

*   **`FROM payment`**:  Tabla de pagos.
*   **`GROUP BY yearweek(payment_date)`**: Agrupamos por semana para calcular los totales semanales.
*   **`ORDER BY 1`**:  Ordenamos el resultado final por la primera columna, `payment_week`, para que las semanas aparezcan en orden cronológico.

**Resultados Simplificados de la suma acumulativa semanal:**

La consulta nos da un resultado como este (simplificado):

```plaintext
+--------------+------------+-------------+
| payment_week | week_total | rolling_sum |
+--------------+------------+-------------+
|       200521 |    2847.18 |     2847.18 |  <- Semana 21: Total semanal, Suma acumulativa (igual al total semanal, porque es la primera semana)
|       200522 |    1977.25 |     4824.43 |  <- Semana 22: Total semanal, Suma acumulativa (total semana 21 + total semana 22)
|       200524 |    5605.42 |    10429.85 |  <- Semana 24: Total semanal, Suma acumulativa (total semana 21 + 22 + 24)
|       200525 |    4026.46 |    14456.31 |  <- Semana 25: Total semanal, Suma acumulativa (total semanas 21 a 25)
| ...          | ...        | ...         |
|       200607 |     514.18 |    67416.51 |  <- Última Semana: Total semanal, Suma acumulativa (¡total de *todas* las semanas!)
+--------------+------------+-------------+
```

**Interpretación de la columna `rolling_sum` (Suma Acumulativa):**

*   **`rolling_sum`**: Esta columna muestra la **suma acumulativa de los totales semanales**.
    *   Para la **primera semana (200521)**, la `rolling_sum` es igual al `week_total` de esa semana (porque no hay semanas *precedentes* en el frame definido por `ROWS UNBOUNDED PRECEDING` para la primera fila).
    *   Para la **segunda semana (200522)**, la `rolling_sum` es la suma del `week_total` de la semana 22 **más** el `week_total` de la semana 21 (la semana *precedente*).
    *   Y así sucesivamente. Para cada semana, la `rolling_sum` es la **suma de los `week_total` de esa semana y de *todas* las semanas anteriores** en el orden cronológico definido por `ORDER BY yearweek(payment_date)`.
    *   En la **última fila**, la `rolling_sum` representa la **suma total de *todas* las semanas**, es la suma acumulativa final.

**Ejemplo 2: Promedio Móvil de 3 Semanas (`ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING`) 📉**

Ahora, veamos otro tipo de frame: el **promedio móvil de 3 semanas**.  Imagina que quieres suavizar las fluctuaciones semanales y ver la tendencia general de las ventas promediando cada semana con sus semanas vecinas. La consulta SQL es:

```sql
mysql> SELECT yearweek(payment_date) payment_week,
    ->  sum(amount) week_total,
    ->  avg(sum(amount))
    ->    over (order by yearweek(payment_date)
    ->      rows between 1 preceding and 1 following) rolling_3wk_avg
    -> FROM payment
    -> GROUP BY yearweek(payment_date)
    -> ORDER BY 1;
```

**Desglose de la consulta de promedio móvil de 3 semanas:**

*   **`SELECT yearweek(payment_date) payment_week, sum(amount) week_total, avg(sum(amount)) over (order by yearweek(payment_date) rows between 1 preceding and 1 following) rolling_3wk_avg`**: Seleccionamos:
    *   `yearweek(payment_date) payment_week`, `sum(amount) week_total`:  Igual que en el ejemplo anterior, la semana y el total semanal.
    *   `avg(sum(amount)) over (order by yearweek(payment_date) rows between 1 preceding and 1 following) rolling_3wk_avg`:  **¡El frame para el promedio móvil!**
        *   `avg(sum(amount))`:  Calculamos el **promedio** de los totales semanales (`sum(amount)`, que es `week_total`).
        *   `over (order by yearweek(payment_date) rows between 1 preceding and 1 following)`:  La cláusula `OVER()` con el frame:
            *   `order by yearweek(payment_date)`: De nuevo, ordenamos por semana para el orden cronológico.
            *   `rows between 1 preceding and 1 following`: **¡Esta es la definición del frame para el promedio móvil!** `ROWS BETWEEN` indica que definimos un **rango de filas *relativo* a la fila actual**.
                *   `1 preceding`: Incluimos **1 fila *antes* de la fila actual** (en el orden definido por `ORDER BY`).
                *   `and 1 following`: Incluimos **1 fila *después* de la fila actual**.
                *   `rows between ... and ...`:  Por lo tanto, para cada fila, el frame incluye **la fila actual, la fila inmediatamente anterior y la fila inmediatamente siguiente** en el orden por semana.  ¡Un "vecindario" de 3 semanas!

**Resultados Simplificados del promedio móvil de 3 semanas:**

La consulta nos da un resultado como este:

```plaintext
+--------------+------------+-----------------+
| payment_week | week_total | rolling_3wk_avg |
+--------------+------------+-----------------+
|       200521 |    2847.18 |     2412.215000 |  <- Semana 21: Total semanal, Promedio móvil (promedio entre semana 21 y 22, solo 2 semanas en el frame para la 1ra fila)
|       200522 |    1977.25 |     3476.616667 |  <- Semana 22: Total semanal, Promedio móvil (promedio entre semanas 21, 22 y 24, 3 semanas en el frame para las filas intermedias)
|       200524 |    5605.42 |     3869.710000 |  <- Semana 24: Total semanal, Promedio móvil (promedio entre semanas 22, 24 y 25)
|       200525 |    4026.46 |     6040.903333 |  <- Semana 25: Total semanal, Promedio móvil (promedio entre semanas 24, 25 y 27)
|       200527 |    8490.83 |     6166.973333 |  <- Semana 27: Total semanal, Promedio móvil (promedio entre semanas 25, 27 y 28)
|       200528 |    5983.63 |     8501.893333 |  <- Semana 28: Total semanal, Promedio móvil (promedio entre semanas 27, 28 y 30)
|       200530 |   11031.22 |     8475.640000 |  <- Semana 30: Total semanal, Promedio móvil (promedio entre semanas 28, 30 y 31)
|       200531 |    8412.07 |    10020.800000 | <- Semana 31: Total semanal, Promedio móvil (promedio entre semanas 30, 31 y 33)
|       200533 |   10619.11 |     8980.113333 | <- Semana 33: Total semanal, Promedio móvil (promedio entre semanas 31, 33 y 34)
|       200534 |    7909.16 |     6347.483333 | <- Semana 34: Total semanal, Promedio móvil (promedio entre semanas 33, 34 y 200607)
|       200607 |     514.18 |     4211.670000 | <- Semana 200607: Total semanal, Promedio móvil (promedio entre semana 34 y 200607, solo 2 semanas en el frame para la última fila)
+--------------+------------+-----------------+
```

**Interpretación de la columna `rolling_3wk_avg` (Promedio Móvil de 3 Semanas):**

*   **`rolling_3wk_avg`**: Esta columna muestra el **promedio móvil de 3 semanas de los totales semanales**.
    *   Para cada semana **intermedia**, el promedio se calcula con los `week_total` de la **semana actual, la semana anterior y la semana siguiente**.  Por ejemplo, para la semana 200522, el promedio se calcula usando los `week_total` de las semanas 200521, 200522, y 200524.
    *   **Casos especiales: primera y última fila**: Para la **primera semana (200521)**, como no hay una semana *precedente*, el promedio se calcula **solo con las semanas 200521 y 200522** (2 semanas en el frame en lugar de 3).  Similarmente, para la **última semana (200607)**, como no hay semana *siguiente*, el promedio se calcula **solo con las semanas 200534 y 200607**.  Los frames se "ajustan" en los bordes del conjunto de resultados.

**Ejemplo 3: Promedio de 7 Días con Rango de Fechas (`RANGE BETWEEN INTERVAL 3 DAY PRECEDING AND INTERVAL 3 DAY FOLLOWING`) 🗓️**

Finalmente, el texto nos muestra frames basados en **rangos de valores**, ¡ideal para trabajar con fechas!  Imagina que quieres calcular el **promedio de pagos en un rango de 7 días** alrededor de cada día. Esto es útil si tienes datos con **brechas** (días sin transacciones) y quieres un promedio basado en un **intervalo de tiempo fijo**, no solo en un número fijo de filas. La consulta SQL es:

```sql
mysql> SELECT date(payment_date), sum(amount),
    ->  avg(sum(amount)) over (order by date(payment_date)
    ->    range between interval 3 day preceding
    ->      and interval 3 day following) 7_day_avg
    -> FROM payment
    -> WHERE payment_date BETWEEN '2005-07-01' AND '2005-09-01'
    -> GROUP BY date(payment_date)
    -> ORDER BY 1;
```

**Desglose de la consulta de promedio de 7 días con rango de fechas:**

*   **`SELECT date(payment_date), sum(amount), avg(sum(amount)) over (order by date(payment_date) range between interval 3 day preceding and interval 3 day following) 7_day_avg`**:  Seleccionamos:
    *   `date(payment_date)`:  Extraemos solo la fecha de la fecha y hora de pago usando `date()`. Para agrupar por día.
    *   `sum(amount)`:  La suma de pagos para cada día (agrupado por `date(payment_date)`).
    *   `avg(sum(amount)) over (order by date(payment_date) range between interval 3 day preceding and interval 3 day following) 7_day_avg`: **¡El frame basado en rango de fechas!**
        *   `avg(sum(amount))`: Promedio de los totales diarios.
        *   `over (order by date(payment_date) range between interval 3 day preceding and interval 3 day following)`: La cláusula `OVER()` con frame:
            *   `order by date(payment_date)`: Ordenamos por fecha de pago para el orden cronológico.
            *   `range between interval 3 day preceding and interval 3 day following`:  **¡Definición del frame por rango!** `RANGE BETWEEN` indica que el frame se define por un **rango de *valores* de la columna de ordenamiento** (en este caso, la fecha).
                *   `interval 3 day preceding`:  Incluimos todas las filas cuya `date(payment_date)` esté **dentro de un intervalo de 3 días *antes*** de la `date(payment_date)` de la fila actual.
                *   `and interval 3 day following`: Incluimos también las filas con `date(payment_date)` **dentro de un intervalo de 3 días *después*** de la fecha de la fila actual.
                *   `range between ... and ...`:  Por lo tanto, para cada día, el frame incluye todas las filas de pagos que ocurrieron **en ese día, y dentro de los 3 días anteriores y 3 días posteriores** a ese día.  ¡Un rango de 7 días centrado en cada día!

*   **`FROM payment`**:  Tabla de pagos.
*   **`WHERE payment_date BETWEEN '2005-07-01' AND '2005-09-01'`**:  **Filtramos** los pagos para que sean solo dentro del rango de fechas de julio y agosto de 2005, para enfocarnos en un período de tiempo específico.
*   **`GROUP BY date(payment_date)`**: Agrupamos por día para obtener los totales diarios.
*   **`ORDER BY 1`**:  Ordenamos el resultado final por la primera columna, `date(payment_date)`, para que las fechas aparezcan en orden cronológico.

**Resultados Simplificados del promedio de 7 días por rango de fechas:**

La consulta nos da un resultado como este:

```plaintext
+--------------------+-------------+-------------+
| date(payment_date) | sum(amount) | 7_day_avg   |
+--------------------+-------------+-------------+
| 2005-07-05         |      128.73 | 1603.740000 |  <- 05-Jul-2005: Total diario, Promedio 7-días (promedio de pagos entre 02-Jul y 08-Jul)
| 2005-07-06         |     2131.96 | 1698.166000 |  <- 06-Jul-2005: Total diario, Promedio 7-días (promedio de pagos entre 03-Jul y 09-Jul)
| 2005-07-07         |     1943.39 | 1738.338333 |  <- 07-Jul-2005: Total diario, Promedio 7-días (promedio de pagos entre 04-Jul y 10-Jul)
| 2005-07-08         |     2210.88 | 1766.917143 |  <- 08-Jul-2005: Total diario, Promedio 7-días (promedio de pagos entre 05-Jul y 11-Jul)
| 2005-07-09         |     2075.87 | 2049.390000 |  <- 09-Jul-2005: Total diario, Promedio 7-días (promedio de pagos entre 06-Jul y 12-Jul)
| 2005-07-10         |     1939.20 | 2035.628333 |  <- 10-Jul-2005: Total diario, Promedio 7-días (promedio de pagos entre 07-Jul y 13-Jul)
| 2005-07-11         |     1938.39 | 2054.076000 |  <- 11-Jul-2005: Total diario, Promedio 7-días (promedio de pagos entre 08-Jul y 14-Jul)
| ...                 | ...         | ...         |
| 2005-08-16         |      111.77 | 1973.837500 |  <- 16-Ago-2005: Total diario, Promedio 7-días (promedio de pagos entre 13-Ago y 19-Ago, ¡nota la brecha de datos!)
| ...                 | ...         | ...         |
+--------------------+-------------+-------------+
```

**Interpretación de la columna `7_day_avg` (Promedio de 7 Días por Rango de Fechas):**

*   **`7_day_avg`**:  Esta columna muestra el **promedio de pagos en un rango de 7 días centrado en cada fecha**.
    *   Para cada **fecha en el resultado**, el promedio se calcula **considerando todas las filas de pagos cuya `payment_date` cae dentro del rango de 7 días** (3 días antes, el día actual, y 3 días después) de esa fecha.
    *   **Manejo de brechas de datos**: ¡Fíjate en la fecha 2005-08-16! El texto menciona que para el cálculo de `7_day_avg` para el 16/08, solo se incluyen las fechas 16/08, 17/08, 18/08, y 19/08, **¡porque no hay filas para los 3 días anteriores (13/08 al 15/08) en los datos!** `RANGE BETWEEN` se basa en los **valores de las fechas**, no en la presencia de filas contiguas.  Si hay "huecos" en tus fechas, el frame ajustará automáticamente para incluir solo las fechas existentes dentro del rango especificado.

**Tipos de Frames en SQL: `ROWS` vs. `RANGE`**

El texto nos muestra dos tipos principales de frames:

1.  **`ROWS` Frames**:  Definen el frame en términos de un **número de filas** relativas a la fila actual (por ejemplo, `ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING` - un frame de 3 filas). Son útiles cuando te importa el **número de filas vecinas** en el orden definido por `ORDER BY`, independientemente de los valores en la columna de ordenamiento.
2.  **`RANGE` Frames**:  Definen el frame en términos de un **rango de *valores*** de la columna especificada en `ORDER BY` (por ejemplo, `RANGE BETWEEN INTERVAL 3 DAY PRECEDING AND INTERVAL 3 DAY FOLLOWING` - un frame de 7 días).  Son especialmente útiles cuando trabajas con **fechas o valores numéricos continuos** y quieres definir el frame basado en un **intervalo de valores** alrededor del valor actual, incluso si hay "huecos" en los datos.

**Opciones Comunes para Frames:**

*   **`UNBOUNDED PRECEDING`**:  Desde el principio de la partición (o del conjunto de resultados) **hasta la fila actual**. (Para sumas acumulativas, etc.)
*   **`UNBOUNDED FOLLOWING`**:  Desde la fila actual **hasta el final** de la partición (o del conjunto de resultados).
*   **`CURRENT ROW`**:  Solo **la fila actual**.
*   **`n PRECEDING`**:  `n` filas **antes** de la fila actual.
*   **`n FOLLOWING`**:  `n` filas **después** de la fila actual.
*   **`BETWEEN ... AND ...`**:  Define un **rango** entre dos límites, que pueden ser combinaciones de `UNBOUNDED PRECEDING`, `UNBOUNDED FOLLOWING`, `CURRENT ROW`, `n PRECEDING`, `n FOLLOWING`.

**En Resumen: Frames - Control Avanzado de Ventanas de Datos**

Con los **"frames"**, has dado un gran paso en el dominio de las funciones analíticas de SQL.  Ahora puedes:

*   Definir **ventanas de datos muy precisas**, especificando **exactamente qué filas** quieres incluir en tus cálculos analíticos.
*   Crear **sumas acumulativas**, **promedios móviles**, y otros **cálculos avanzados** que dependen del orden de las filas y de "vecindarios" de datos.
*   Usar frames basados en **número de filas (`ROWS`)** o en **rangos de valores (`RANGE`)**, adaptándote a diferentes tipos de análisis (series temporales, datos continuos, etc.).
*   Manejar **datos con brechas** usando `RANGE` frames, que se basan en intervalos de valores y no solo en filas contiguas.


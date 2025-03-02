
## **Funciones de Informes: Más Allá de la Clasificación, ¡Informes Poderosos!**

Este texto nos introduce a otra faceta fantástica de las **funciones analíticas**: usarlas para generar **informes** muy útiles.  No solo sirven para clasificar (como vimos con `RANK()`), sino también para **encontrar valores clave**, calcular **totales y promedios**, ¡y mucho más! La clave está en usar **funciones de agregación** (`MIN()`, `MAX()`, `AVG()`, `SUM()`, `COUNT()`) **junto con la cláusula `OVER()`**.

**Ejemplo 1: Totales Mensuales y Globales de Pagos Grandes 💰📊**

El texto empieza con un ejemplo muy práctico: queremos saber los **totales de pagos mensuales** y el **total global** de **todos los pagos que sean de $10 o más**. La consulta SQL para esto es:

```sql
mysql> SELECT monthname(payment_date) payment_month,
    ->  amount,
    ->  sum(amount) over (partition by monthname(payment_date)) monthly_total,
    ->  sum(amount) over () grand_total
    -> FROM payment
    -> WHERE amount >= 10
    -> ORDER BY 1;
```

**Desglose de la consulta para totales mensuales y globales:**

*   **`SELECT monthname(payment_date) payment_month, amount, sum(amount) over (partition by monthname(payment_date)) monthly_total, sum(amount) over () grand_total`**:  Seleccionamos varias columnas:
    *   `monthname(payment_date) payment_month`: El nombre del mes del pago, para saber a qué mes corresponde cada fila. Lo llamamos `payment_month`.
    *   `amount`:  El monto de cada pago individual.
    *   `sum(amount) over (partition by monthname(payment_date)) monthly_total`: **¡Aquí está la magia!** Esta es una función analítica que calcula la **suma de `amount` (montos de pago)**.
        *   **`sum(amount)`**: La función de agregación `SUM()` que ya conoces, para sumar valores.
        *   **`over (partition by monthname(payment_date))`**: La cláusula `OVER()` con `PARTITION BY monthname(payment_date)` define la **ventana de datos**.  Divide los pagos en **grupos o "ventanas" por mes**.  La función `SUM(amount)` se calculará **por separado dentro de cada ventana mensual**.  El resultado será el **total de pagos para *cada mes***. Lo llamamos `monthly_total`.
    *   `sum(amount) over () grand_total`: **¡Otra función analítica clave!**  También calcula la **suma de `amount`**.
        *   **`sum(amount)`**: De nuevo, la función de agregación `SUM()`.
        *   **`over ()`**:  La cláusula `OVER()` **sin nada dentro**. Esto significa que la ventana de datos es **todo el conjunto de resultados**.  La función `SUM(amount)` se calculará **sobre *todas* las filas**.  El resultado será la **suma total de todos los pagos** que cumplen la condición `WHERE`. Lo llamamos `grand_total`.
*   **`FROM payment`**:  La tabla de origen es `payment` (tabla de pagos).
*   **`WHERE amount >= 10`**:  **Filtramos** los pagos para incluir **solo aquellos cuyo monto sea de $10 o más**.
*   **`ORDER BY 1`**:  Ordenamos el resultado por la primera columna, `payment_month` (el nombre del mes), para que los meses aparezcan en orden alfabético.

**Resultados Simplificados del ejemplo de totales:**

La consulta nos da un resultado como este (simplificado):

```plaintext
+---------------+--------+---------------+-------------+
| payment_month | amount | monthly_total | grand_total |
+---------------+--------+---------------+-------------+
| August        |  10.99 |        521.53 |     1262.86 |  <- Agosto, pago de $10.99, total de Agosto, total global
| August        |  11.99 |        521.53 |     1262.86 |  <- Agosto, otro pago, mismo total de Agosto y total global
| July          |  10.99 |        519.53 |     1262.86 |  <- Julio, pago, total de Julio, total global
| June          |  10.99 |        165.85 |     1262.86 |  <- Junio, pago, total de Junio, total global
| May           |  10.99 |         55.95 |     1262.86 |  <- Mayo, pago, total de Mayo, total global
+---------------+--------+---------------+-------------+
```

**Interpretación de las columnas `monthly_total` y `grand_total`:**

*   **`grand_total`**:  ¡Mira esta columna! **Tiene el mismo valor en *todas* las filas: $1,262.86**.  Esto es porque `sum(amount) over ()` calcula el total **sobre *todo* el conjunto de resultados** (todos los pagos de $10 o más).  Es un **total global** que se repite en cada fila para que puedas compararlo fácilmente con cada pago individual y con los totales mensuales.

*   **`monthly_total`**:  Esta columna es más dinámica. **El valor cambia según el mes**.  Por ejemplo, para las filas de "August", `monthly_total` es $521.53.  Para las filas de "July", es $519.53, y así sucesivamente.  Esto es porque `sum(amount) over (partition by monthname(payment_date))` calcula el total **por mes**.  Muestra el **total de pagos de $10 o más para *cada mes***.

**Ejemplo 2: Cálculo de Porcentajes del Total 📊 %**

Aunque una columna como `grand_total` que se repite en cada fila pueda parecer extraña al principio, ¡es súper útil para hacer **cálculos adicionales**, como **calcular porcentajes**!  El texto nos muestra cómo calcular el **porcentaje que representa el total de cada mes sobre el total global**:

```sql
mysql> SELECT monthname(payment_date) payment_month,
    ->  sum(amount) month_total,
    ->  round(sum(amount) / sum(sum(amount)) over () * 100, 2) pct_of_total
    -> FROM payment
    -> GROUP BY monthname(payment_date);
```

**Desglose de la consulta para porcentajes:**

*   **`SELECT monthname(payment_date) payment_month, sum(amount) month_total, round(sum(amount) / sum(sum(amount)) over () * 100, 2) pct_of_total`**:  Seleccionamos:
    *   `monthname(payment_date) payment_month`:  El nombre del mes.
    *   `sum(amount) month_total`:  La suma de los pagos (`amount`) para cada mes (usando `GROUP BY monthname(payment_date)`).  Lo llamamos `month_total`.  **Ojo**: Aquí *sí* se usa `GROUP BY` porque queremos un total *por mes* como base para el porcentaje.
    *   `round(sum(amount) / sum(sum(amount)) over () * 100, 2) pct_of_total`: **¡Aquí está el cálculo del porcentaje!**  Es una expresión un poco más compleja, pero vamos a desglosarla:
        *   `sum(amount)`: Dentro de la división, la primera `sum(amount)` **sin `OVER()`** calcula el **total de pagos para *cada mes*** (porque está dentro del `SELECT` que tiene `GROUP BY monthname(payment_date)`).  Este es el **numerador** del porcentaje (la parte).
        *   `sum(sum(amount)) over ()`:  **¡Mira aquí!**  Tenemos `sum(sum(amount)) over ()`.  Parece raro, ¿verdad? Pero es muy inteligente.
            *   La parte **interna** `sum(amount)` (sin `OVER()`) dentro de la *segunda* `SUM()` **ya calculó los totales mensuales** (en la columna `month_total`).  Entonces, `sum(amount)` aquí **representa el total de *un mes específico***.
            *   Ahora, la **externa** `sum( ... ) over ()` toma esos **totales mensuales** (calculados *para cada mes en el `GROUP BY`*) y los **suma *todos juntos* usando `OVER ()`**.  ¡Esto nos da el **total global de pagos**!  Este es el **denominador** del porcentaje (el todo).
        *   `sum(amount) / sum(sum(amount)) over ()`:  Dividimos el **total de un mes** por el **total global**, obteniendo la **proporción** de ese mes sobre el total.
        *   `* 100`:  Multiplicamos por 100 para convertir la proporción en **porcentaje**.
        *   `round( ..., 2)`:  Usamos la función `ROUND()` para **redondear el resultado a 2 decimales**, para que el porcentaje sea más limpio en la presentación. Lo llamamos `pct_of_total`.
*   **`FROM payment`**:  Tabla de pagos.
*   **`GROUP BY monthname(payment_date)`**:  Agrupamos por mes para calcular los totales mensuales base para el porcentaje.

**Resultados Simplificados del ejemplo de porcentajes:**

La consulta de porcentajes nos da algo como esto:

```plaintext
+---------------+-------------+--------------+
| payment_month | month_total | pct_of_total |
+---------------+-------------+--------------+
| May           |     4824.43 |         7.16 |  <- Mayo: Total de ventas, % del total
| June          |     9631.88 |        14.29 |  <- Junio: Total de ventas, % del total
| July          |    28373.89 |        42.09 |  <- Julio: Total de ventas, % del total
| August        |    24072.13 |        35.71 |  <- Agosto: Total de ventas, % del total
| February      |      514.18 |         0.76 |  <- Febrero: Total de ventas, % del total
+---------------+-------------+--------------+
```

¡Ahora tenemos una columna `pct_of_total` que nos muestra el **porcentaje del total de ventas que corresponde a cada mes**!  Por ejemplo, Julio representa el 42.09% del total de ventas. ¡Muy útil para ver la distribución de ventas a lo largo del tiempo!

**Ejemplo 3: Comparaciones (Mínimo, Máximo, Intermedio) 📊 ↔️**

Las funciones de informe también son geniales para hacer **comparaciones**. El texto nos muestra cómo usar una expresión `CASE` junto con funciones analíticas para **comparar los totales mensuales y clasificarlos como "Máximo", "Mínimo", o "Intermedio"** según si son el mes con el total más alto, más bajo, o un valor entre ambos:

```sql
mysql> SELECT monthname(payment_date) payment_month,
    ->  sum(amount) month_total,
    ->  CASE sum(amount)
    ->    WHEN max(sum(amount)) over () THEN 'Highest'
    ->    WHEN min(sum(amount)) over () THEN 'Lowest'
    ->    ELSE 'Middle'
    ->  END descriptor
    -> FROM payment
    -> GROUP BY monthname(payment_date);
```

**Desglose de la consulta de comparación (Min, Max, Medio):**

*   **`SELECT monthname(payment_date) payment_month, sum(amount) month_total, CASE sum(amount) WHEN max(sum(amount)) over () THEN 'Highest' WHEN min(sum(amount)) over () THEN 'Lowest' ELSE 'Middle' END descriptor`**:  Seleccionamos:
    *   `monthname(payment_date) payment_month`: Nombre del mes.
    *   `sum(amount) month_total`: Total de pagos por mes (con `GROUP BY`).
    *   **`CASE sum(amount) WHEN max(sum(amount)) over () THEN 'Highest' WHEN min(sum(amount)) over () THEN 'Lowest' ELSE 'Middle' END descriptor`**:  ¡Esta es la expresión `CASE` que hace la comparación y clasificación!
        *   `CASE sum(amount)`:  Empezamos un `CASE` que **evalúa el valor de `sum(amount)`** (el total de pagos para el mes actual, ya calculado por el `GROUP BY`).
        *   `WHEN max(sum(amount)) over () THEN 'Highest'`:  **Primera condición**:  `WHEN` el `sum(amount)` (total mensual) **es igual a** `max(sum(amount)) over ()` (el **máximo total mensual *de todos los meses***, calculado con `MAX() OVER ()`), `THEN` asignamos el valor 'Highest' a la columna `descriptor`.
        *   `WHEN min(sum(amount)) over () THEN 'Lowest'`:  **Segunda condición**: `WHEN` el `sum(amount)` (total mensual) **es igual a** `min(sum(amount)) over ()` (el **mínimo total mensual *de todos los meses***, calculado con `MIN() OVER ()`), `THEN` asignamos el valor 'Lowest' a `descriptor`.
        *   `ELSE 'Middle'`:  Si **ninguna de las condiciones anteriores se cumple** (es decir, el total mensual no es ni el máximo ni el mínimo), entonces `ELSE` asignamos el valor 'Middle' a `descriptor`.
        *   `END descriptor`:  Cerramos la expresión `CASE` y le damos el alias `descriptor` a la columna resultante.
*   **`FROM payment`**: Tabla de pagos.
*   **`GROUP BY monthname(payment_date)`**: Agrupamos por mes para obtener los totales mensuales base para la comparación.

**Resultados Simplificados del ejemplo de comparación (Min, Max, Medio):**

La consulta de comparación nos da este resultado:

```plaintext
+---------------+-------------+------------+
| payment_month | month_total | descriptor |
+---------------+-------------+------------+
| May           |     4824.43 | Middle     |  <- Mayo: Total mensual, descriptor 'Middle' (intermedio)
| June          |     9631.88 | Middle     |  <- Junio: Total mensual, descriptor 'Middle' (intermedio)
| July          |    28373.89 | Highest    |  <- Julio: Total mensual, descriptor 'Highest' (más alto)
| August        |    24072.13 | Middle     |  <- Agosto: Total mensual, descriptor 'Middle' (intermedio)
| February      |      514.18 | Lowest     |  <- Febrero: Total mensual, descriptor 'Lowest' (más bajo)
+---------------+-------------+------------+
```

La columna `descriptor` ahora nos da una **"cuasi-clasificación" descriptiva** para cada mes: "Highest" para el mes con el total más alto (Julio), "Lowest" para el mes con el total más bajo (Febrero), y "Middle" para los demás meses. ¡Es una forma sencilla de **identificar visualmente valores atípicos (máximos y mínimos) y valores intermedios** en tu informe!

**Resumen de las Funciones de Informes:**

En resumen, este texto te ha mostrado cómo las **funciones analíticas** combinadas con **funciones de agregación** (`SUM()`, `MAX()`, `MIN()`, etc.) y la cláusula `OVER()` son increíblemente poderosas para generar **informes detallados y con cálculos complejos en SQL**. Puedes:

*   Calcular **totales y promedios** sobre diferentes "ventanas" de datos (como por mes, por categoría, etc.) usando `PARTITION BY` dentro de `OVER()`.
*   Calcular **totales globales** sobre todo el conjunto de resultados usando `OVER()` vacío.
*   Usar estos totales (mensuales, globales, etc.) para hacer **cálculos adicionales**, como **porcentajes** y **comparaciones**.
*   Usar expresiones `CASE` junto con funciones analíticas para **crear "descriptores"** que resumen visualmente la información y resaltan valores clave (máximos, mínimos, etc.).


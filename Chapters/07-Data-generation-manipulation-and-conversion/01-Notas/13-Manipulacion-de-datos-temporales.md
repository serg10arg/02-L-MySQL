
---

## Manipulación de Datos Temporales

### Funciones Temporales que Devuelven Fechas

Muchas de las funciones temporales integradas toman una fecha como argumento y devuelven otra fecha. Por ejemplo, la función `date_add()` de MySQL te permite agregar cualquier tipo de intervalo (p.ej., días, meses, años) a una fecha específica para generar otra fecha. Aquí tienes un ejemplo que demuestra cómo agregar cinco días a la fecha actual:

```sql
mysql> SELECT DATE_ADD(CURRENT_DATE(), INTERVAL 5 DAY);
+------------------------------------------+
| DATE_ADD(CURRENT_DATE(), INTERVAL 5 DAY) |
+------------------------------------------+
| 2019-06-10                               |
+------------------------------------------+
1 row in set (0.06 sec)
```

El segundo argumento está compuesto por tres elementos: la palabra clave de intervalo, la cantidad deseada y el tipo de intervalo. La siguiente tabla muestra algunos de los tipos de intervalos más comúnmente utilizados.

### Tabla 7-5. Tipos de Intervalos Comunes

| Nombre del Intervalo | Descripción                                     |
|----------------------|-------------------------------------------------|
| second               | Número de segundos                              |
| minute               | Número de minutos                               |
| hour                 | Número de horas                                 |
| day                  | Número de días                                  |
| month                | Número de meses                                 |
| year                 | Número de años                                  |
| minute_second        | Número de minutos y segundos, separados por ":" |
| hour_second          | Número de horas, minutos y segundos, separados por ":" |
| year_month           | Número de años y meses, separados por "-"       |

Mientras que los primeros seis tipos enumerados en la Tabla 7-5 son bastante sencillos, los últimos tres tipos requieren un poco más de explicación ya que tienen múltiples elementos. Por ejemplo, si te dicen que una película fue devuelta 3 horas, 27 minutos y 11 segundos más tarde de lo que se especificó originalmente, puedes corregirlo mediante lo siguiente:

```sql
UPDATE rental
SET return_date = DATE_ADD(return_date, INTERVAL '3:27:11' HOUR_SECOND)
WHERE rental_id = 99999;
```

En este ejemplo, la función `date_add()` toma el valor en la columna `return_date` y le suma 3 horas, 27 minutos y 11 segundos. Luego usa el valor resultante para modificar la columna `return_date`.

O, si trabajas en recursos humanos y descubriste que el empleado con ID 4789 afirmó ser mayor de lo que realmente es, podrías agregar 9 años y 11 meses a su fecha de nacimiento, como en:

```sql
UPDATE employee
SET birth_date = DATE_ADD(birth_date, INTERVAL '9-11' YEAR_MONTH)
WHERE emp_id = 4789;
```

Los usuarios de SQL Server pueden lograr el ejemplo anterior usando la función `dateadd()`:
```sql
UPDATE employee
SET birth_date = DATEADD(MONTH, 119, birth_date)
WHERE emp_id = 4789;
```

SQL Server no tiene intervalos combinados (es decir, `year_month`), por lo que convertí 9 años y 11 meses a 119 meses.

Los usuarios de Oracle Database pueden usar la función `add_months()` para este ejemplo, como en:
```sql
UPDATE employee
SET birth_date = ADD_MONTHS(birth_date, 119)
WHERE emp_id = 4789;
```

Hay algunos casos en los que deseas agregar un intervalo a una fecha, y sabes a dónde quieres llegar pero no cuántos días se necesitan para llegar allí. Por ejemplo, supongamos que un cliente de un banco inicia sesión en el sistema de banca en línea y programa una transferencia para el final del mes. En lugar de escribir algún código que calcule el mes actual y luego busque el número de días en ese mes, puedes llamar a la función `last_day()`, que hace el trabajo por ti (tanto MySQL como Oracle Database incluyen la función `last_day()`; SQL Server no tiene una función comparable). Si el cliente solicita la transferencia el 17 de septiembre de 2019, podrías encontrar el último día de septiembre mediante lo siguiente:

```sql
mysql> SELECT LAST_DAY('2019-09-17');
+------------------------+
| LAST_DAY('2019-09-17') |
+------------------------+
| 2019-09-30             |
+------------------------+
1 row in set (0.10 sec)
```

Ya sea que proporciones un valor de fecha o datetime, la función `last_day()` siempre devuelve una fecha. Aunque esta función puede no parecer un gran ahorro de tiempo, la lógica subyacente puede ser complicada si estás tratando de encontrar el último día de febrero y necesitas averiguar si el año actual es un año bisiesto.

### Funciones Temporales que Devuelven Cadenas

La mayoría de las funciones temporales que devuelven valores de cadena se utilizan para extraer una porción de una fecha o tiempo. Por ejemplo, MySQL incluye la función `dayname()` para determinar qué día de la semana cae en una determinada fecha, como en:

```sql
mysql> SELECT DAYNAME('2019-09-18');
+-----------------------+
| DAYNAME('2019-09-18') |
+-----------------------+
| Wednesday             |
+-----------------------+
1 row in set (0.00 sec)
```

Muchas de estas funciones se incluyen con MySQL para extraer información de los valores de fecha, pero recomiendo que utilices la función `extract()` en su lugar, ya que es más fácil recordar algunas variaciones de una función que recordar una docena de funciones diferentes. Además, la función `extract()` es parte del estándar SQL:2003 y ha sido implementada tanto por Oracle Database como por MySQL.

La función `extract()` utiliza los mismos tipos de intervalos que la función `date_add()` (ver Tabla 7-5) para definir qué elemento de la fecha te interesa. Por ejemplo, si deseas extraer solo la porción del año de un valor datetime, puedes hacer lo siguiente:

```sql
mysql> SELECT EXTRACT(YEAR FROM '2019-09-18 22:19:05');
+------------------------------------------+
| EXTRACT(YEAR FROM '2019-09-18 22:19:05') |
+------------------------------------------+
|                                     2019 |
+------------------------------------------+
1 row in set (0.00 sec)
```

SQL Server no incluye una implementación de `extract()`, pero sí incluye la función `datepart()`. Aquí tienes cómo extraerías el año de un valor datetime usando `datepart()`:
```sql
SELECT DATEPART(YEAR, GETDATE());
```

### Funciones Temporales que Devuelven Números

Anteriormente en este capítulo, te mostré una función utilizada para agregar un intervalo dado a un valor de fecha, generando así otro valor de fecha. Otra actividad común al trabajar con fechas es tomar dos valores de fecha y determinar el número de intervalos (días, semanas, años) entre las dos fechas. Para este propósito, MySQL incluye la función `datediff()`, que devuelve el número de días completos entre dos fechas. Por ejemplo, si quiero saber el número de días que mis hijos estarán fuera de la escuela este verano, puedo hacer lo siguiente:

```sql
mysql> SELECT DATEDIFF('2019-09-03', '2019-06-21');
+--------------------------------------+
| DATEDIFF('2019-09-03', '2019-06-21') |
+--------------------------------------+
|                                   74 |
+--------------------------------------+
1 row in set (0.00 sec)
```

Así, tendré que soportar 74 días de hiedra venenosa, picaduras de mosquitos y raspones en las rodillas antes de que los niños estén de vuelta en la escuela de manera segura. La función `datediff()` ignora la hora del día en sus argumentos. Incluso si incluyo una hora del día, configurándola a un segundo hasta la medianoche para la primera fecha y a un segundo después de la medianoche para la segunda fecha, esos tiempos no tendrán efecto en el cálculo:

```sql
mysql> SELECT DATEDIFF('2019-09-03 23:59:59', '2019-06-21 00:00:01');
+--------------------------------------------------------+
| DATEDIFF('2019-09-03 23:59:59', '2019-06-21 00:00:01') |
+--------------------------------------------------------+
|                                                     74 |
+--------------------------------------------------------+
1 row in set (0.00 sec)
```

Si intercambio los argumentos y coloco la fecha anterior primero, `datediff()` devolverá un número negativo, como en:

```sql
mysql> SELECT DATEDIFF('2019-06-21', '2019-09-03');
+--------------------------------------+
| DATEDIFF('2019-06-21', '2019-09-03') |
+--------------------------------------+
|                                  -74 |
+--------------------------------------+
1 row in set (0.00 sec)
```

SQL Server también incluye la función `datediff()`, pero es más flexible que la implementación de MySQL en el sentido de que puedes especificar el tipo de intervalo (es decir, año, mes, día, hora) en lugar de contar solo el número de días entre dos fechas. Aquí tienes cómo SQL Server realizaría el ejemplo anterior:

```sql
SELECT DATEDIFF(DAY, '2019-06-21', '2019-09-03');
```

Oracle Database te permite determinar el número de días entre dos fechas simplemente restando una fecha de otra.

---


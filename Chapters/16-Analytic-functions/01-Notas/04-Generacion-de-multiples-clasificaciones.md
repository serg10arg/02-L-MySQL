
## **Generación de Múltiples Clasificaciones: Rankings ¡Por Grupos!**

En la sección anterior, vimos cómo usar `RANK()` para obtener un ranking de clientes basado en el total de alquileres. Pero ahora, el departamento de marketing quiere refinar su estrategia: ¡quieren ofrecer alquileres gratis a los **5 mejores clientes de *cada mes***!  Ya no es un ranking global, sino **rankings mensuales separados**.

**El Primer Paso: Agrupar por Cliente y Mes 🗓️**

Para empezar, necesitamos una consulta que nos muestre el número de alquileres de cada cliente, pero **desglosado por mes**. El texto nos muestra esta consulta inicial:

```sql
SELECT customer_id,
       monthname(rental_date) rental_month,
       count(*) num_rentals
FROM rental
GROUP BY customer_id, monthname(rental_date)
ORDER BY 2, 3 desc;
```

**Desglose de esta consulta inicial:**

*   **`SELECT customer_id, monthname(rental_date) rental_month, count(*) num_rentals`**:  Seleccionamos:
    *   `customer_id`: El identificador del cliente.
    *   `monthname(rental_date) rental_month`:  El nombre del mes en que se realizó el alquiler (extraído de la columna `rental_date`).  Lo llamamos `rental_month`.
    *   `count(*) num_rentals`:  La cantidad de alquileres (`count(*)`) para cada combinación de cliente y mes. Lo llamamos `num_rentals`.
*   **`FROM rental`**:  La tabla de origen es `rental` (que guarda información sobre los alquileres).
*   **`GROUP BY customer_id, monthname(rental_date)`**:  Aquí agrupamos los resultados por **dos cosas**: `customer_id` y `monthname(rental_date)`. Esto significa que obtendremos una fila por cada **combinación única de cliente y mes**.  Así podemos contar los alquileres de cada cliente *para cada mes*.
*   **`ORDER BY 2, 3 desc`**:  Ordenamos el resultado para que sea más fácil de leer:
    *   `ORDER BY 2`: Primero, ordenamos por la segunda columna en el `SELECT`, que es `rental_month` (el nombre del mes), en orden alfabético por defecto.
    *   `, 3 desc`: Dentro de cada mes, ordenamos por la tercera columna, `num_rentals` (el número de alquileres), en orden **descendente** (`desc`), para que los clientes con más alquileres dentro de cada mes aparezcan primero.

**Resultados Simplificados de la consulta inicial:**

Esta consulta nos da algo como esto (recuerda, está simplificado para entenderlo mejor):

```plaintext
+-------------+--------------+-------------+
| customer_id | rental_month | num_rentals |
+-------------+--------------+-------------+
|         119 | August       |          18 |  <- Cliente 119, Agosto, 18 alquileres (los que más en Agosto, pero podría haber empates)
|          15 | August       |          18 |  <- Cliente 15, Agosto, 18 alquileres (¡Empate con el anterior!)
|         569 | August       |          18 |  <- Cliente 569, Agosto, 18 alquileres (¡Otro empate más!)
|         148 | August       |          18 |  <- Cliente 148, Agosto, 18 alquileres (¡Y otro más!)
| ...         | ...          | ...         |
|          75 | February     |           3 |  <- Cliente 75, Febrero, 3 alquileres
|         148 | July         |          22 |  <- Cliente 148, Julio, 22 alquileres (los que más en Julio, pero podrían haber empates)
|         102 | July         |          21 |  <- Cliente 102, Julio, 21 alquileres
| ...         | ...          | ...         |
```

Ahora tenemos una tabla que muestra los alquileres por cliente y por mes. ¡Ya estamos listos para el ranking mensual!

**Usando `PARTITION BY` para Rankings Mensuales 🏆**

Para conseguir rankings **separados para cada mes**, necesitamos decirle a la función `RANK()` que **divida los datos en "ventanas" por mes**.  Esto lo hacemos con la cláusula `PARTITION BY` dentro de `OVER()`.  La consulta modificada es:

```sql
SELECT customer_id,
       monthname(rental_date) rental_month,
       count(*) num_rentals,
       rank() over (partition by monthname(rental_date)
                        order by count(*) desc) rank_rnk
FROM rental
GROUP BY customer_id, monthname(rental_date)
ORDER BY 2, 3 desc;
```

**La parte mágica: `rank() over (partition by monthname(rental_date) order by count(*) desc) rank_rnk`**

*   **`rank() over (...)`**:  Ya sabemos que `RANK()` es la función de clasificación.  `OVER()` nos permite definir la "ventana" sobre la que opera.
*   **`partition by monthname(rental_date)`**:  **¡Aquí está la clave!**  `PARTITION BY` le dice a `RANK()` que **divida el conjunto de resultados en grupos o "ventanas"**, basándose en la columna `monthname(rental_date)` (el nombre del mes).  Para cada mes diferente (Agosto, Febrero, Julio, etc.), `RANK()` va a crear una ventana **separada**.  ¡Es como si hiciéramos un ranking **para cada mes por separado**!
*   **`order by count(*) desc`**: Dentro de **cada ventana mensual**, las filas (clientes) se ordenan por `count(*) desc` (número de alquileres en orden descendente).  Y dentro de esa ventana mensual ordenada, `RANK()` asigna los rangos (1, 2, 3...).

**Resultados Simplificados con Ranking Mensual:**

Ahora, la consulta nos da este resultado (simplificado):

```plaintext
+-------------+--------------+-------------+----------+
| customer_id | rental_month | num_rentals | rank_rnk |
+-------------+--------------+-------------+----------+
|         569 | August       |          18 |        1 |  <- Cliente 569, Agosto, Rango 1 (¡Mejor en Agosto!)
|         119 | August       |          18 |        1 |  <- Cliente 119, Agosto, Rango 1 (¡Empate!)
|         148 | August       |          18 |        1 |  <- Cliente 148, Agosto, Rango 1 (¡Más empates!)
|          15 | August       |          18 |        1 |  <- Cliente 15, Agosto, Rango 1 (¡Otro más!)
| ...         | ...          | ...         | ...      |
|          75 | February     |           3 |        1 |  <- Cliente 75, Febrero, Rango 1 (¡Mejor en Febrero!)
|         457 | February     |           2 |        2 |  <- Cliente 457, Febrero, Rango 2
| ...         | ...          | ...         | ...      |
|         148 | July         |          22 |        1 |  <- Cliente 148, Julio, Rango 1 (¡Mejor en Julio!)
|         102 | July         |          21 |        2 |  <- Cliente 102, Julio, Rango 2
| ...         | ...          | ...         | ...      |
```

¡Fíjate en la columna `rank_rnk`!  **Para cada mes, el ranking empieza de nuevo en 1**.  Tenemos múltiples "rankings", uno para cada mes.  En Agosto, hay varios clientes empatados en el rango 1, y en Febrero el cliente 75 es el número 1 de ese mes.  ¡`PARTITION BY` realmente nos permite dividir el ranking por grupos!

**Filtrando para Quedarnos con los 5 Mejores de Cada Mes 🥇🥈🥉🥈🥉**

Ahora que tenemos el ranking mensual, el departamento de marketing solo quiere los **5 mejores clientes *de cada mes***.  Para hacer esto, necesitamos **filtrar** el resultado por el `rank_rnk` para quedarnos solo con los que tienen un ranking de 5 o menos.

**¡Truco! Subconsulta porque no podemos usar `WHERE` directamente con funciones de ventana.**

Como las funciones de ventana como `RANK()` solo se pueden usar en la cláusula `SELECT` (no en `WHERE`), **necesitamos usar una subconsulta** para poder filtrar por el `rank_rnk`.  La consulta final con el filtro es:

```sql
SELECT customer_id, rental_month, num_rentals,
       rank_rnk ranking -- Cambiamos el nombre a 'ranking' para la salida final
FROM
  (SELECT customer_id, -- Subconsulta empieza aquí
          monthname(rental_date) rental_month,
          count(*) num_rentals,
          rank() over (partition by monthname(rental_date)
                           order by count(*) desc) rank_rnk
   FROM rental
   GROUP BY customer_id, monthname(rental_date)
  ) cust_rankings -- Alias para la subconsulta (¡importante!)
WHERE rank_rnk <= 5 -- Filtramos aquí: rank_rnk debe ser menor o igual a 5
ORDER BY rental_month, num_rentals desc, rank_rnk; -- Ordenamos para la salida final
```

**Desglose de la consulta final con subconsulta:**

1.  **`SELECT customer_id, rental_month, num_rentals, rank_rnk ranking FROM (...) cust_rankings WHERE rank_rnk <= 5 ORDER BY rental_month, num_rentals desc, rank_rnk;`**:  Esta es la **consulta principal**, que selecciona las columnas que queremos mostrar en el resultado final.
    *   `FROM (...) cust_rankings`:  La fuente de datos **no es una tabla directamente**, sino una **subconsulta** que está entre paréntesis.  Le damos el alias `cust_rankings` a esta subconsulta. ¡Alias **obligatorio** para poder referenciar las columnas de la subconsulta!
    *   `WHERE rank_rnk <= 5`:  Aquí **filtramos** los resultados.  Seleccionamos solo las filas **donde la columna `rank_rnk` (que calculamos en la subconsulta) es menor o igual a 5**.  ¡Así nos quedamos con los 5 mejores clientes de cada mes!
    *   `ORDER BY rental_month, num_rentals desc, rank_rnk`:  Ordenamos el resultado final por mes, luego por número de alquileres (descendente dentro del mes) y finalmente por ranking (para desempatar si hay empates en número de alquileres).

2.  **Subconsulta `(SELECT ... FROM ... GROUP BY ... rank() over ... ) cust_rankings`**:  Esta es la **subconsulta** que calcula los rankings mensuales.  **Es *exactamente* la consulta que vimos antes para generar rankings mensuales**, ¡la que usa `PARTITION BY monthname(rental_date)` en `RANK()`!  La metemos dentro de paréntesis y le damos un alias (`cust_rankings`).

**Resultados Finales: Los 5 Mejores de Cada Mes 🎉**

El resultado de esta consulta final será una lista de los **5 mejores clientes de cada mes**, con su ranking mensual.  Por ejemplo, para el mes de Agosto, mostraría los clientes que están en el rango 1 al 5 (o menos si hay muchos empates en el rango 5), y lo mismo para Febrero, Julio, y todos los demás meses para los que haya datos.

**Nota Importante: Funciones Analíticas solo en `SELECT` (¡y Subconsultas al Rescate!)**

El texto termina con una **nota clave**: ¡las **funciones analíticas** (como `RANK()`, `ROW_NUMBER()`, `DENSE_RANK()`, etc.) solo se pueden usar **directamente en la cláusula `SELECT`** de una consulta!  **No puedes usarlas directamente en la cláusula `WHERE`** para filtrar los resultados basándote en los valores que calculan.

Por eso, cuando necesitas **filtrar o agrupar** basado en los resultados de funciones analíticas, **la solución es usar subconsultas**.  Calculas los valores de las funciones analíticas en la subconsulta (en la cláusula `SELECT` de la subconsulta), y luego, en la consulta principal (en la cláusula `WHERE` de la consulta principal), filtras usando esos valores calculados que ahora son columnas del resultado de la subconsulta.

**En resumen, con este texto aprendiste:**

*   Cómo usar `PARTITION BY` dentro de `RANK()` (y otras funciones analíticas) para generar **rankings por grupos o categorías** (en este caso, rankings mensuales).
*   Que `PARTITION BY` **divide el conjunto de resultados en "ventanas"** separadas, y la función analítica opera **dentro de cada ventana de forma independiente**.
*   Que para **filtrar o agrupar** basado en los resultados de funciones analíticas, **necesitas usar subconsultas**, porque estas funciones solo se pueden usar directamente en la cláusula `SELECT`.


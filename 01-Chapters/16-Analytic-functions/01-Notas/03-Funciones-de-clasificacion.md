
Este texto nos habla de tres funciones de SQL que sirven para **clasificar** filas dentro de un conjunto de resultados, como si les diéramos medallas de oro, plata, bronce, etc.  Estas funciones son: `ROW_NUMBER()`, `RANK()`, y `DENSE_RANK()`.  La principal diferencia entre ellas está en cómo manejan los **empates**, ¡cuando dos o más filas tienen el mismo "mérito" para la clasificación!

---
## **El Ejemplo del Departamento de Marketing: Los 10 Mejores Clientes 🎬🎁**

Para que lo entendamos mejor, el texto nos pone en una situación práctica: el departamento de marketing de una empresa de alquiler de películas quiere **encontrar a sus 10 mejores clientes** para regalarles un alquiler gratis.  ¿Cómo definimos "mejores" clientes aquí? ¡Por la cantidad de películas que han alquilado!

Primero, hacen una consulta para contar cuántas películas ha alquilado cada cliente y ordenar el resultado de mayor a menor número de alquileres:

```sql
SELECT customer_id, count(*) num_rentals
FROM rental
GROUP BY customer_id
ORDER BY 2 desc;
```

Esta consulta nos da una lista como esta (simplificada para que sea más fácil de ver):

```plaintext
+-------------+-------------+
| customer_id | num_rentals |
+-------------+-------------+
|         148 |          46 |  <- El mejor cliente (más alquileres)
|         526 |          45 |  <- El segundo mejor
|         236 |          42 |  <- Empate aquí
|         144 |          42 |  <- Empate aquí
|          75 |          41 |  <- El siguiente
...
```

Aquí vemos el problema: ¡los clientes 236 y 144 tienen **exactamente el mismo número de alquileres (42)**!  Si queremos dar premios a los "10 mejores", ¿cómo los clasificamos a ellos? ¿Deben tener la misma posición en el ranking? Y si es así, ¿qué posición le toca al siguiente cliente (el que tiene 41 alquileres)?

**Las Tres Funciones de Clasificación al Rescate 🦸‍♀️🦸‍♂️🦸**

SQL nos da tres funciones para ayudarnos con estos rankings, especialmente cuando hay empates: `ROW_NUMBER()`, `RANK()`, y `DENSE_RANK()`.  Vamos a ver cómo cada una de ellas maneja este ejemplo de los clientes empatados.

El texto ejecuta una consulta que usa las tres funciones a la vez, para que podamos comparar directamente los resultados:

```sql
SELECT customer_id, count(*) num_rentals,
       row_number() over (order by count(*) desc) row_number_rnk,
       rank() over (order by count(*) desc) rank_rnk,
       dense_rank() over (order by count(*) desc) dense_rank_rnk
FROM rental
GROUP BY customer_id
ORDER BY 2 desc;
```

Y aquí están los resultados simplificados, enfocándonos en los clientes con 42 alquileres y sus posiciones:

```plaintext
+-------------+-------------+----------------+----------+----------------+
| customer_id | num_rentals | row_number_rnk | rank_rnk | dense_rank_rnk |
+-------------+-------------+----------------+----------+----------------+
|         148 |          46 |              1 |        1 |              1 |
|         526 |          45 |              2 |        2 |              2 |
|         144 |          42 |              3 |        3 |              3 |
|         236 |          42 |              4 |        3 |              3 |
|          75 |          41 |              5 |        5 |              4 |
...
```

¡Veamos las diferencias en detalle!

**1. `ROW_NUMBER()`:  El Número de Fila Seco (Sin Empates Reconocidos)**

Mira la columna `row_number_rnk`.  `ROW_NUMBER()` simplemente asigna un **número de fila secuencial** a cada cliente, empezando desde 1.  No importa si hay empates, cada cliente tiene un número **único**: 1, 2, 3, 4, 5, ¡y así sigue!

*   **¿Qué hace con los empates?**  Los **ignora**.  A los clientes 144 y 236 (que tienen el mismo número de alquileres: 42), les asigna los números 3 y 4 **de forma arbitraria**.  No los reconoce como "empatados en el mismo puesto".

*   **Para qué se usa:**  Cuando necesitas **simplemente numerar las filas** en un orden específico, sin importar los empates.  Por ejemplo, si solo quieres seleccionar las primeras 10 filas de un resultado y necesitas que *siempre* sean exactamente 10 filas, aunque haya empates en la posición 10.

**2. `RANK()`: ¡El Ranking Justo con "Huecos"!**

Ahora, mira la columna `rank_rnk`.  ¡Aquí sí se reconocen los empates!  Los clientes 144 y 236, al tener el mismo número de alquileres (42), **ambos reciben el mismo rango: 3**.  ¡Eso es justo!

*   **¿Qué hace con los empates?**  Les da el **mismo rango**.  Pero, fíjate en el siguiente cliente (el 75 con 41 alquileres): ¡su rango es **5**, no 4!  **`RANK()` "salta" rangos** para indicar que hubo un empate.  Como hubo dos clientes en el rango 3, se salta el rango 4 y el siguiente rango es el 5.  Hay un "hueco" en la secuencia de números (1, 2, 3, 3, **5**, ...).

*   **Para qué se usa:**  Cuando es **importante reconocer los empates** y mostrar que varias filas comparten la misma posición en el ranking.  Es útil cuando quieres seleccionar los "top N" y quieres **incluir a *todos* los que empaten en la posición N**, aunque eso signifique seleccionar más de N filas en total.

**3. `DENSE_RANK()`: ¡El Ranking Justo y "Denso", sin Huecos!**

Finalmente, veamos la columna `dense_rank_rnk`.  Como `RANK()`, ¡también reconoce los empates!  Los clientes 144 y 236 **también reciben el mismo rango: 3**.  ¡Genial!

*   **¿Qué hace con los empates?**  Les da el **mismo rango**, ¡igual que `RANK()`!  Pero, mira el cliente siguiente (el 75 con 41 alquileres): ¡su rango es **4**, no 5!  **`DENSE_RANK()` NO "salta" rangos**.  Después del rango 3 (que se comparte entre dos clientes), el siguiente rango es simplemente el 4, ¡sin huecos!  Es un ranking "denso", donde los números van seguidos (1, 2, 3, 3, **4**, ...).

*   **Para qué se usa:** Cuando también es **importante reconocer los empates**, pero quieres un ranking **consecutivo**, sin huecos en la numeración.  Es útil cuando necesitas categorizar datos en grupos basados en rangos (por ejemplo, "top", "medio", "bajo"), y quieres que estas categorías sean contiguas y sin "saltos" debido a los empates.  También si quieres seleccionar "los 10 mejores o más" y quieres incluir a todos los empatados en la posición 10, pero luego seguir numerando consecutivamente para los siguientes grupos.

**Resumen Rápido: ¿Cuál Función Usar? 🧐**

El texto nos da una guía para elegir la función correcta para encontrar a los "10 mejores clientes":

*   **`ROW_NUMBER()`:**  Podría servir si **casi no hay empates cerca de la posición 10**.  Pero, si hay empates, podrías dejar fuera a clientes igual de "buenos" que los que sí incluyes. **No es la mejor opción si los empates importan**.

*   **`RANK()`:**  Es una **buena opción para seleccionar "los 10 mejores o menos"**.  Si usas `WHERE rank_rnk <= 10`, te aseguras de incluir a *todos* los clientes que estén en el top 10, incluyendo los que empaten en la posición 10.  **Puede devolver más de 10 clientes si hay empates en el rango 10**.  Es una opción **justa** al reconocer empates y asegurar que todos los "igual de buenos" sean considerados.

*   **`DENSE_RANK()`:**  También sirve para seleccionar "los 10 mejores o menos" (con `WHERE dense_rank_rnk <= 10`).  **Puede incluir *incluso más* clientes que `RANK()` si hay muchos empates en los rangos inferiores**, porque los rangos son más "densos" y no se saltan números.  Podría ser útil si quieres ser muy inclusivo y no dejar fuera a *nadie* que esté cerca del top 10, incluso si eso significa seleccionar muchos más de 10.

**En la mayoría de las situaciones donde los empates son posibles y relevantes, `RANK()` suele ser una opción adecuada** porque equilibra el reconocimiento de los empates con la idea de seleccionar un "top N". Pero, la **mejor elección siempre dependerá de lo que necesites *específicamente* para tu análisis**.


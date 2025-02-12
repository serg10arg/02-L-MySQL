¡Claro! Aquí tienes la traducción del texto seleccionado:

### Null: Esa Palabra de Cuatro Letras

Lo he pospuesto tanto como pude, pero es momento de abordar un tema que tiende a ser recibido con miedo, incertidumbre y temor: el valor `null`. `null` es la ausencia de un valor; antes de que un empleado sea despedido, por ejemplo, su columna `end_date` en la tabla `employee` debería ser `null`. No hay un valor que pueda asignarse a la columna `end_date` que tenga sentido en esta situación. Sin embargo, `null` es un poco escurridizo, ya que existen varios tipos de `null`:
- **No aplicable**: como la columna de ID de empleado para una transacción que tuvo lugar en un cajero automático.
- **Valor aún no conocido**: como cuando el ID federal no se conoce en el momento en que se crea una fila de cliente.
- **Valor indefinido**: como cuando se crea una cuenta para un producto que aún no ha sido agregado a la base de datos.

Algunos teóricos argumentan que debería haber una expresión diferente para cubrir cada una de estas (y más) situaciones, pero la mayoría de los practicantes estarían de acuerdo en que tener múltiples valores `null` sería demasiado confuso.

Al trabajar con `null`, debes recordar:
- Una expresión puede ser `null`, pero nunca puede ser igual a `null`.
- Dos valores `null` nunca son iguales entre sí.

Para probar si una expresión es `null`, necesitas usar el operador `IS NULL`, como se demuestra a continuación:
```sql
mysql> SELECT rental_id, customer_id
    -> FROM rental
    -> WHERE return_date IS NULL;
```
**Resultado**:
```sql
+-----------+-------------+
| rental_id | customer_id |
+-----------+-------------+
|     11496 |         155 |
|     11541 |         335 |
|     11563 |          83 |
|     11577 |         219 |
|     11593 |          99 |
...
|     15867 |         505 |
|     15875 |          41 |
|     15894 |         168 |
|     15966 |         374 |
+-----------+-------------+
183 rows in set (0.01 sec)
```
Esta consulta encuentra todas las rentas de películas que nunca fueron devueltas. Aquí tienes la misma consulta usando `= null` en lugar de `IS NULL`:
```sql
mysql> SELECT rental_id, customer_id
    -> FROM rental
    -> WHERE return_date = NULL;
```
**Resultado**:
```sql
Empty set (0.01 sec)
```
Como puedes ver, la consulta se parsea y se ejecuta, pero no devuelve ninguna fila. Este es un error común cometido por programadores SQL inexpertos, y el servidor de bases de datos no te alertará sobre tu error, así que ten cuidado al construir condiciones que prueben `null`.

Si quieres ver si se ha asignado un valor a una columna, puedes usar el operador `IS NOT NULL`, como en:
```sql
mysql> SELECT rental_id, customer_id, return_date
    -> FROM rental
    -> WHERE return_date IS NOT NULL;
```
**Resultado**:
```sql
+-----------+-------------+---------------------+
| rental_id | customer_id | return_date         |
+-----------+-------------+---------------------+
|         1 |         130 | 2005-05-26 22:04:30 |
|         2 |         459 | 2005-05-28 19:40:33 |
|         3 |         408 | 2005-06-01 22:12:39 |
|         4 |         333 | 2005-06-03 01:43:41 |
|         5 |         222 | 2005-06-02 04:33:21 |
|         6 |         549 | 2005-05-27 01:32:07 |
|         7 |         269 | 2005-05-29 20:34:53 |
...
|     16043 |         526 | 2005-08-31 03:09:03 |
|     16044 |         468 | 2005-08-25 04:08:39 |
|     16045 |          14 | 2005-08-25 23:54:26 |
|     16046 |          74 | 2005-08-27 18:02:47 |
|     16047 |         114 | 2005-08-25 02:48:48 |
|     16048 |         103 | 2005-08-31 21:33:07 |
|     16049 |         393 | 2005-08-30 01:01:12 |
+-----------+-------------+---------------------+
15861 rows in set (0.02 sec)
```
Esta versión de la consulta devuelve todas las rentas que fueron devueltas, que es la mayoría de las filas en la tabla (15,861 de 16,044).

---







Así es. Cuando trabajas con bases de datos desconocidas, es crucial identificar qué columnas permiten valores `NULL` para asegurarte de que tus condiciones de filtro sean precisas y completas.

### Resumen de Consideraciones sobre `NULL`

1. **Identificación de Filas con Valores `NULL`**:
    - Usa el operador `IS NULL` para buscar filas donde una columna específica tiene un valor `NULL`.
    - Ejemplo:
      ```sql
      SELECT rental_id, customer_id
      FROM rental
      WHERE return_date IS NULL;
      ```

2. **Identificación de Filas con Valores No `NULL`**:
    - Usa el operador `IS NOT NULL` para buscar filas donde una columna específica no tiene un valor `NULL`.
    - Ejemplo:
      ```sql
      SELECT rental_id, customer_id, return_date
      FROM rental
      WHERE return_date IS NOT NULL;
      ```

3. **Condiciones Compuestas con `NULL`**:
    - Cuando combines condiciones con rangos o valores específicos, recuerda incluir las filas con `NULL` si es relevante para tu consulta.
    - Ejemplo: Encontrar todas las rentas no devueltas durante un período específico y aquellas que nunca fueron devueltas:
      ```sql
      SELECT rental_id, customer_id, return_date
      FROM rental
      WHERE return_date IS NULL
        OR return_date NOT BETWEEN '2005-05-01' AND '2005-09-01';
      ```

4. **Consejos Adicionales**:
    - Comprende que una expresión `NULL` no es igual a otra `NULL`.
    - Evita usar `= NULL` o `<> NULL` para comparar valores `NULL`. Usa siempre `IS NULL` o `IS NOT NULL`.
    - Considera los valores `NULL` al planificar tus condiciones de filtro para evitar que los datos se pierdan o se omitan accidentalmente.


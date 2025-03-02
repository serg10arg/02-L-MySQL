
---

## **Cómo se Utilizan los Índices**

Los índices son generalmente utilizados por el servidor para localizar rápidamente filas en una tabla en particular, después de lo cual el servidor visita la tabla asociada para extraer la información adicional solicitada por el usuario. Considera la siguiente consulta:

```sql
mysql> SELECT customer_id, first_name, last_name
    -> FROM customer
    -> WHERE first_name LIKE 'S%' AND last_name LIKE 'P%';
+-------------+------------+-----------+
| customer_id | first_name | last_name |
+-------------+------------+-----------+
|          84 | SARA       | PERRY     |
|         197 | SUE        | PETERS    |
|         167 | SALLY      | PIERCE    |
+-------------+------------+-----------+
3 rows in set (0.00 sec)
```

Para esta consulta, el servidor puede emplear cualquiera de las siguientes estrategias:
- Escanear todas las filas en la tabla **customer**.
- Utilizar el índice en la columna **last_name** para encontrar todos los clientes cuyo apellido comienza con P; luego visitar cada fila de la tabla **customer** para encontrar solo las filas cuyo nombre comienza con S.
- Utilizar el índice en las columnas **last_name** y **first_name** para encontrar todos los clientes cuyo apellido comienza con P y cuyo nombre comienza con S.

La tercera opción parece ser la mejor, ya que el índice producirá todas las filas necesarias para el conjunto de resultados, sin necesidad de revisar la tabla. Pero, ¿cómo sabes cuál de las tres opciones se utilizará? Para ver cómo el optimizador de consultas de MySQL decide ejecutar la consulta, uso la declaración **explain** para pedirle al servidor que muestre el plan de ejecución de la consulta en lugar de ejecutar la consulta:

```sql
mysql> EXPLAIN
    -> SELECT customer_id, first_name, last_name
    -> FROM customer
    -> WHERE first_name LIKE 'S%' AND last_name LIKE 'P%' \G;
*************************** 1. row ***************************
          id: 1
 select_type: SIMPLE
       table: customer
  partitions: NULL
        type: range
possible_keys: idx_last_name, idx_full_name
         key: idx_full_name
     key_len: 274
         ref: NULL
        rows: 28
    filtered: 11.11
       Extra: Using where; Using index
1 row in set, 1 warning (0.00 sec)
```

Cada servidor de base de datos incluye herramientas para permitirte ver cómo el optimizador de consultas maneja tu declaración SQL. SQL Server te permite ver un plan de ejecución emitiendo la declaración **set showplan_text on** antes de ejecutar tu declaración SQL. Oracle Database incluye la declaración **explain plan**, que escribe el plan de ejecución en una tabla especial llamada **plan_table**.

Al observar los resultados de la consulta, la columna **possible_keys** te dice que el servidor podría decidir usar el índice **idx_last_name** o el **idx_full_name**, y la columna **key** te dice que se eligió el índice **idx_full_name**. Además, la columna **type** te indica que se utilizará un escaneo de rango, lo que significa que el servidor de la base de datos buscará un rango de valores en el índice, en lugar de esperar recuperar una sola fila.

El proceso que te acabo de mostrar es un ejemplo de ajuste de consultas. El ajuste implica observar una declaración SQL y determinar los recursos disponibles para el servidor para ejecutar la declaración. Puedes decidir modificar la declaración SQL, ajustar los recursos de la base de datos o hacer ambas cosas para que una declaración se ejecute de manera más eficiente. El ajuste es un tema detallado y te recomiendo encarecidamente que leas la guía de ajuste de tu servidor o consigas un buen libro de ajuste para que puedas ver todos los diferentes enfoques disponibles para tu servidor.

---


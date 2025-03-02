### Filtrado en SQL

A veces, querrás trabajar con todas las filas de una tabla, como en los siguientes casos:
- Eliminar todos los datos de una tabla utilizada para preparar nuevas alimentaciones de almacenes de datos.
- Modificar todas las filas en una tabla después de haber agregado una nueva columna.
- Recuperar todas las filas de una tabla de cola de mensajes.

En estos casos, tus sentencias SQL no necesitarán tener una cláusula `WHERE`, ya que no necesitas excluir ninguna fila. Sin embargo, la mayoría de las veces, querrás limitar tu enfoque a un subconjunto de las filas de una tabla. Por lo tanto, todas las sentencias de datos SQL (excepto la sentencia `INSERT`) incluyen una cláusula `WHERE` opcional que contiene una o más condiciones de filtro utilizadas para restringir el número de filas sobre las que actúa la sentencia SQL. Además, la sentencia `SELECT` incluye una cláusula `HAVING` en la que pueden incluirse condiciones de filtro relacionadas con datos agrupados.

Este capítulo explora los diversos tipos de condiciones de filtro que puedes emplear en las cláusulas `WHERE` de las sentencias `SELECT`, `UPDATE` y `DELETE`. También demostraré el uso de condiciones de filtro en la cláusula `HAVING` de una sentencia `SELECT` en el Capítulo 8.

#### Ejemplos de Cláusulas `WHERE` y `HAVING`

1. **Cláusula `WHERE` en `SELECT`**:
   ```sql
   SELECT * FROM customers WHERE country = 'USA';
   ```
    - Filtra los resultados para incluir solo las filas donde el país es 'USA'.

2. **Cláusula `WHERE` en `UPDATE`**:
   ```sql
   UPDATE employees SET salary = salary * 1.10 WHERE department_id = 5;
   ```
    - Actualiza el salario de los empleados solo en el departamento con `department_id` igual a 5.

3. **Cláusula `WHERE` en `DELETE`**:
   ```sql
   DELETE FROM orders WHERE order_date < '2022-01-01';
   ```
    - Elimina las órdenes realizadas antes del 1 de enero de 2022.

4. **Cláusula `HAVING` en `SELECT`**:
   ```sql
   SELECT department_id, COUNT(*) as employee_count
   FROM employees
   GROUP BY department_id
   HAVING COUNT(*) > 10;
   ```
    - Agrupa los empleados por departamento y filtra los resultados para incluir solo los departamentos con más de 10 empleados.

El uso adecuado de las cláusulas `WHERE` y `HAVING` te permitirá trabajar eficientemente con subconjuntos de datos y aplicar condiciones específicas en tus consultas SQL.
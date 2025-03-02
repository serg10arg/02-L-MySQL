
## **Verificación de Despliegue: Asegurando la Integridad del Esquema Tras Cambios**

El texto introduce la práctica crucial de la **verificación de despliegue** en el contexto de las **ventanas de mantenimiento de bases de datos**.  Durante estas ventanas, las organizaciones realizan cambios en las bases de datos, como:

*   **Administración de objetos existentes**:  Modificación de objetos ya existentes, por ejemplo, añadir o eliminar particiones en tablas grandes para mejorar el rendimiento y la gestión.
*   **Despliegue de nuevos esquemas y código**:  Implementación de nuevas tablas, vistas, procedimientos almacenados, funciones, y otras modificaciones al esquema de la base de datos que forman parte de una nueva versión de la aplicación o una nueva funcionalidad.

**La Necesidad de la Verificación Post-Despliegue**

Después de ejecutar todos los scripts de despliegue que implementan estos cambios, es **fundamental verificar que los cambios se hayan aplicado correctamente**.  No basta con asumir que los scripts se ejecutaron sin errores.  Es esencial **confirmar** que:

*   **Los nuevos objetos de esquema existen**:  Que las nuevas tablas, vistas, etc., que se pretendían crear, realmente se han creado en la base de datos.
*   **Los objetos tienen la estructura correcta**:  Que las tablas tienen las columnas esperadas, con los tipos de datos, restricciones `NOT NULL`, y valores por defecto correctos.
*   **Índices y claves primarias están definidos**: Que los índices necesarios para el rendimiento y las restricciones de clave primaria para la integridad referencial se han creado como se esperaba.

**El Script de Verificación con `information_schema`**

El texto proporciona un ejemplo de **consulta SQL que actúa como un script de verificación**. Esta consulta utiliza `information_schema` para **contar** para cada tabla del esquema `Sakila`:

*   **Número de columnas (`num_columns`)**.
*   **Número de índices (`num_indexes`)**.
*   **Número de restricciones de clave primaria (`num_primary_keys`)**.  En la mayoría de los casos, se espera que una tabla tenga 0 o 1 clave primaria.

Aquí está la consulta nuevamente para referencia:

```sql
mysql> SELECT tbl.table_name,
    ->  (SELECT count(*) FROM information_schema.columns clm
    ->   WHERE clm.table_schema = tbl.table_schema
    ->    AND clm.table_name = tbl.table_name) num_columns,
    ->  (SELECT count(*) FROM information_schema.statistics sta
    ->   WHERE sta.table_schema = tbl.table_schema
    ->    AND sta.table_name = tbl.table_name) num_indexes,
    ->  (SELECT count(*) FROM information_schema.table_constraints tc
    ->   WHERE tc.table_schema = tbl.table_schema
    ->    AND tc.table_name = tbl.table_name
    ->    AND tc.constraint_type = 'PRIMARY KEY') num_primary_keys
    -> FROM information_schema.tables tbl
    -> WHERE tbl.table_schema = 'sakila' AND tbl.table_type = 'BASE TABLE'
    -> ORDER BY 1;
```

**Análisis Detallado de la Consulta de Verificación:**

Esta consulta es más compleja que los ejemplos anteriores, ya que utiliza **subconsultas correlacionadas** para obtener los conteos. Vamos a desglosarla paso a paso:

1.  **`SELECT tbl.table_name, ... FROM information_schema.tables tbl WHERE tbl.table_schema = 'sakila' AND tbl.table_type = 'BASE TABLE' ORDER BY 1`**:
    *   Esta es la **consulta principal**.  Selecciona información de la vista `information_schema.tables` y le da el alias `tbl`.
    *   `SELECT tbl.table_name, ...`:  Selecciona el nombre de la tabla (`table_name`) de la vista `tables`. Esta será la primera columna en el resultado.
    *   `FROM information_schema.tables tbl`:  Indica que la fuente principal de datos es la vista `tables` de `information_schema`.
    *   `WHERE tbl.table_schema = 'sakila' AND tbl.table_type = 'BASE TABLE'`:  Filtra para considerar **solo las tablas base** (excluyendo vistas) dentro del esquema `sakila`.  Esto asegura que estamos verificando las tablas de datos reales.
    *   `ORDER BY 1`: Ordena los resultados por la primera columna (`table_name`), en orden alfabético, para que la salida sea más organizada.

2.  **`(SELECT count(*) FROM information_schema.columns clm WHERE clm.table_schema = tbl.table_schema AND clm.table_name = tbl.table_name) num_columns`**:
    *   Esta es la **primera subconsulta correlacionada**.  Calcula el **número de columnas** para cada tabla.
    *   `SELECT count(*)`:  Cuenta el número de filas que devuelve la subconsulta.
    *   `FROM information_schema.columns clm`:  Consulta la vista `columns` de `information_schema` y le da el alias `clm`.
    *   `WHERE clm.table_schema = tbl.table_schema AND clm.table_name = tbl.table_name`:  Esta es la **correlación clave**.  Las condiciones `clm.table_schema = tbl.table_schema` y `clm.table_name = tbl.table_name` **relacionan esta subconsulta con la consulta principal**.  Para cada fila de la consulta principal (cada tabla `tbl`), esta subconsulta se ejecuta **filtrando la vista `columns` para que coincida con el esquema y el nombre de la tabla actual de la consulta principal (`tbl`)**.  En esencia, para cada tabla listada en la consulta principal, esta subconsulta cuenta cuántas columnas tiene esa tabla en particular.
    *   `num_columns`:  El resultado de esta subconsulta (el conteo de columnas) se aliasa como `num_columns`.

3.  **`(SELECT count(*) FROM information_schema.statistics sta WHERE sta.table_schema = tbl.table_schema AND sta.table_name = tbl.table_name) num_indexes`**:
    *   Esta es la **segunda subconsulta correlacionada**.  Calcula el **número de índices** para cada tabla.
    *   Funciona de manera **análoga a la subconsulta anterior de columnas**, pero en lugar de `information_schema.columns`, utiliza `information_schema.statistics` (vista que contiene metadatos de índices) y el alias `sta`.
    *   Cuenta el número de filas en `information_schema.statistics` que coinciden con el esquema y nombre de tabla de la fila actual de la consulta principal (`tbl`), lo que efectivamente cuenta los índices definidos en esa tabla.
    *   `num_indexes`: El resultado (conteo de índices) se aliasa como `num_indexes`.

4.  **`(SELECT count(*) FROM information_schema.table_constraints tc WHERE tc.table_schema = tbl.table_schema AND tc.table_name = tbl.table_name AND tc.constraint_type = 'PRIMARY KEY') num_primary_keys`**:
    *   Esta es la **tercera subconsulta correlacionada**. Calcula el **número de restricciones de clave primaria** para cada tabla.
    *   Utiliza `information_schema.table_constraints` (vista que contiene metadatos de restricciones de tabla) con el alias `tc`.
    *   Añade una condición **adicional**: `tc.constraint_type = 'PRIMARY KEY'`.  Esto **filtra aún más los resultados de `table_constraints` para contar solo las filas que representan restricciones de clave primaria**.  Como se espera que cada tabla tenga como máximo una clave primaria, este conteo debería ser 0 o 1.
    *   `num_primary_keys`: El resultado (conteo de claves primarias) se aliasa como `num_primary_keys`.

**Resultado de Ejemplo y su Interpretación**

El texto proporciona un **resultado de ejemplo** de la ejecución de esta consulta en la base de datos `sakila`:

```plaintext
+---------------+-------------+-------------+------------------+
| TABLE_NAME    | num_columns | num_indexes | num_primary_keys |
+---------------+-------------+-------------+------------------+
| actor         |          4 |          2 |              1 |
| address       |          9 |          3 |              1 |
| category      |          3 |          1 |              1 |
| city          |          4 |          2 |              1 |
| country       |          3 |          1 |              1 |
| customer      |          9 |          7 |              1 |
| film          |         13 |          4 |              1 |
| film_actor    |          3 |          3 |              1 |
| film_category |          3 |          3 |              1 |
| film_text     |          3 |          3 |              1 |
| inventory     |          4 |          4 |              1 |
| language      |          3 |          1 |              1 |
| payment       |          7 |          4 |              1 |
| rental        |          7 |          7 |              1 |
| staff         |         11 |          3 |              1 |
| store         |          4 |          3 |              1 |
+---------------+-------------+-------------+------------------+
16 rows in set (0.01 sec)
```

**Columnas del Resultado:**

*   **`TABLE_NAME`**: Nombre de la tabla.
*   **`num_columns`**: Número de columnas en la tabla.
*   **`num_indexes`**: Número de índices en la tabla.
*   **`num_primary_keys`**:  Número de restricciones de clave primaria en la tabla (en la base de datos Sakila, todas las tablas base tienen exactamente una clave primaria, por lo que esta columna es siempre 1).

**Cómo Utilizar la Consulta para Verificación de Despliegue**

El texto explica **cómo usar esta consulta en un escenario de despliegue**:

1.  **Ejecutar la consulta ANTES del despliegue**: Guarda los resultados de esta consulta **antes de ejecutar los scripts de despliegue** que modificarán la base de datos.  Este resultado representa el estado "baseline" o inicial del esquema.
2.  **Ejecutar los scripts de despliegue**:  Realiza la ventana de mantenimiento y ejecuta todos los scripts SQL que implementan los cambios planeados (creación de nuevas tablas, modificación de tablas existentes, etc.).
3.  **Ejecutar la consulta DESPUÉS del despliegue**:  Vuelve a ejecutar **exactamente la misma consulta de verificación** después de que los scripts de despliegue se hayan completado.  Guarda los nuevos resultados.
4.  **Comparar los resultados**:  **Compara los resultados "antes" y "después"**.  Idealmente, si el despliegue fue exitoso, **no debería haber diferencias significativas en los conteos de columnas, índices y claves primarias para las tablas existentes que no se esperaba modificar estructuralmente**.  Para las **nuevas tablas** que se crearon durante el despliegue, verificarás que **aparezcan en los resultados "después"** y que tengan los **conteos de columnas, índices y claves primarias esperados**.

**Beneficios de este Enfoque de Verificación:**

*   **Automatización**:  La verificación se realiza mediante una simple consulta SQL, que puede ser fácilmente automatizada e integrada en scripts de despliegue o herramientas de monitoreo.
*   **Programático y Estándar**: Utiliza `information_schema`, que es una forma estándar y programática de acceder a los metadatos, lo que hace que el script sea más portable entre diferentes sistemas de bases de datos que soporten `information_schema`.
*   **Rápido y Eficiente**:  La consulta es relativamente rápida de ejecutar y proporciona una visión general del estado del esquema.
*   **Detección temprana de problemas**:  Permite detectar rápidamente problemas comunes de despliegue, como errores en la creación de tablas, columnas faltantes, índices no creados, o restricciones de clave primaria incorrectas.

**Conclusión sobre la Verificación de Despliegue con Metadatos:**

En resumen, el texto te presenta un ejemplo práctico de cómo **`information_schema` puede ser utilizado para crear scripts de verificación de despliegue**.  Esta consulta proporciona una forma rápida y automatizada de **asegurar que los cambios en el esquema de la base de datos se han aplicado correctamente** durante las ventanas de mantenimiento.  Al comparar los metadatos antes y después del despliegue, puedes **confirmar la integridad del esquema** y reducir el riesgo de problemas posteriores causados por despliegues incompletos o incorrectos.  Este enfoque es una práctica fundamental para mantener la calidad y estabilidad de las bases de datos en entornos de producción.


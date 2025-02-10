### Cláusulas de Consulta

Una consulta SQL (también conocida como sentencia `SELECT`) está compuesta por varias cláusulas. Aunque solo una de ellas es obligatoria al usar MySQL (la cláusula `SELECT`), generalmente incluirás al menos dos o tres de las seis cláusulas disponibles. La siguiente tabla muestra las diferentes cláusulas y sus propósitos:

#### Tabla 3-1: Cláusulas de Consulta

| Nombre de la Cláusula | Propósito                                                                           |
|-----------------------|-------------------------------------------------------------------------------------|
| **SELECT**            | Determina qué columnas incluir en el conjunto de resultados de la consulta          |
| **FROM**              | Identifica las tablas de las que se recuperarán los datos y cómo deben unirse       |
| **WHERE**             | Filtra los datos no deseados                                                        |
| **GROUP BY**          | Se utiliza para agrupar filas según valores comunes en las columnas                 |
| **HAVING**            | Filtra los grupos no deseados                                                       |
| **ORDER BY**          | Ordena las filas del conjunto de resultados final por una o más columnas            |

### Uso de las Cláusulas de Consulta

#### SELECT
- **Propósito**: Define las columnas que deseas recuperar.
- **Ejemplo**:
  ```sql
  SELECT first_name, last_name
  FROM customer;
  ```

#### FROM
- **Propósito**: Especifica las tablas de las cuales deseas recuperar los datos.
- **Ejemplo**:
  ```sql
  SELECT first_name, last_name
  FROM customer;
  ```

#### WHERE
- **Propósito**: Filtra las filas que deseas recuperar según una condición.
- **Ejemplo**:
  ```sql
  SELECT first_name, last_name
  FROM customer
  WHERE country = 'USA';
  ```

#### GROUP BY
- **Propósito**: Agrupa filas que tienen valores iguales en columnas especificadas para realizar funciones de agregado.
- **Ejemplo**:
  ```sql
  SELECT country, COUNT(*)
  FROM customer
  GROUP BY country;
  ```

#### HAVING
- **Propósito**: Filtra grupos de resultados después de que se haya aplicado `GROUP BY`.
- **Ejemplo**:
  ```sql
  SELECT country, COUNT(*)
  FROM customer
  GROUP BY country
  HAVING COUNT(*) > 10;
  ```

#### ORDER BY
- **Propósito**: Ordena los resultados según una o más columnas.
- **Ejemplo**:
  ```sql
  SELECT first_name, last_name
  FROM customer
  ORDER BY last_name ASC, first_name DESC;
  ```

Estas cláusulas son parte de la especificación ANSI para SQL y se utilizan para estructurar y refinar las consultas para obtener los datos deseados de una manera eficiente y precisa.

Espero que esta explicación te ayude a entender mejor las diferentes partes de una consulta `SELECT`. Si tienes más preguntas o necesitas más detalles, no dudes en decírmelo. ¡Estoy aquí para ayudarte!
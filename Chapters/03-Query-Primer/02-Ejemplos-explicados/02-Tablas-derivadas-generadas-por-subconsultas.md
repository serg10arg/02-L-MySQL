
### Desglose de la Sentencia SQL

```sql
SELECT CONCAT(cust.last_name, ', ', cust.first_name) full_name
FROM
  (SELECT first_name, last_name, email
   FROM customer
   WHERE first_name = 'JESSIE'
  ) cust;
```

#### Partes de la Sentencia

1. **Subconsulta**:
   ```sql
   (SELECT first_name, last_name, email
    FROM customer
    WHERE first_name = 'JESSIE')
   ```
    - Esta subconsulta selecciona las columnas `first_name`, `last_name` y `email` de la tabla `customer`.
    - Filtra los resultados para incluir solo las filas donde el `first_name` es `'JESSIE'`.
    - Los resultados de esta subconsulta se tratan como una tabla derivada temporal y se les da el alias `cust`.

2. **Consulta Principal**:
   ```sql
   SELECT CONCAT(cust.last_name, ', ', cust.first_name) full_name
   FROM
     (subconsulta) cust;
   ```
    - La consulta principal selecciona una expresión que concatena el `last_name` y `first_name` de la tabla derivada `cust`.
    - Utiliza la función `CONCAT` para combinar el `last_name` y `first_name`, separándolos con una coma y un espacio.
    - Asigna el resultado concatenado a una nueva columna llamada `full_name`.
    - La `FROM` cláusula referencia la tabla derivada `cust` generada por la subconsulta.

### Ejemplo de Resultados

Supongamos que la tabla `customer` contiene los siguientes datos:

| first_name | last_name | email              |
|------------|-----------|--------------------|
| JESSIE     | BANKS     | jessie.banks@mail.com |
| JESSIE     | MILAM     | jessie.milam@mail.com |
| JOHN       | DOE       | john.doe@mail.com     |

Dado que la subconsulta filtra los datos para incluir solo las filas donde el `first_name` es `'JESSIE'`, el resultado de la subconsulta sería:

| first_name | last_name | email              |
|------------|-----------|--------------------|
| JESSIE     | BANKS     | jessie.banks@mail.com |
| JESSIE     | MILAM     | jessie.milam@mail.com |

La consulta principal luego concatenaría los nombres y apellidos de los resultados de la subconsulta:

| full_name      |
|----------------|
| BANKS, JESSIE  |
| MILAM, JESSIE  |

### Explicación del Resultado

- **`full_name`**: Muestra los nombres completos concatenados en el formato `Apellido, Nombre`.

Esta sentencia SQL es útil cuando deseas crear una lista de nombres completos a partir de los datos de `first_name` y `last_name` y filtrar los resultados según ciertos criterios, como el nombre `first_name = 'JESSIE'`.


### Construcción de una Condición en SQL

Una condición en SQL se compone de una o más expresiones combinadas con uno o más operadores. Vamos a desglosar los componentes y operadores que puedes utilizar para construir una condición.

#### Componentes de una Condición

1. **Expresiones**:
   - **Número**: Un valor numérico.
      - Ejemplo: `5`
   - **Columna en una tabla o vista**: Un nombre de columna.
      - Ejemplo: `first_name`
   - **Cadena literal**: Un valor de texto.
      - Ejemplo: `'Maple Street'`
   - **Función integrada**: Una función proporcionada por SQL.
      - Ejemplo: `CONCAT('Learning', ' ', 'SQL')`
   - **Subconsulta**: Una consulta dentro de otra consulta.
      - Ejemplo: `(SELECT MAX(salary) FROM employees)`
   - **Lista de expresiones**: Un conjunto de valores.
      - Ejemplo: `('Boston', 'New York', 'Chicago')`

#### Operadores Utilizados en Condiciones

1. **Operadores de Comparación**:
   - **Igual a**: `=`
      - Ejemplo: `first_name = 'STEVEN'`
   - **Diferente de**: `!=` o `<>`
      - Ejemplo: `first_name != 'STEVEN'`
   - **Menor que**: `<`
      - Ejemplo: `salary < 50000`
   - **Mayor que**: `>`
      - Ejemplo: `salary > 50000`
   - **LIKE**: Compara un patrón de texto.
      - Ejemplo: `last_name LIKE 'S%'`
   - **IN**: Comprueba si un valor está en una lista.
      - Ejemplo: `city IN ('Boston', 'New York', 'Chicago')`
   - **BETWEEN**: Comprueba si un valor está en un rango.
      - Ejemplo: `salary BETWEEN 40000 AND 60000`

2. **Operadores Aritméticos**:
   - **Suma**: `+`
      - Ejemplo: `salary + 5000`
   - **Resta**: `-`
      - Ejemplo: `salary - 5000`
   - **Multiplicación**: `*`
      - Ejemplo: `salary * 1.1`
   - **División**: `/`
      - Ejemplo: `salary / 2`

### Ejemplos de Condiciones

Aquí tienes algunos ejemplos de cómo combinar expresiones y operadores para construir condiciones:

1. **Comparación de Columnas**:
   ```sql
   WHERE age > 30
   ```

2. **Uso de Funciones Integradas**:
   ```sql
   WHERE UPPER(first_name) = 'STEVEN'
   ```

3. **Uso de Subconsultas**:
   ```sql
   WHERE salary > (SELECT AVG(salary) FROM employees)
   ```

4. **Condiciones con Listas**:
   ```sql
   WHERE city IN ('Boston', 'New York', 'Chicago')
   ```

5. **Condiciones con Rango**:
   ```sql
   WHERE hire_date BETWEEN '2020-01-01' AND '2022-12-31'
   ```

6. **Combinación de Condiciones**:
   ```sql
   WHERE (first_name = 'STEVEN' OR last_name = 'YOUNG') AND create_date > '2006-01-01'
   ```

Estos ejemplos demuestran cómo puedes combinar expresiones y operadores para crear diversas condiciones en tus consultas SQL.
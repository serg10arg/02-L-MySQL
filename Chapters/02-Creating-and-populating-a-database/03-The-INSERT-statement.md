### La Sentencia `INSERT`

Ahora que todos los elementos están en su lugar, es hora de agregar algunos datos. La siguiente sentencia crea una fila en la tabla `person` para William Turner:

```sql
mysql> INSERT INTO person
    -> (person_id, fname, lname, eye_color, birth_date)
    -> VALUES (NULL, 'William', 'Turner', 'BR', '1972-05-27');
```

**Resultado**:
```
Query OK, 1 row affected (0.22 sec)
```

El mensaje "Query OK, 1 row affected" indica que la sintaxis de tu sentencia es correcta y que se ha agregado una fila a la base de datos. Puedes ver los datos recién agregados a la tabla emitiendo una sentencia `SELECT`:

```sql
mysql> SELECT person_id, fname, lname, birth_date
    -> FROM person;
```

**Resultado**:
```
+-----------+---------+--------+------------+
| person_id | fname   | lname  | birth_date |
+-----------+---------+--------+------------+
|         1 | William | Turner | 1972-05-27 |
+-----------+---------+--------+------------+
1 row in set (0.06 sec)
```

Como puedes ver, el servidor MySQL generó un valor de 1 para la clave primaria. Si hubiera más de una fila en la tabla, podrías agregar una cláusula `WHERE` para especificar que deseas recuperar datos solo para la fila con un valor de 1 para la columna `person_id`:

```sql
mysql> SELECT person_id, fname, lname, birth_date
    -> FROM person
    -> WHERE person_id = 1;
```

**Resultado**:
```
+-----------+---------+--------+------------+
| person_id | fname   | lname  | birth_date |
+-----------+---------+--------+------------+
|         1 | William | Turner | 1972-05-27 |
+-----------+---------+--------+------------+
1 row in set (0.00 sec)
```

Puedes usar cualquier columna en la tabla para buscar filas. Por ejemplo, para encontrar todas las filas con el valor 'Turner' en la columna `lname`:

```sql
mysql> SELECT person_id, fname, lname, birth_date
    -> FROM person
    -> WHERE lname = 'Turner';
```

**Resultado**:
```
+-----------+---------+--------+------------+
| person_id | fname   | lname  | birth_date |
+-----------+---------+--------+------------+
|         1 | William | Turner | 1972-05-27 |
+-----------+---------+--------+------------+
1 row in set (0.00 sec)
```

### Insertar Datos en `favorite_food`

William Turner también proporcionó información sobre sus tres comidas favoritas, así que aquí hay tres sentencias `INSERT` para almacenar sus preferencias alimenticias:

```sql
mysql> INSERT INTO favorite_food (person_id, food)
    -> VALUES (1, 'pizza');
Query OK, 1 row affected (0.01 sec)

mysql> INSERT INTO favorite_food (person_id, food)
    -> VALUES (1, 'cookies');
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO favorite_food (person_id, food)
    -> VALUES (1, 'nachos');
Query OK, 1 row affected (0.01 sec)
```

### Consultar las Comidas Favoritas de William en Orden Alfabético

Aquí tienes una consulta que recupera las comidas favoritas de William en orden alfabético usando una cláusula `ORDER BY`:

```sql
mysql> SELECT food
    -> FROM favorite_food
    -> WHERE person_id = 1
    -> ORDER BY food;
```

**Resultado**:
```
+---------+
| food    |
+---------+
| cookies |
| nachos  |
| pizza   |
+---------+
3 rows in set (0.02 sec)
```

### Agregar Más Personas

Para que William no se sienta solo, puedes ejecutar otra sentencia `INSERT` para agregar a Susan Smith a la tabla `person`:

```sql
mysql> INSERT INTO person
    -> (person_id, fname, lname, eye_color, birth_date, street, city, state, country, postal_code)
    -> VALUES (NULL, 'Susan', 'Smith', 'BL', '1975-11-02', '23 Maple St.', 'Arlington', 'VA', 'USA', '20220');
```

**Resultado**:
```
Query OK, 1 row affected (0.01 sec)
```

Si consultas la tabla nuevamente, verás que la fila de Susan ha sido asignada con el valor 2 para su clave primaria:

```sql
mysql> SELECT person_id, fname, lname, birth_date
    -> FROM person;
```

**Resultado**:
```
+-----------+---------+--------+------------+
| person_id | fname   | lname  | birth_date |
+-----------+---------+--------+------------+
|         1 | William | Turner | 1972-05-27 |
|         2 | Susan   | Smith  | 1975-11-02 |
+-----------+---------+--------+------------+
2 rows in set (0.00 sec)
```


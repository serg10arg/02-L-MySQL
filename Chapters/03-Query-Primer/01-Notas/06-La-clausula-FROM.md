### La Cláusula `FROM`

Hasta ahora, has visto consultas cuyas cláusulas `FROM` contienen una sola tabla. Aunque la mayoría de los libros de SQL definen la cláusula `FROM` como una lista de una o más tablas, es útil ampliar esta definición:

**La cláusula `FROM` define las tablas utilizadas por una consulta, junto con los medios para vincular las tablas entre sí.**

### Tipos de Tablas

1. **Tablas Permanentes**: Son las tablas creadas utilizando la sentencia `CREATE TABLE`.
2. **Tablas Derivadas**: Filas devueltas por una subconsulta y mantenidas en memoria.
3. **Tablas Temporales**: Datos volátiles mantenidos en memoria.
4. **Tablas Virtuales**: Tablas creadas utilizando la sentencia `CREATE VIEW`.

### Tablas Derivadas

Una subconsulta es una consulta contenida dentro de otra consulta. Las subconsultas están rodeadas por paréntesis y pueden encontrarse en varias partes de una sentencia `SELECT`. Dentro de la cláusula `FROM`, una subconsulta genera una tabla derivada visible desde todas las demás cláusulas de la consulta y puede interactuar con otras tablas nombradas en la cláusula `FROM`.

#### Ejemplo:

```sql
mysql> SELECT CONCAT(cust.last_name, ', ', cust.first_name) full_name
    -> FROM
    ->  (SELECT first_name, last_name, email
    ->   FROM customer
    ->   WHERE first_name = 'JESSIE'
    ->  ) cust;
```

**Resultado**:
```
+---------------+
| full_name     |
+---------------+
| BANKS, JESSIE |
| MILAM, JESSIE |
+---------------+
2 rows in set (0.00 sec)
```

### Tablas Temporales

Todas las bases de datos relacionales permiten definir tablas temporales. Estas tablas se parecen a las tablas permanentes, pero cualquier dato insertado en una tabla temporal desaparecerá en algún momento (generalmente al final de una transacción o cuando se cierra la sesión de la base de datos).

#### Ejemplo:

```sql
mysql> CREATE TEMPORARY TABLE actors_j
    ->  (actor_id SMALLINT(5),
    ->   first_name VARCHAR(45),
    ->   last_name VARCHAR(45));
Query OK, 0 rows affected (0.00 sec)

mysql> INSERT INTO actors_j
    -> SELECT actor_id, first_name, last_name
    -> FROM actor
    -> WHERE last_name LIKE 'J%';
Query OK, 7 rows affected (0.03 sec)

mysql> SELECT * FROM actors_j;
```

**Resultado**:
```
+----------+------------+-----------+
| actor_id | first_name | last_name |
+----------+------------+-----------+
|      119 | WARREN     | JACKMAN   |
|      131 | JANE       | JACKMAN   |
|        8 | MATTHEW    | JOHANSSON |
|       64 | RAY        | JOHANSSON |
|      146 | ALBERT     | JOHANSSON |
|       82 | WOODY      | JOLIE     |
|       43 | KIRK       | JOVOVICH  |
+----------+------------+-----------+
7 rows in set (0.00 sec)
```

Estas siete filas se mantienen temporalmente en memoria y desaparecerán después de que se cierre tu sesión.

### Vistas

Una vista es una consulta almacenada en el diccionario de datos. Parece y actúa como una tabla, pero no hay datos asociados con una vista (por eso se le llama una tabla virtual). Cuando emites una consulta contra una vista, tu consulta se fusiona con la definición de la vista para crear una consulta final que se ejecuta.

#### Ejemplo:

```sql
mysql> CREATE VIEW cust_vw AS
    -> SELECT customer_id, first_name, last_name, active
    -> FROM customer;
Query OK, 0 rows affected (0.12 sec)

mysql> SELECT first_name, last_name
    -> FROM cust_vw
    -> WHERE active = 0;
```

**Resultado**:
```
+------------+-----------+
| first_name | last_name |
+------------+-----------+
| SANDRA     | MARTIN    |
| JUDITH     | COX       |
| SHEILA     | WELLS     |
| ERICA      | MATTHEWS  |
| HEIDI      | LARSON    |
| PENNY      | NEAL      |
| KENNETH    | GOODEN    |
| HARRY      | ARCE      |
| NATHAN     | RUNYON    |
| THEODORE   | CULP      |
| MAURICE    | CRAWLEY   |
| BEN        | EASTER    |
| CHRISTIAN  | JUNG      |
| JIMMIE     | EGGLESTON |
| TERRANCE   | ROUSH     |
+------------+-----------+
15 rows in set (0.00 sec)
```

### Uso de las Vistas

Las vistas se crean por varias razones, incluyendo ocultar columnas de los usuarios y simplificar diseños complejos de bases de datos.

Espero que esta explicación te ayude a comprender mejor la cláusula `FROM` y los diferentes tipos de tablas que puedes utilizar en tus consultas. Si tienes más preguntas o necesitas más detalles, no dudes en decírmelo. ¡Estoy aquí para ayudarte!
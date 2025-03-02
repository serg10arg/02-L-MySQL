### Población y Modificación de Tablas

Con las tablas `person` y `favorite_food` en su lugar, ahora puedes comenzar a explorar las cuatro sentencias de datos SQL: `INSERT`, `UPDATE`, `DELETE` y `SELECT`.

#### Insertar Datos

Dado que aún no hay datos en las tablas `person` y `favorite_food`, la primera de las cuatro sentencias de datos SQL a explorar será la sentencia `INSERT`. Hay tres componentes principales en una sentencia `INSERT`:

1. **El nombre de la tabla en la que se agregará los datos**.
2. **Los nombres de las columnas en la tabla a poblar**.
3. **Los valores con los que se poblarán las columnas**.

No estás obligado a proporcionar datos para cada columna en la tabla (a menos que todas las columnas en la tabla hayan sido definidas como `NOT NULL`). En algunos casos, esas columnas que no se incluyen en la sentencia `INSERT` inicial recibirán un valor más tarde a través de una sentencia `UPDATE`. En otros casos, una columna puede nunca recibir un valor para una fila en particular de datos (como un pedido de cliente que se cancela antes de ser enviado, lo que hace que la columna `ship_date` sea inaplicable).

#### Generación de Datos Clave Numéricos

Antes de insertar datos en la tabla `person`, sería útil discutir cómo se generan los valores para las claves primarias numéricas. Además de elegir un número al azar, tienes un par de opciones:

1. **Mirar el valor más grande actualmente en la tabla y agregar uno**.
2. **Dejar que el servidor de la base de datos proporcione el valor para ti**.

Aunque la primera opción puede parecer válida, resulta problemática en un entorno multiusuario, ya que dos usuarios pueden mirar la tabla al mismo tiempo y generar el mismo valor para la clave primaria. En su lugar, todos los servidores de bases de datos en el mercado hoy en día proporcionan un método seguro y robusto para generar claves numéricas. En algunos servidores, como Oracle Database, se usa un objeto de esquema separado (llamado secuencia); en el caso de MySQL, sin embargo, simplemente necesitas activar la función de autoincremento para tu columna de clave primaria. Normalmente, harías esto al crear la tabla, pero hacerlo ahora proporciona la oportunidad de aprender otra sentencia de esquema SQL, `ALTER TABLE`, que se usa para modificar la definición de una tabla existente:

```sql
ALTER TABLE person MODIFY person_id SMALLINT UNSIGNED AUTO_INCREMENT;
```

Si estás ejecutando estas sentencias en tu base de datos, primero necesitarás deshabilitar la restricción de clave foránea en la tabla `favorite_food`, y luego volver a habilitar las restricciones cuando hayas terminado. La progresión de las sentencias sería:

```sql
SET foreign_key_checks = 0;
ALTER TABLE person MODIFY person_id SMALLINT UNSIGNED AUTO_INCREMENT;
SET foreign_key_checks = 1;
```

Esta sentencia redefine esencialmente la columna `person_id` en la tabla `person`. Si describes la tabla, ahora verás la función de autoincremento listada en la columna "Extra" para `person_id`:

```sql
mysql> DESC person;
+-------------+------------------------+------+-----+---------+-----------------+
| Field       | Type                   | Null | Key | Default | Extra           |
+-------------+------------------------+------+-----+---------+-----------------+
| person_id   | smallint(5) unsigned   | NO   | PRI | NULL    | auto_increment  |
| .           |                        |      |     |         |                 |
| .           |                        |      |     |         |                 |
| .           |                        |      |     |         |                 |
```

Cuando insertas datos en la tabla `person`, simplemente proporcionas un valor nulo para la columna `person_id`, y MySQL poblará la columna con el siguiente número disponible (por defecto, MySQL comienza en 1 para columnas de autoincremento).


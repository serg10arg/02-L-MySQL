
---

# Funciones de Cadena que Devuelven Cadenas

En algunos casos, necesitarás modificar cadenas existentes, ya sea extrayendo parte de la cadena o añadiendo texto adicional a la cadena. Cada servidor de bases de datos incluye múltiples funciones para ayudar con estas tareas. Antes de comenzar, una vez más restablezco los datos en la tabla `string_tbl`:

```sql
mysql> DELETE FROM string_tbl;
Query OK, 5 rows affected (0.00 sec)

mysql> INSERT INTO string_tbl (text_fld)
    -> VALUES ('This string was 29 characters');
Query OK, 1 row affected (0.01 sec)
```

Anteriormente en el capítulo, demostré el uso de la función `concat()` para ayudar a construir palabras que incluyen caracteres acentuados. La función `concat()` es útil en muchas otras situaciones, incluyendo cuando necesitas agregar caracteres adicionales a una cadena almacenada. Por ejemplo, el siguiente ejemplo modifica la cadena almacenada en la columna `text_fld` añadiendo una frase adicional al final:

```sql
mysql> UPDATE string_tbl
    -> SET text_fld = CONCAT(text_fld, ', but now it is longer');
Query OK, 1 row affected (0.03 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

El contenido de la columna `text_fld` ahora es el siguiente:

```sql
mysql> SELECT text_fld
    -> FROM string_tbl;
+-----------------------------------------------------+
| text_fld                                            |
+-----------------------------------------------------+
| This string was 29 characters, but now it is longer |
+-----------------------------------------------------+
1 row in set (0.00 sec)
```

Así, como todas las funciones que devuelven una cadena, puedes usar `concat()` para reemplazar los datos almacenados en una columna de caracteres.

Otro uso común para la función `concat()` es construir una cadena a partir de piezas individuales de datos. Por ejemplo, la siguiente consulta genera una cadena narrativa para cada cliente:

```sql
mysql> SELECT concat(first_name, ' ', last_name,
    ->   ' has been a customer since ', date(create_date)) cust_narrative
    -> FROM customer;
+---------------------------------------------------------+
| cust_narrative                                          |
+---------------------------------------------------------+
| MARY SMITH has been a customer since 2006-02-14         |
| PATRICIA JOHNSON has been a customer since 2006-02-14   |
| LINDA WILLIAMS has been a customer since 2006-02-14     |
| BARBARA JONES has been a customer since 2006-02-14      |
| ELIZABETH BROWN has been a customer since 2006-02-14    |
| JENNIFER DAVIS has been a customer since 2006-02-14     |
| MARIA MILLER has been a customer since 2006-02-14       |
| SUSAN WILSON has been a customer since 2006-02-14       |
| MARGARET MOORE has been a customer since 2006-02-14     |
| DOROTHY TAYLOR has been a customer since 2006-02-14     |
...
| RENE MCALISTER has been a customer since 2006-02-14     |
| EDUARDO HIATT has been a customer since 2006-02-14      |
| TERRENCE GUNDERSON has been a customer since 2006-02-14 |
| ENRIQUE FORSYTHE has been a customer since 2006-02-14   |
| FREDDIE DUGGAN has been a customer since 2006-02-14     |
| WADE DELVALLE has been a customer since 2006-02-14      |
| AUSTIN CINTRON has been a customer since 2006-02-14     |
+---------------------------------------------------------+
599 rows in set (0.00 sec)
```

La función `concat()` puede manejar cualquier expresión que devuelva una cadena e incluso convertirá números y fechas a formato de cadena, como lo evidencia la columna de fecha (`create_date`) utilizada como argumento. Aunque Oracle Database incluye la función `concat()`, solo aceptará dos argumentos de cadena, por lo que la consulta anterior no funcionará en Oracle. En su lugar, necesitarías usar el operador de concatenación (`||`) en lugar de una llamada a función, como en:

```sql
SELECT first_name || ' ' || last_name ||
 ' has been a customer since ' || date(create_date)) cust_narrative
FROM customer;
```

SQL Server no incluye una función `concat()`, por lo que necesitarías usar el mismo enfoque que la consulta anterior, excepto que usarías el operador de concatenación de SQL Server (`+`) en lugar de `||`.

Aunque `concat()` es útil para agregar caracteres al principio o al final de una cadena, también podrías necesitar agregar o reemplazar caracteres en el medio de una cadena. Los tres servidores de bases de datos proporcionan funciones para este propósito, pero todas son diferentes, por lo que demuestro la función de MySQL y luego muestro las funciones de los otros dos servidores.

MySQL incluye la función `insert()`, que toma cuatro argumentos: la cadena original, la posición en la que comenzar, el número de caracteres a reemplazar y la cadena de reemplazo. Dependiendo del valor del tercer argumento, la función puede usarse para insertar o reemplazar caracteres en una cadena. Con un valor de 0 para el tercer argumento, la cadena de reemplazo se inserta y cualquier carácter final se empuja hacia la derecha, como en:

```sql
mysql> SELECT INSERT('goodbye world', 9, 0, 'cruel ') string;
+---------------------+
| string              |
+---------------------+
| goodbye cruel world |
+---------------------+
1 row in set (0.00 sec)
```

En este ejemplo, todos los caracteres a partir de la posición 9 se empujan hacia la derecha y se inserta la cadena 'cruel'. Si el tercer argumento es mayor que cero, entonces ese número de caracteres se reemplaza con la cadena de reemplazo, como en:

```sql
mysql> SELECT INSERT('goodbye world', 1, 7, 'hello') string;
+-------------+
| string      |
+-------------+
| hello world |
+-------------+
1 row in set (0.00 sec)
```

Para este ejemplo, los primeros siete caracteres se reemplazan con la cadena 'hello'. Oracle Database no proporciona una única función con la flexibilidad de la función `insert()` de MySQL, pero Oracle sí proporciona la función `replace()`, que es útil para reemplazar una subcadena con otra. Aquí tienes el ejemplo anterior rehecho para usar `replace()`:

```sql
SELECT REPLACE('goodbye world', 'goodbye', 'hello')
FROM dual;
```

Todas las instancias de la cadena 'goodbye' serán reemplazadas por la cadena 'hello', resultando en la cadena 'hello world'. La función `replace()` reemplazará cada instancia de la cadena de búsqueda con la cadena de reemplazo, por lo que debes tener cuidado de no terminar con más reemplazos de los anticipados.

SQL Server también incluye una función `replace()` con la misma funcionalidad que la de Oracle, pero SQL Server también incluye una función llamada `stuff()` con una funcionalidad similar a la función `insert()` de MySQL. Aquí tienes un ejemplo:

```sql
SELECT STUFF('hello world', 1, 5, 'goodbye cruel')
```

Cuando se ejecuta, se eliminan cinco caracteres a partir de la posición 1 y luego se inserta la cadena 'goodbye cruel' en la posición inicial, resultando en la cadena 'goodbye cruel world'.

Además de insertar caracteres en una cadena, podrías necesitar extraer una subcadena de una cadena. Para este propósito, los tres servidores incluyen la función `substring()` (aunque la versión de Oracle Database se llama `substr()`), que extrae un número especificado de caracteres comenzando en una posición especificada. El siguiente ejemplo extrae cinco caracteres de una cadena comenzando en la novena posición:

```sql
mysql> SELECT SUBSTRING('goodbye cruel world', 9, 5);
+----------------------------------------+
| SUBSTRING('goodbye cruel world', 9, 5) |
+----------------------------------------+
| cruel                                  |
+----------------------------------------+
1 row in set (0.00 sec)
```

Además de las funciones demostradas aquí, los tres servidores incluyen muchas más funciones integradas para manipular datos de cadena. Mientras que muchas de ellas están diseñadas para propósitos muy específicos, como generar el equivalente en cadena de números octales o hexadecimales, hay muchas otras funciones de uso general, así como funciones que eliminan o agregan espacios finales. Para más información, consulta la guía de referencia SQL de tu servidor, o una guía de referencia SQL de propósito general como **"SQL in a Nutshell"** (O’Reilly).

---


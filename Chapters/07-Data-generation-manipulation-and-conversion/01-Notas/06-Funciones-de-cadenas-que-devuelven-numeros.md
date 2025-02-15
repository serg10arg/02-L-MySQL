
---

# Manipulación de Cadenas

Cada servidor de bases de datos incluye muchas funciones integradas para manipular cadenas. Esta sección explora dos tipos de funciones de cadena: aquellas que devuelven números y aquellas que devuelven cadenas. Antes de comenzar, restablezco los datos en la tabla `string_tbl` a lo siguiente:

```sql
mysql> DELETE FROM string_tbl;
Query OK, 1 row affected (0.02 sec)

mysql> INSERT INTO string_tbl (char_fld, vchar_fld, text_fld)
    -> VALUES ('This string is 28 characters',
    ->   'This string is 28 characters',
    ->   'This string is 28 characters');
Query OK, 1 row affected (0.00 sec)
```

## Funciones de cadena que devuelven números

De las funciones de cadena que devuelven números, una de las más comúnmente utilizadas es la función `length()`, que devuelve el número de caracteres en la cadena (los usuarios de SQL Server deberán usar la función `len()`). La siguiente consulta aplica la función `length()` a cada columna en la tabla `string_tbl`:

```sql
mysql> SELECT LENGTH(char_fld) char_length,
    ->   LENGTH(vchar_fld) varchar_length,
    ->   LENGTH(text_fld) text_length
    -> FROM string_tbl;
+-------------+----------------+-------------+
| char_length | varchar_length | text_length |
+-------------+----------------+-------------+
|          28 |             28 |          28 |
+-------------+----------------+-------------+
1 row in set (0.00 sec)
```

Aunque las longitudes de las columnas `varchar` y `text` son las esperadas, podrías haber esperado que la longitud de la columna `char` fuera 30, ya que te dije que las cadenas almacenadas en columnas `char` se rellenan con espacios a la derecha. Sin embargo, el servidor MySQL elimina los espacios finales de los datos `char` cuando se recuperan, por lo que verás los mismos resultados de todas las funciones de cadena independientemente del tipo de columna en la que se almacenen las cadenas.

Además de encontrar la longitud de una cadena, podrías querer encontrar la ubicación de una subcadena dentro de una cadena. Por ejemplo, si deseas encontrar la posición en la que aparece la cadena 'characters' en la columna `vchar_fld`, podrías usar la función `position()`, como se demuestra a continuación:

```sql
mysql> SELECT POSITION('characters' IN vchar_fld)
    -> FROM string_tbl;
+-------------------------------------+
| POSITION('characters' IN vchar_fld) |
+-------------------------------------+
|                                  19 |
+-------------------------------------+
1 row in set (0.12 sec)
```

Si la subcadena no puede ser encontrada, la función `position()` devuelve 0.

Para aquellos de ustedes que programan en un lenguaje como C o C++, donde el primer elemento de un arreglo está en la posición 0, recuerden que al trabajar con bases de datos, el primer carácter en una cadena está en la posición 1. Un valor de retorno de 0 de `instr()` indica que la subcadena no pudo ser encontrada, no que la subcadena fue encontrada en la primera posición de la cadena.

Si deseas iniciar tu búsqueda en un lugar diferente al primer carácter de tu cadena objetivo, necesitarás usar la función `locate()`, que es similar a la función `position()` excepto que permite un tercer parámetro opcional, que se utiliza para definir la posición de inicio de la búsqueda. La función `locate()` también es propietaria, mientras que la función `position()` es parte del estándar SQL:2003. Aquí tienes un ejemplo que solicita la posición de la cadena 'is' comenzando en el quinto carácter en la columna `vchar_fld`:

```sql
mysql> SELECT LOCATE('is', vchar_fld, 5)
    -> FROM string_tbl;
+----------------------------+
| LOCATE('is', vchar_fld, 5) |
+----------------------------+
|                         13 |
+----------------------------+
1 row in set (0.02 sec)
```

Oracle Database no incluye las funciones `position()` o `locate()`, pero sí incluye la función `instr()`, que imita la función `position()` cuando se le proporcionan dos argumentos e imita la función `locate()` cuando se le proporcionan tres argumentos. SQL Server tampoco incluye las funciones `position()` o `locate()`, pero sí incluye la función `charindex()`, que también acepta dos o tres argumentos similar a la función `instr()` de Oracle.

Otra función que toma cadenas como argumentos y devuelve números es la función de comparación de cadenas `strcmp()`. `strcmp()`, que solo es implementada por MySQL y no tiene análogo en Oracle Database o SQL Server, toma dos cadenas como argumentos y devuelve una de las siguientes opciones:
- -1 si la primera cadena viene antes de la segunda cadena en el orden de clasificación.
- 0 si las cadenas son idénticas.
- 1 si la primera cadena viene después de la segunda cadena en el orden de clasificación.

Para ilustrar cómo funciona la función, primero muestro el orden de clasificación de cinco cadenas usando una consulta y luego muestro cómo se comparan las cadenas entre sí usando `strcmp()`. Aquí están las cinco cadenas que inserto en la tabla `string_tbl`:

```sql
mysql> DELETE FROM string_tbl;
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO string_tbl(vchar_fld)
    -> VALUES ('abcd'),
    ->        ('xyz'),
    ->        ('QRSTUV'),
    ->        ('qrstuv'),
    ->        ('12345');
Query OK, 5 rows affected (0.05 sec)
Records: 5  Duplicates: 0  Warnings: 0
```

Aquí están las cinco cadenas en su orden de clasificación:

```sql
mysql> SELECT vchar_fld
    -> FROM string_tbl
    -> ORDER BY vchar_fld;
+-----------+
| vchar_fld |
+-----------+
| 12345     |
| abcd      |
| QRSTUV    |
| qrstuv    |
| xyz       |
+-----------+
5 rows in set (0.00 sec)
```

La siguiente consulta realiza seis comparaciones entre las cinco cadenas diferentes:

```sql
mysql> SELECT STRCMP('12345','12345') 12345_12345,
    ->   STRCMP('abcd','xyz') abcd_xyz,
    ->   STRCMP('abcd','QRSTUV') abcd_QRSTUV,
    ->   STRCMP('qrstuv','QRSTUV') qrstuv_QRSTUV,
    ->   STRCMP('12345','xyz') 12345_xyz,
    ->   STRCMP('xyz','qrstuv') xyz_qrstuv;
+-------------+----------+-------------+---------------+-----------+------------+
| 12345_12345 | abcd_xyz | abcd_QRSTUV | qrstuv_QRSTUV | 12345_xyz | xyz_qrstuv |
+-------------+----------+-------------+---------------+-----------+------------+
|           0 |       -1 |          -1 |             0 |        -1 |          1 |
+-------------+----------+-------------+---------------+-----------+------------+
1 row in set (0.00 sec)
```

La primera comparación devuelve 0, lo cual es de esperar ya que comparé una cadena consigo misma. La cuarta comparación también devuelve 0, lo cual es un poco sorprendente, ya que las cadenas están compuestas por las mismas letras, con una cadena completamente en mayúsculas y la otra completamente en minúsculas. La razón de este resultado es que la función `strcmp()` de MySQL no distingue entre mayúsculas y minúsculas, algo que debes recordar al usar la función. Las otras cuatro comparaciones devuelven -1 o 1 dependiendo de si la primera cadena viene antes o después de la segunda cadena en el orden de clasificación. Por ejemplo, `strcmp('abcd','xyz')` devuelve -1, ya que la cadena 'abcd' viene antes de la cadena 'xyz'.

Junto con la función `strcmp()`, MySQL también te permite usar los operadores `like` y `regexp` para comparar cadenas en la cláusula `select`. Tales comparaciones devolverán 1 (para verdadero) o 0 (para falso). Por lo tanto, estos operadores te permiten construir expresiones que devuelven un número, muy parecido a las funciones descritas en esta sección. Aquí tienes un ejemplo usando `like`:

```sql
mysql> SELECT name, name LIKE '%y' ends_in_y
    -> FROM category;
+-------------+-----------+
| name        | ends_in_y |
+-------------+-----------+
| Action      |         0 |
| Animation   |         0 |
| Children    |         0 |
| Classics    |         0 |
| Comedy      |         1 |
| Documentary |         1 |
| Drama       |         0 |
| Family      |         1 |
| Foreign     |         0 |
| Games       |         0 |
| Horror      |         0 |
| Music       |         0 |
| New         |         0 |
| Sci-Fi      |         0 |
| Sports      |         0 |
| Travel      |         0 |
+-------------+-----------+
16 rows in set (0.00 sec)
```

Este ejemplo recupera todos los nombres de categoría, junto con una expresión que devuelve 1 si el nombre termina en "y" o 0 en caso contrario.

Si deseas realizar coincidencias de patrones más complejas, puedes usar el operador `regexp`, como se demuestra a continuación:

```sql
mysql> SELECT name, name REGEXP 'y$' ends_in_y
    -> FROM category;
+-------------+-----------+
| name        | ends_in_y |
+-------------+-----------+
| Action      |         0 |
| Animation   |         0 |
| Children    |         0 |
| Classics    |         0 |
| Comedy      |         1 |
| Documentary |         1 |
| Drama       |         0 |
| Family      |         1 |
| Foreign     |         0 |
| Games       |         0 |
| Horror      |         0 |
| Music       |         0 |
| New         |         0 |
| Sci-Fi      |         0 |
| Sports      |         0 |
| Travel      |         0 |
+-------------+-----------+
16 rows in set (0.00 sec)
```

La segunda columna de esta consulta devuelve 1 si el valor almacenado en la columna `name` coincide con la expresión regular dada.

Los usuarios de Microsoft SQL Server y Oracle Database pueden lograr resultados similares construyendo expresiones `case`, que describiré en detalle en el Capítulo 11.

---


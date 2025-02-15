
---

# Generación de Cadenas

La forma más sencilla de llenar una columna de caracteres es encerrar una cadena entre comillas, como en los siguientes ejemplos:

```sql
mysql> INSERT INTO string_tbl (char_fld, vchar_fld, text_fld)
    -> VALUES ('This is char data',
    ->   'This is varchar data',
    ->   'This is text data');
Query OK, 1 row affected (0.00 sec)
```

Al insertar datos de cadena en una tabla, recuerda que si la longitud de la cadena excede el tamaño máximo para la columna de caracteres (ya sea el máximo designado o el máximo permitido para el tipo de datos), el servidor lanzará una excepción. Aunque este es el comportamiento predeterminado para los tres servidores, puedes configurar MySQL y SQL Server para truncar silenciosamente la cadena en lugar de lanzar una excepción. Para demostrar cómo maneja MySQL esta situación, la siguiente declaración `update` intenta modificar la columna `vchar_fld`, cuya longitud máxima se define como 30, con una cadena que tiene 46 caracteres de longitud:

```sql
mysql> UPDATE string_tbl
    -> SET vchar_fld = 'This is a piece of extremely long varchar data';
ERROR 1406 (22001): Data too long for column 'vchar_fld' at row 1
```

Desde MySQL 6.0, el comportamiento predeterminado ahora es el modo "estricto", lo que significa que se lanzan excepciones cuando surgen problemas, mientras que en versiones anteriores del servidor, la cadena se habría truncado y se habría emitido una advertencia. Si prefieres que el motor trunque la cadena y emita una advertencia en lugar de generar una excepción, puedes optar por estar en modo ANSI. El siguiente ejemplo muestra cómo verificar en qué modo te encuentras y luego cómo cambiar el modo utilizando el comando `set`:

```sql
mysql> SELECT @@session.sql_mode;
+----------------------------------------------------------------+
| @@session.sql_mode                                             |
+----------------------------------------------------------------+
| STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION                     |
+----------------------------------------------------------------+
1 row in set (0.00 sec)

mysql> SET sql_mode='ansi';
Query OK, 0 rows affected (0.08 sec)

mysql> SELECT @@session.sql_mode;
+--------------------------------------------------------------------------------+
| @@session.sql_mode                                                             |
+--------------------------------------------------------------------------------+
| REAL_AS_FLOAT,PIPES_AS_CONCAT,ANSI_QUOTES,IGNORE_SPACE,ONLY_FULL_GROUP_BY,ANSI |
+--------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

Si vuelves a ejecutar la declaración `update` anterior, verás que la columna ha sido modificada, pero se genera la siguiente advertencia:

```sql
mysql> SHOW WARNINGS;
+---------+------+------------------------------------------------+
| Level   | Code | Message                                        |
+---------+------+------------------------------------------------+
| Warning | 1265 | Data truncated for column 'vchar_fld' at row 1 |
+---------+------+------------------------------------------------+
1 row in set (0.00 sec)
```

Si recuperas la columna `vchar_fld`, verás que la cadena ha sido truncada:

```sql
mysql> SELECT vchar_fld
    -> FROM string_tbl;
+--------------------------------+
| vchar_fld                      |
+--------------------------------+
| This is a piece of extremely l |
+--------------------------------+
1 row in set (0.05 sec)
```

Como puedes ver, solo los primeros 30 caracteres de la cadena de 46 caracteres llegaron a la columna `vchar_fld`. La mejor manera de evitar el truncamiento de cadenas (o excepciones, en el caso de Oracle Database o MySQL en modo estricto) al trabajar con columnas `varchar` es establecer el límite superior de una columna a un valor lo suficientemente alto como para manejar las cadenas más largas que podrían almacenarse en la columna (teniendo en cuenta que el servidor asigna solo el espacio necesario para almacenar la cadena, por lo que no es derrochador establecer un límite superior alto para las columnas `varchar`).

---



---

# Incluyendo Comillas Simples

Dado que las cadenas están delimitadas por comillas simples, deberás estar atento a las cadenas que incluyan comillas simples o apóstrofes. Por ejemplo, no podrás insertar la siguiente cadena porque el servidor pensará que el apóstrofe en la palabra *doesn't* marca el final de la cadena:

```sql
UPDATE string_tbl
SET text_fld = 'This string doesn't work';
```

Para hacer que el servidor ignore el apóstrofe en la palabra *doesn't*, necesitarás agregar un carácter de escape a la cadena para que el servidor trate el apóstrofe como cualquier otro carácter en la cadena. Los tres servidores te permiten escapar una comilla simple añadiendo otra comilla simple directamente antes, como en:

```sql
mysql> UPDATE string_tbl
    -> SET text_fld = 'This string didn''t work, but it does now';
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

Los usuarios de Oracle Database y MySQL también pueden optar por escapar una comilla simple añadiendo un carácter de barra invertida inmediatamente antes, como en:

```sql
UPDATE string_tbl SET text_fld =
'This string didn\'t work, but it does now'
```

Si recuperas una cadena para usarla en un campo de pantalla o informe, no necesitas hacer nada especial para manejar las comillas incrustadas:

```sql
mysql> SELECT text_fld
    -> FROM string_tbl;
+------------------------------------------+
| text_fld                                 |
+------------------------------------------+
| This string didn't work, but it does now |
+------------------------------------------+
1 row in set (0.00 sec)
```

Sin embargo, si estás recuperando la cadena para añadirla a un archivo que otro programa leerá, es posible que desees incluir el carácter de escape como parte de la cadena recuperada. Si estás utilizando MySQL, puedes usar la función integrada `quote()`, que coloca comillas alrededor de toda la cadena y añade caracteres de escape a cualquier comilla simple/apóstrofe dentro de la cadena. Aquí está cómo se ve nuestra cadena cuando se recupera mediante la función `quote()`:

```sql
mysql> SELECT quote(text_fld)
    -> FROM string_tbl;
+---------------------------------------------+
| QUOTE(text_fld)                             |
+---------------------------------------------+
| 'This string didn\'t work, but it does now' |
+---------------------------------------------+
1 row in set (0.04 sec)
```

Al recuperar datos para la exportación de datos, es posible que desees usar la función `quote()` para todas las columnas de caracteres no generadas por el sistema, como una columna `customer_notes`.

---


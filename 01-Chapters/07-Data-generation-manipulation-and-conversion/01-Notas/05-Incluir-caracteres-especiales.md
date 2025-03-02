
---

# Incluir Caracteres Especiales

Si tu aplicación es multinacional, podrías encontrarte trabajando con cadenas que incluyan caracteres que no aparecen en tu teclado. Al trabajar con los idiomas francés y alemán, por ejemplo, podrías necesitar incluir caracteres acentuados como é y ö. Los servidores SQL Server y MySQL incluyen la función incorporada `char()` para que puedas construir cadenas a partir de cualquiera de los 255 caracteres en el conjunto de caracteres ASCII (los usuarios de Oracle Database pueden usar la función `chr()`). Para demostrarlo, el siguiente ejemplo recupera una cadena escrita y su equivalente construida a través de caracteres individuales:

```sql
mysql> SELECT 'abcdefg', CHAR(97,98,99,100,101,102,103);
+---------+--------------------------------+
| abcdefg | CHAR(97,98,99,100,101,102,103) |
+---------+--------------------------------+
| abcdefg | abcdefg                        |
+---------+--------------------------------+
1 row in set (0.01 sec)
```

Así, el 97º carácter en el conjunto de caracteres ASCII es la letra *a*. Aunque los caracteres mostrados en el ejemplo anterior no son especiales, los siguientes ejemplos muestran la ubicación de los caracteres acentuados junto con otros caracteres especiales, como los símbolos de moneda:

```sql
mysql> SELECT CHAR(128,129,130,131,132,133,134,135,136,137);
+-----------------------------------------------+
| CHAR(128,129,130,131,132,133,134,135,136,137) |
+-----------------------------------------------+
| Çüéâäàåçêë                                    |
+-----------------------------------------------+
1 row in set (0.01 sec)

mysql> SELECT CHAR(138,139,140,141,142,143,144,145,146,147);
+-----------------------------------------------+
| CHAR(138,139,140,141,142,143,144,145,146,147) |
+-----------------------------------------------+
| èïîìÄÅÉæÆô                                    |
+-----------------------------------------------+
1 row in set (0.01 sec)

mysql> SELECT CHAR(148,149,150,151,152,153,154,155,156,157);
+-----------------------------------------------+
| CHAR(148,149,150,151,152,153,154,155,156,157) |
+-----------------------------------------------+
| öòûùÿÖÜø£Ø                                    |
+-----------------------------------------------+
1 row in set (0.00 sec)

mysql> SELECT CHAR(158,159,160,161,162,163,164,165);
+---------------------------------------+
| CHAR(158,159,160,161,162,163,164,165) |
+---------------------------------------+
| ×ƒáíóúñÑ                              |
+---------------------------------------+
1 row in set (0.01 sec)
```

Estoy usando el conjunto de caracteres `utf8mb4` para los ejemplos en esta sección. Si tu sesión está configurada para un conjunto de caracteres diferente, verás un conjunto de caracteres diferente al mostrado aquí. Los mismos conceptos se aplican, pero deberás familiarizarte con el diseño de tu conjunto de caracteres para localizar caracteres específicos.

Construir cadenas carácter por carácter puede ser bastante tedioso, especialmente si solo unos pocos de los caracteres en la cadena están acentuados. Afortunadamente, puedes usar la función `concat()` para concatenar cadenas individuales, algunas de las cuales puedes escribir mientras que otras puedes generar a través de la función `char()`. Por ejemplo, lo siguiente muestra cómo construir la frase *danke schön* utilizando las funciones `concat()` y `char()`:

```sql
mysql> SELECT CONCAT('danke sch', CHAR(148), 'n');
+-------------------------------------+
| CONCAT('danke sch', CHAR(148), 'n') |
+-------------------------------------+
| danke schön                         |
+-------------------------------------+
1 row in set (0.00 sec)
```

Los usuarios de Oracle Database pueden usar el operador de concatenación (`||`) en lugar de la función `concat()`, como en:

```sql
SELECT 'danke sch' || CHR(148) || 'n'
FROM dual;
```

SQL Server no incluye una función `concat()`, por lo que necesitarás usar el operador de concatenación (`+`), como en:

```sql
SELECT 'danke sch' + CHAR(148) + 'n';
```

Si tienes un carácter y necesitas encontrar su equivalente ASCII, puedes usar la función `ascii()`, que toma el carácter más a la izquierda en la cadena y devuelve un número:

```sql
mysql> SELECT ASCII('ö');
+------------+
| ASCII('ö') |
+------------+
|        148 |
+------------+
1 row in set (0.00 sec)
```

Usando las funciones `char()`, `ascii()` y `concat()` (o los operadores de concatenación), deberías poder trabajar con cualquier idioma romano, incluso si estás utilizando un teclado que no incluye caracteres acentuados o especiales.

---


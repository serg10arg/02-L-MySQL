
---

# La Sintaxis de Join de ANSI

La notación utilizada a lo largo de este libro para unir tablas se introdujo en la versión SQL92 del estándar ANSI SQL. Todas las principales bases de datos (Oracle Database, Microsoft SQL Server, MySQL, IBM DB2 Universal Database y Sybase Adaptive Server) han adoptado la sintaxis de join de SQL92. Debido a que la mayoría de estos servidores han existido desde antes de la publicación de la especificación SQL92, todos incluyen también una sintaxis de join más antigua. Por ejemplo, todos estos servidores entenderían la siguiente variación de la consulta anterior:

```sql
mysql> SELECT c.first_name, c.last_name, a.address
    -> FROM customer c, address a
    -> WHERE c.address_id = a.address_id;
+------------+------------+------------------------------------+
| first_name | last_name  | address                            |
+------------+------------+------------------------------------+
| MARY       | SMITH      | 1913 Hanoi Way                     |
| PATRICIA   | JOHNSON    | 1121 Loja Avenue                   |
| LINDA      | WILLIAMS   | 692 Joliet Street                  |
| BARBARA    | JONES      | 1566 Inegl Manor                   |
| ELIZABETH  | BROWN      | 53 Idfu Parkway                    |
| JENNIFER   | DAVIS      | 1795 Santiago de Compostela Way    |
| MARIA      | MILLER     | 900 Santiago de Compostela Parkway |
| SUSAN      | WILSON     | 478 Joliet Way                     |
| MARGARET   | MOORE      | 613 Korolev Drive                  |
...
| TERRANCE   | ROUSH      | 42 Fontana Avenue                  |
| RENE       | MCALISTER  | 1895 Zhezqazghan Drive             |
| EDUARDO    | HIATT      | 1837 Kaduna Parkway                |
| TERRENCE   | GUNDERSON  | 844 Bucuresti Place                |
| ENRIQUE    | FORSYTHE   | 1101 Bucuresti Boulevard           |
| FREDDIE    | DUGGAN     | 1103 Quilmes Boulevard             |
| WADE       | DELVALLE   | 1331 Usak Boulevard                |
| AUSTIN     | CINTRON    | 1325 Fukuyama Street               |
+------------+------------+------------------------------------+
599 rows in set (0.00 sec)
```

Este método más antiguo de especificar uniones no incluye la subcláusula `on`; en su lugar, las tablas se nombran en la cláusula `from` separadas por comas, y las condiciones de unión se incluyen en la cláusula `where`. Aunque puedes decidir ignorar la sintaxis de SQL92 a favor de la sintaxis de unión antigua, la sintaxis de join de ANSI tiene las siguientes ventajas:
- Las condiciones de unión y las condiciones de filtro se separan en dos cláusulas diferentes (la subcláusula `on` y la cláusula `where`, respectivamente), lo que hace que una consulta sea más fácil de entender.
- Las condiciones de unión para cada par de tablas se incluyen en su propia cláusula `on`, lo que reduce la probabilidad de que se omita parte de una unión por error.
- Las consultas que usan la sintaxis de unión SQL92 son portátiles entre servidores de bases de datos, mientras que la sintaxis antigua es ligeramente diferente entre los diferentes servidores.

Los beneficios de la sintaxis de unión SQL92 son más fáciles de identificar para consultas complejas que incluyen tanto condiciones de unión como condiciones de filtro. Considera la siguiente consulta, que devuelve solo aquellos clientes cuyo código postal es 52137:

```sql
mysql> SELECT c.first_name, c.last_name, a.address
    -> FROM customer c, address a
    -> WHERE c.address_id = a.address_id
    ->   AND a.postal_code = 52137;
+------------+-----------+------------------------+
| first_name | last_name | address                |
+------------+-----------+------------------------+
| JAMES      | GANNON    | 1635 Kuwana Boulevard  |
| FREDDIE    | DUGGAN    | 1103 Quilmes Boulevard |
+------------+-----------+------------------------+
2 rows in set (0.01 sec)
```

A primera vista, no es tan fácil determinar cuáles condiciones en la cláusula `where` son condiciones de unión y cuáles son condiciones de filtro. Tampoco es inmediatamente evidente qué tipo de unión se está empleando (para identificar el tipo de unión, tendrías que mirar de cerca las condiciones de unión en la cláusula `where` para ver si se emplean caracteres especiales), ni es fácil determinar si alguna condición de unión se ha omitido por error. Aquí está la misma consulta usando la sintaxis de unión SQL92:

```sql
mysql> SELECT c.first_name, c.last_name, a.address
    -> FROM customer c INNER JOIN address a
    ->   ON c.address_id = a.address_id
    -> WHERE a.postal_code = 52137;
+------------+-----------+------------------------+
| first_name | last_name | address                |
+------------+-----------+------------------------+
| JAMES      | GANNON    | 1635 Kuwana Boulevard  |
| FREDDIE    | DUGGAN    | 1103 Quilmes Boulevard |
+------------+-----------+------------------------+
2 rows in set (0.00 sec)
```

Con esta versión, está claro qué condición se usa para la unión y cuál para el filtrado. Espero que estés de acuerdo en que la versión que usa la sintaxis de unión SQL92 es más fácil de entender.

---


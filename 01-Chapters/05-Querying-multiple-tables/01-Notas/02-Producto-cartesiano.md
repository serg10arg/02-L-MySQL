
---

# Producto Cartesiano

La forma más fácil de empezar es colocar las tablas `customer` y `address` en la cláusula `from` de una consulta y ver qué sucede. Aquí hay una consulta que recupera los nombres y apellidos del cliente junto con la dirección, con una cláusula `from` que nombra ambas tablas separadas por la palabra clave `join`:

```sql
mysql> SELECT c.first_name, c.last_name, a.address
    -> FROM customer c JOIN address a;
+------------+-----------+----------------------+
| first_name | last_name | address              |
+------------+-----------+----------------------+
| MARY       | SMITH     | 47 MySakila Drive    |
| PATRICIA   | JOHNSON   | 47 MySakila Drive    |
| LINDA      | WILLIAMS  | 47 MySakila Drive    |
| BARBARA    | JONES     | 47 MySakila Drive    |
| ELIZABETH  | BROWN     | 47 MySakila Drive    |
| JENNIFER   | DAVIS     | 47 MySakila Drive    |
| MARIA      | MILLER    | 47 MySakila Drive    |
| SUSAN      | WILSON    | 47 MySakila Drive    |
...
| SETH       | HANNON    | 1325 Fukuyama Street |
| KENT       | ARSENAULT | 1325 Fukuyama Street |
| TERRANCE   | ROUSH     | 1325 Fukuyama Street |
| RENE       | MCALISTER | 1325 Fukuyama Street |
| EDUARDO    | HIATT     | 1325 Fukuyama Street |
| TERRENCE   | GUNDERSON | 1325 Fukuyama Street |
| ENRIQUE    | FORSYTHE  | 1325 Fukuyama Street |
| FREDDIE    | DUGGAN    | 1325 Fukuyama Street |
| WADE       | DELVALLE  | 1325 Fukuyama Street |
| AUSTIN     | CINTRON   | 1325 Fukuyama Street |
+------------+-----------+----------------------+
361197 rows in set (0.03 sec)
```

Hmmm... hay solo 599 clientes y 603 filas en la tabla `address`, entonces, ¿cómo es que el conjunto de resultados terminó con 361,197 filas? Observando más de cerca, puedes ver que muchos de los clientes parecen tener la misma dirección. Debido a que la consulta no especificó cómo deberían unirse las dos tablas, el servidor de la base de datos generó el producto cartesiano, que es cada permutación de las dos tablas (599 clientes x 603 direcciones = 361,197 permutaciones). Este tipo de unión se conoce como una unión cruzada, y rara vez se usa (al menos, intencionadamente). Las uniones cruzadas son uno de los tipos de unión que estudiamos en el Capítulo 10.

---


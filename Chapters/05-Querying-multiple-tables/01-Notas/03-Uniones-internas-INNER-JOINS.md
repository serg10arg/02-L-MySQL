
---

# Uniones Internas (Inner Joins)

Para modificar la consulta anterior de modo que solo se devuelva una fila por cada cliente, debes describir cómo están relacionadas las dos tablas. Anteriormente, mencioné que la columna `customer.address_id` sirve como el enlace entre las dos tablas, por lo que esta información debe agregarse a la subcláusula `on` de la cláusula `from`:

```sql
mysql> SELECT c.first_name, c.last_name, a.address
    -> FROM customer c JOIN address a
    ->   ON c.address_id = a.address_id;
+-------------+--------------+----------------------------------------+
| first_name  | last_name    | address                                |
+-------------+--------------+----------------------------------------+
| MARY        | SMITH        | 1913 Hanoi Way                         |
| PATRICIA    | JOHNSON      | 1121 Loja Avenue                       |
| LINDA       | WILLIAMS     | 692 Joliet Street                      |
| BARBARA     | JONES        | 1566 Inegl Manor                       |
| ELIZABETH   | BROWN        | 53 Idfu Parkway                        |
| JENNIFER    | DAVIS        | 1795 Santiago de Compostela Way        |
| MARIA       | MILLER       | 900 Santiago de Compostela Parkway     |
| SUSAN       | WILSON       | 478 Joliet Way                         |
...
| MARGARET    | MOORE        | 613 Korolev Drive                      |
| TERRANCE    | ROUSH        | 42 Fontana Avenue                      |
| RENE        | MCALISTER    | 1895 Zhezqazghan Drive                 |
| EDUARDO     | HIATT        | 1837 Kaduna Parkway                    |
| TERRENCE    | GUNDERSON    | 844 Bucuresti Place                    |
| ENRIQUE     | FORSYTHE     | 1101 Bucuresti Boulevard               |
| FREDDIE     | DUGGAN       | 1103 Quilmes Boulevard                 |
| WADE        | DELVALLE     | 1331 Usak Boulevard                    |
| AUSTIN      | CINTRON      | 1325 Fukuyama Street                   |
+-------------+--------------+----------------------------------------+
599 rows in set (0.00 sec)
```

En lugar de 361,197 filas, ahora tienes las 599 filas esperadas debido a la adición de la subcláusula `on`, que instruye al servidor a unir las tablas `customer` y `address` utilizando la columna `address_id` para recorrer de una tabla a la otra. Por ejemplo, la fila de Mary Smith en la tabla `customer` contiene un valor de 5 en la columna `address_id` (no mostrado en el ejemplo). El servidor usa este valor para buscar la fila en la tabla `address` que tenga un valor de 5 en su columna `address_id` y luego recupera el valor '1913 Hanoi Way' de la columna `address` en esa fila.

Si existe un valor para la columna `address_id` en una tabla pero no en la otra, entonces la unión falla para las filas que contienen ese valor, y esas filas se excluyen del conjunto de resultados. Este tipo de unión se conoce como una unión interna (inner join), y es el tipo de unión más comúnmente utilizado. Para aclarar, si una fila en la tabla `customer` tiene el valor 999 en la columna `address_id` y no hay una fila en la tabla `address` con un valor de 999 en la columna `address_id`, entonces esa fila de cliente no se incluiría en el conjunto de resultados.

Si deseas incluir todas las filas de una tabla u otra, independientemente de si existe una coincidencia, necesitas especificar una unión externa (outer join), pero esto se explorará en el Capítulo 10.

En el ejemplo anterior, no especifiqué en la cláusula `from` qué tipo de unión usar. Sin embargo, cuando desees unir dos tablas usando una unión interna, debes especificarlo explícitamente en tu cláusula `from`; aquí está el mismo ejemplo, con la adición del tipo de unión (observa la palabra clave `inner`):

```sql
SELECT c.first_name, c.last_name, a.address
FROM customer c INNER JOIN address a
  ON c.address_id = a.address_id;
```

Si no especificas el tipo de unión, el servidor realizará una unión interna por defecto. Sin embargo, como verás más adelante en el libro, hay varios tipos de uniones, por lo que debes acostumbrarte a especificar el tipo exacto de unión que necesitas, especialmente para el beneficio de otras personas que puedan usar/mantener tus consultas en el futuro.

Si los nombres de las columnas utilizadas para unir las dos tablas son idénticos, lo cual es cierto en la consulta anterior, puedes usar la subcláusula `using` en lugar de la subcláusula `on`, como en:

```sql
SELECT c.first_name, c.last_name, a.address
FROM customer c INNER JOIN address a
USING (address_id);
```

Dado que `using` es una notación abreviada que solo puedes usar en una situación específica, prefiero siempre usar la subcláusula `on` para evitar confusiones.

---


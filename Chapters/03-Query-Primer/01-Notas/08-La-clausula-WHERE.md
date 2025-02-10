### La Cláusula `WHERE`

En algunos casos, es posible que desees recuperar todas las filas de una tabla, especialmente para tablas pequeñas como `language`. Sin embargo, la mayoría de las veces, no querrás recuperar todas las filas de una tabla, sino filtrar aquellas filas que no son de interés. Para esto sirve la cláusula `WHERE`.

**La cláusula `WHERE` es el mecanismo para filtrar filas no deseadas del conjunto de resultados.**

### Ejemplo de Uso de `WHERE`

Imagina que estás interesado en alquilar una película, pero solo te interesan las películas clasificadas como G que se puedan mantener durante al menos una semana. La siguiente consulta utiliza una cláusula `WHERE` para recuperar solo las películas que cumplen con estos criterios:

```sql
mysql> SELECT title
    -> FROM film
    -> WHERE rating = 'G' AND rental_duration >= 7;
```

**Resultado**:
```
+-------------------------+
| title                   |
+-------------------------+
| BLANKET BEVERLY         |
| BORROWERS BEDAZZLED     |
| BRIDE INTRIGUE          |
| CATCH AMISTAD           |
| CITIZEN SHREK           |
| COLDBLOODED DARLING     |
| CONTROL ANTHEM          |
| CRUELTY UNFORGIVEN      |
| DARN FORRESTER          |
| DESPERATE TRAINSPOTTING |
| DIARY PANIC             |
| DRACULA CRYSTAL         |
| EMPIRE MALKOVICH        |
| FIREHOUSE VIETNAM       |
| GILBERT PELICAN         |
| GRADUATE LORD           |
| GREASE YOUTH            |
| GUN BONNIE              |
| HOOK CHARIOTS           |
| MARRIED GO              |
| MENAGERIE RUSHMORE      |
| MUSCLE BRIGHT           |
| OPERATION OPERATION     |
| PRIMARY GLASS           |
| REBEL AIRPORT           |
| SPIKING ELEMENT         |
| TRUMAN CRAZY            |
| WAKE JAWS               |
| WAR NOTTING             |
+-------------------------+
29 rows in set (0.00 sec)
```

En este caso, la cláusula `WHERE` filtró 971 de las 1000 filas en la tabla `film`. Esta cláusula `WHERE` contiene dos condiciones de filtro, pero puedes incluir tantas condiciones como sea necesario; las condiciones individuales se separan usando operadores como `AND`, `OR` y `NOT`.

### Uso de Operadores `AND` y `OR`

¿Qué sucede si cambias el operador que separa las dos condiciones de `AND` a `OR`?

```sql
mysql> SELECT title
    -> FROM film
    -> WHERE rating = 'G' OR rental_duration >= 7;
```

**Resultado**:
```
+---------------------------+
| title                     |
+---------------------------+
| ACE GOLDFINGER            |
| ADAPTATION HOLES          |
| AFFAIR PREJUDICE          |
| AFRICAN EGG               |
| ALAMO VIDEOTAPE           |
| AMISTAD MIDSUMMER         |
| ANGELS LIFE               |
| ANNIE IDENTITY            |
|...                        |
| WATERSHIP FRONTIER        |
| WEREWOLF LOLA             |
| WEST LION                 |
| WESTWARD SEABISCUIT       |
| WOLVES DESIRE             |
| WON DARES                 |
| WORKER TARZAN             |
| YOUNG LANGUAGE            |
+---------------------------+
340 rows in set (0.00 sec)
```

Cuando separas condiciones usando el operador `AND`, todas las condiciones deben evaluar como verdaderas para ser incluidas en el conjunto de resultados; cuando usas `OR`, solo una de las condiciones necesita evaluar como verdadera para que una fila sea incluida, lo que explica por qué el tamaño del conjunto de resultados saltó de 29 a 340 filas.

### Uso de Paréntesis para Agrupar Condiciones

Si necesitas usar tanto `AND` como `OR` en tu cláusula `WHERE`, debes usar paréntesis para agrupar las condiciones. La siguiente consulta especifica que solo se incluyan en el conjunto de resultados aquellas películas que están clasificadas como G y están disponibles durante 7 o más días, o están clasificadas como PG-13 y están disponibles durante 3 o menos días:

```sql
mysql> SELECT title, rating, rental_duration
    -> FROM film
    -> WHERE (rating = 'G' AND rental_duration >= 7)
    ->   OR (rating = 'PG-13' AND rental_duration < 4);
```

**Resultado**:
```
+-------------------------+--------+-----------------+
| title                   | rating | rental_duration |
+-------------------------+--------+-----------------+
| ALABAMA DEVIL           | PG-13  |               3 |
| BACKLASH UNDEFEATED     | PG-13  |               3 |
| BILKO ANONYMOUS         | PG-13  |               3 |
| BLANKET BEVERLY         | G      |               7 |
| BORROWERS BEDAZZLED     | G      |               7 |
| BRIDE INTRIGUE          | G      |               7 |
| CASPER DRAGONFLY        | PG-13  |               3 |
| CATCH AMISTAD           | G      |               7 |
| CITIZEN SHREK           | G      |               7 |
| COLDBLOODED DARLING     | G      |               7 |
|...                                                 |
| TREASURE COMMAND        | PG-13  |               3 |
| TRUMAN CRAZY            | G      |               7 |
| WAIT CIDER              | PG-13  |               3 |
| WAKE JAWS               | G      |               7 |
| WAR NOTTING             | G      |               7 |
| WORLD LEATHERNECKS      | PG-13  |               3 |
+-------------------------+--------+-----------------+
68 rows in set (0.00 sec)
```

Siempre debes usar paréntesis para separar grupos de condiciones cuando mezclas diferentes operadores, para que tanto tú, el servidor de bases de datos, y cualquier persona que modifique tu código más adelante estén en la misma página.


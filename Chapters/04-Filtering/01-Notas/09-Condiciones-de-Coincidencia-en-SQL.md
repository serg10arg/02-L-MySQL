### Condiciones de Coincidencia en SQL

Hasta ahora, has aprendido a utilizar condiciones que identifican una cadena exacta, un rango de cadenas o un conjunto de cadenas. Ahora exploraremos cómo trabajar con coincidencias parciales de cadenas.

### Ejemplo de Coincidencia Parcial

Supongamos que deseas encontrar todos los clientes cuyo apellido comienza con la letra `Q`. Podrías usar una función integrada para obtener la primera letra de la columna `last_name`, como en el siguiente ejemplo:

```sql
mysql> SELECT last_name, first_name
    -> FROM customer
    -> WHERE LEFT(last_name, 1) = 'Q';
```

**Resultado**:

```sql
+-------------+------------+
| last_name   | first_name |
+-------------+------------+
| QUALLS      | STEPHEN    |
| QUINTANILLA | ROGER      |
| QUIGLEY     | TROY       |
+-------------+------------+
3 rows in set (0.00 sec)
```

Aunque la función `LEFT()` hace el trabajo, no te ofrece mucha flexibilidad. En su lugar, puedes usar caracteres comodín para construir expresiones de búsqueda.

### Uso de Comodines

Al buscar coincidencias parciales de cadenas, podrías estar interesado en:

- Cadenas que comienzan/terminan con un cierto carácter.
- Cadenas que comienzan/terminan con una subcadena.
- Cadenas que contienen un cierto carácter en cualquier parte de la cadena.
- Cadenas que contienen una subcadena en cualquier parte de la cadena.
- Cadenas con un formato específico, independientemente de los caracteres individuales.

Puedes construir expresiones de búsqueda para identificar estas y muchas otras coincidencias parciales de cadenas utilizando los caracteres comodín que se muestran en la siguiente tabla:

| Carácter Comodín | Coincidencias |
| --- | --- |
| `_` | Exactamente un carácter |
| `%` | Cualquier número de caracteres (incluyendo 0) |

El carácter de subrayado (`_`) reemplaza un solo carácter, mientras que el signo de porcentaje (`%`) puede reemplazar un número variable de caracteres. Al construir condiciones que utilizan expresiones de búsqueda, usas el operador `LIKE`, como en el siguiente ejemplo:

```sql
mysql> SELECT last_name, first_name
    -> FROM customer
    -> WHERE last_name LIKE '_A_T%S';
```

**Resultado**:

```sql
+-----------+------------+
| last_name | first_name |
+-----------+------------+
| MATTHEWS  | ERICA      |
| WALTERS   | CASSANDRA  |
| WATTS     | SHELLY     |
+-----------+------------+
3 rows in set (0.00 sec)
```

La expresión de búsqueda en el ejemplo anterior especifica cadenas que contienen una `A` en la segunda posición y una `T` en la cuarta posición, seguidas de cualquier número de caracteres y terminando en `S`.

### Ejemplos Adicionales de Expresiones de Búsqueda

| Expresión de Búsqueda | Interpretación |
| --- | --- |
| `F%` | Cadenas que comienzan con `F` |
| `%t` | Cadenas que terminan con `t` |
| `%bas%` | Cadenas que contienen la subcadena `bas` |
| `__t_` | Cadenas de cuatro caracteres con una `t` en la tercera posición |
| `___-__-____` |  |
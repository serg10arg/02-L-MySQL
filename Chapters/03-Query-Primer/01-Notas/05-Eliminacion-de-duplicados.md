### Eliminación de Duplicados

En algunos casos, una consulta puede devolver filas duplicadas de datos. Por ejemplo, si deseas recuperar los IDs de todos los actores que aparecieron en una película, podrías ver lo siguiente:

```sql
mysql> SELECT actor_id FROM film_actor ORDER BY actor_id;
```

**Resultado**:
```
+----------+
| actor_id |
+----------+
|        1 |
|        1 |
|        1 |
|        1 |
|        1 |
|        1 |
|        1 |
|        1 |
|        1 |
|        1 |
...
|      200 |
|      200 |
|      200 |
|      200 |
|      200 |
|      200 |
|      200 |
|      200 |
|      200 |
+----------+
5462 rows in set (0.01 sec)
```

Dado que algunos actores aparecieron en más de una película, verás el mismo ID de actor varias veces. Probablemente lo que desees en este caso es el conjunto distinto de actores, en lugar de ver los IDs de actores repetidos para cada película en la que aparecieron. Puedes lograr esto añadiendo la palabra clave `DISTINCT` directamente después de la palabra clave `SELECT`, como se muestra a continuación:

```sql
mysql> SELECT DISTINCT actor_id FROM film_actor ORDER BY actor_id;
```

**Resultado**:
```
+----------+
| actor_id |
+----------+
|        1 |
|        2 |
|        3 |
|        4 |
|        5 |
|        6 |
|        7 |
|        8 |
|        9 |
|       10 |
...
|      192 |
|      193 |
|      194 |
|      195 |
|      196 |
|      197 |
|      198 |
|      199 |
|      200 |
+----------+
200 rows in set (0.01 sec)
```

Ahora, el conjunto de resultados contiene 200 filas, una para cada actor distinto, en lugar de 5,462 filas, una para cada aparición de un actor en una película.

### Consejos para el Uso de `DISTINCT`

- **Lista de Todos los Actores**: Si simplemente quieres una lista de todos los actores, puedes consultar la tabla `actor` en lugar de leer todas las filas en `film_actor` y eliminar duplicados.
- **Palabra Clave `ALL`**: Si no deseas que el servidor elimine datos duplicados o estás seguro de que no habrá duplicados en tu conjunto de resultados, puedes especificar la palabra clave `ALL` en lugar de `DISTINCT`. Sin embargo, la palabra clave `ALL` es el valor predeterminado y nunca necesita ser nombrada explícitamente, por lo que la mayoría de los programadores no la incluyen en sus consultas.
- **Costo de `DISTINCT`**: Generar un conjunto de resultados distintos requiere que los datos se ordenen, lo cual puede consumir tiempo para conjuntos de resultados grandes. No caigas en la trampa de usar `DISTINCT` solo para asegurarte de que no haya duplicados; en su lugar, tómate el tiempo para entender los datos con los que estás trabajando para saber si los duplicados son posibles.


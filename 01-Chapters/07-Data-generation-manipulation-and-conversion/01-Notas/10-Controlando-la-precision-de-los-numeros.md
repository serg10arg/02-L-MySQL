# Controlando la Precisión de los Números

Al trabajar con números de punto flotante, es posible que no siempre desees interactuar con o mostrar un número con su precisión completa. Por ejemplo, puedes almacenar datos de transacciones monetarias con una precisión de hasta seis decimales, pero podrías querer redondear al centésimo más cercano para propósitos de visualización. Cuatro funciones son útiles cuando se limita la precisión de los números de punto flotante: `ceil()`, `floor()`, `round()` y `truncate()`. Los tres servidores incluyen estas funciones, aunque Oracle Database incluye `trunc()` en lugar de `truncate()`, y SQL Server incluye `ceiling()` en lugar de `ceil()`.

Las funciones `ceil()` y `floor()` se utilizan para redondear hacia arriba o hacia abajo al entero más cercano, como se demuestra a continuación:

```sql
mysql> SELECT CEIL(72.445), FLOOR(72.445);
+--------------+---------------+
| CEIL(72.445) | FLOOR(72.445) |
+--------------+---------------+
|           73 |            72 |
+--------------+---------------+
1 row in set (0.06 sec)
```

Así, cualquier número entre 72 y 73 será evaluado como 73 por la función `ceil()` y 72 por la función `floor()`. Recuerda que `ceil()` redondeará hacia arriba incluso si la porción decimal de un número es muy pequeña, y `floor()` redondeará hacia abajo incluso si la porción decimal es bastante significativa, como en:

```sql
mysql> SELECT CEIL(72.000000001), FLOOR(72.999999999);
+--------------------+---------------------+
| CEIL(72.000000001) | FLOOR(72.999999999) |
+--------------------+---------------------+
|                 73 |                  72 |
+--------------------+---------------------+
1 row in set (0.00 sec)
```

Si esto es un poco demasiado severo para tu aplicación, puedes usar la función `round()` para redondear hacia arriba o hacia abajo desde el punto medio entre dos enteros, como en:

```sql
mysql> SELECT ROUND(72.49999), ROUND(72.5), ROUND(72.50001);
+-----------------+-------------+-----------------+
| ROUND(72.49999) | ROUND(72.5) | ROUND(72.50001) |
+-----------------+-------------+-----------------+
|              72 |          73 |              73 |
+-----------------+-------------+-----------------+
1 row in set (0.00 sec)
```

Usando `round()`, cualquier número cuya porción decimal esté a medio camino o más entre dos enteros será redondeado hacia arriba, mientras que el número será redondeado hacia abajo si la porción decimal es menos de la mitad entre los dos enteros.

La mayoría de las veces, querrás mantener al menos una parte de la porción decimal de un número en lugar de redondear al entero más cercano; la función `round()` permite un segundo argumento opcional para especificar cuántos dígitos a la derecha del punto decimal se redondearán. El siguiente ejemplo muestra cómo puedes usar el segundo argumento para redondear el número 72.0909 a uno, dos y tres lugares decimales:

```sql
mysql> SELECT ROUND(72.0909, 1), ROUND(72.0909, 2), ROUND(72.0909, 3);
+-------------------+-------------------+-------------------+
| ROUND(72.0909, 1) | ROUND(72.0909, 2) | ROUND(72.0909, 3) |
+-------------------+-------------------+-------------------+
|              72.1 |             72.09 |            72.091 |
+-------------------+-------------------+-------------------+
1 row in set (0.00 sec)
```

Al igual que la función `round()`, la función `truncate()` permite un segundo argumento opcional para especificar el número de dígitos a la derecha del punto decimal, pero `truncate()` simplemente descarta los dígitos no deseados sin redondear. El siguiente ejemplo muestra cómo el número 72.0909 sería truncado a uno, dos y tres lugares decimales:

```sql
mysql> SELECT TRUNCATE(72.0909, 1), TRUNCATE(72.0909, 2),
    ->   TRUNCATE(72.0909, 3);
+----------------------+----------------------+----------------------+
| TRUNCATE(72.0909, 1) | TRUNCATE(72.0909, 2) | TRUNCATE(72.0909, 3) |
+----------------------+----------------------+----------------------+
|                 72.0 |                72.09 |               72.090 |
+----------------------+----------------------+----------------------+
1 row in set (0.00 sec)
```

SQL Server no incluye una función `truncate()`. En su lugar, la función `round()` permite un tercer argumento opcional que, si está presente y es distinto de cero, llama a que el número sea truncado en lugar de redondeado.

Tanto `truncate()` como `round()` también permiten un valor negativo para el segundo argumento, lo que significa que los números a la izquierda del punto decimal se truncan o redondean. Esto podría parecer una cosa extraña de hacer al principio, pero hay aplicaciones válidas. Por ejemplo, podrías vender un producto que solo se puede comprar en unidades de 10. Si un cliente ordenara 17 unidades, podrías elegir uno de los siguientes métodos para modificar la cantidad de pedido del cliente:

```sql
mysql> SELECT ROUND(17, -1), TRUNCATE(17, -1);
+---------------+------------------+
| ROUND(17, -1) | TRUNCATE(17, -1) |
+---------------+------------------+
|            20 |               10 |
+---------------+------------------+
1 row in set (0.00 sec)
```

Si el producto en cuestión son chinchetas, entonces podría no hacer mucha diferencia en tu balance final si vendiste al cliente 10 o 20 chinchetas cuando solo se solicitaron 17; si estás vendiendo relojes Rolex, sin embargo, a tu negocio le podría ir mejor redondeando.

---


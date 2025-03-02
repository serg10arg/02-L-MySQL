
---

# Trabajando con Datos Numéricos

A diferencia de los datos de cadena (y los datos temporales, como verás en breve), la generación de datos numéricos es bastante sencilla. Puedes escribir un número, recuperarlo de otra columna o generarlo mediante un cálculo. Todos los operadores aritméticos habituales (+, -, *, /) están disponibles para realizar cálculos, y se pueden usar paréntesis para dictar la precedencia, como en:

```sql
mysql> SELECT (37 * 59) / (78 - (8 * 6));
+----------------------------+
| (37 * 59) / (78 - (8 * 6)) |
+----------------------------+
|                      72.77 |
+----------------------------+
1 row in set (0.00 sec)
```

Como mencioné en el Capítulo 2, la principal preocupación al almacenar datos numéricos es que los números podrían redondearse si son más grandes que el tamaño especificado para una columna numérica. Por ejemplo, el número 9.96 se redondeará a 10.0 si se almacena en una columna definida como `float(3,1)`.

---


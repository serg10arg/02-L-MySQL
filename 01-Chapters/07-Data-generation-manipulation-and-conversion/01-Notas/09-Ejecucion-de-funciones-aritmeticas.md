# Realización de Funciones Aritméticas

La mayoría de las funciones numéricas integradas se utilizan para propósitos aritméticos específicos, como determinar la raíz cuadrada de un número. A continuación se muestran algunas de las funciones numéricas comunes que toman un argumento numérico y devuelven un número.

### Tabla 7-1. Funciones Numéricas de un Solo Argumento

| Nombre de la Función | Descripción                             |
|----------------------|-----------------------------------------|
| `acos(x)`            | Calcula el arcocoseno de `x`.           |
| `asin(x)`            | Calcula el arcoseno de `x`.             |
| `atan(x)`            | Calcula el arcotangente de `x`.         |
| `cos(x)`             | Calcula el coseno de `x`.               |
| `cot(x)`             | Calcula la cotangente de `x`.           |
| `exp(x)`             | Calcula `e^x`.                          |
| `ln(x)`              | Calcula el logaritmo natural de `x`.    |
| `sin(x)`             | Calcula el seno de `x`.                 |
| `sqrt(x)`            | Calcula la raíz cuadrada de `x`.        |
| `tan(x)`             | Calcula la tangente de `x`.             |

Estas funciones realizan tareas muy específicas, y prefiero no mostrar ejemplos para estas funciones (si no reconoces una función por nombre o descripción, probablemente no la necesites). Sin embargo, otras funciones numéricas utilizadas para cálculos son un poco más flexibles y merecen alguna explicación.

Por ejemplo, el operador módulo, que calcula el residuo cuando un número se divide por otro número, se implementa en MySQL y Oracle Database a través de la función `mod()`. El siguiente ejemplo calcula el residuo cuando 10 se divide por 4:

```sql
mysql> SELECT MOD(10,4);
+-----------+
| MOD(10,4) |
+-----------+
|         2 |
+-----------+
1 row in set (0.02 sec)
```

Aunque la función `mod()` se usa típicamente con argumentos enteros, con MySQL también puedes usar números reales, como en:

```sql
mysql> SELECT MOD(22.75, 5);
+---------------+
| MOD(22.75, 5) |
+---------------+
|          2.75 |
+---------------+
1 row in set (0.02 sec)
```

SQL Server no tiene una función `mod()`. En su lugar, se usa el operador `%` para encontrar residuos. La expresión `10 % 4` por lo tanto, dará el valor 2.

Otra función numérica que toma dos argumentos numéricos es la función `pow()` (o `power()` si estás usando Oracle Database o SQL Server), que devuelve un número elevado a la potencia de un segundo número, como en:

```sql
mysql> SELECT POW(2,8);
+----------+
| POW(2,8) |
+----------+
|      256 |
+----------+
1 row in set (0.03 sec)
```

Así, `pow(2,8)` es el equivalente en MySQL a especificar `2^8`. Dado que la memoria de la computadora se asigna en bloques de `2^x` bytes, la función `pow()` puede ser una forma útil de determinar el número exacto de bytes en una cierta cantidad de memoria:

```sql
mysql> SELECT POW(2,10) kilobyte, POW(2,20) megabyte,
    ->   POW(2,30) gigabyte, POW(2,40) terabyte;
+----------+----------+------------+---------------+
| kilobyte | megabyte | gigabyte   | terabyte      |
+----------+----------+------------+---------------+
|     1024 |  1048576 | 1073741824 | 1099511627776 |
+----------+----------+------------+---------------+
1 row in set (0.00 sec)
```

No sé tú, pero yo encuentro más fácil recordar que un gigabyte es `2^30` bytes que recordar el número `1,073,741,824`.

---


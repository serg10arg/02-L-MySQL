
### La Sentencia SQL:
```sql
SELECT account_id, SIGN(balance), ABS(balance)
FROM account;
```

### ¿Qué hace esta sentencia?

1. **SELECT**: Es una palabra clave en SQL que se utiliza para seleccionar datos de una tabla. En este caso, estamos seleccionando tres columnas:
    - `account_id`: El identificador de la cuenta.
    - `SIGN(balance)`: El resultado de aplicar la función `SIGN` al saldo (`balance`).
    - `ABS(balance)`: El resultado de aplicar la función `ABS` al saldo (`balance`).

2. **FROM account**: Indica que los datos se seleccionan de la tabla llamada `account`.

3. **Funciones utilizadas**:
    - **`SIGN`**: Es una función en SQL que devuelve el signo de un número. Puede devolver:
        - `1` si el número es positivo.
        - `0` si el número es cero.
        - `-1` si el número es negativo.
    - **`ABS`**: Es una función en SQL que devuelve el valor absoluto de un número. El valor absoluto es el número sin su signo (siempre positivo o cero).

---

### Desglosando los resultados:

#### Tabla `account` (supuesta):
| account_id | balance |
|------------|---------|
| 123        | 785.22  |
| 456        | 0.00    |
| 789        | -324.22 |

#### Aplicando las funciones:

1. **Fila 1 (`account_id = 123`)**:
    - `balance = 785.22` (positivo).
    - `SIGN(785.22)` = `1` (porque el número es positivo).
    - `ABS(785.22)` = `785.22` (el valor absoluto de un número positivo es el mismo número).

2. **Fila 2 (`account_id = 456`)**:
    - `balance = 0.00` (cero).
    - `SIGN(0.00)` = `0` (porque el número es cero).
    - `ABS(0.00)` = `0.00` (el valor absoluto de cero es cero).

3. **Fila 3 (`account_id = 789`)**:
    - `balance = -324.22` (negativo).
    - `SIGN(-324.22)` = `-1` (porque el número es negativo).
    - `ABS(-324.22)` = `324.22` (el valor absoluto de un número negativo es el número sin el signo).

---

### Resultado:
```sql
+------------+---------------+--------------+
| account_id | SIGN(balance) | ABS(balance) |
+------------+---------------+--------------+
|        123 |             1 |       785.22 |
|        456 |             0 |         0.00 |
|        789 |            -1 |       324.22 |
+------------+---------------+--------------+
```

---

### Resumen:
- **`SIGN(balance)`**: Devuelve el signo del saldo:
    - `1` para saldos positivos.
    - `0` para saldos iguales a cero.
    - `-1` para saldos negativos.
- **`ABS(balance)`**: Devuelve el valor absoluto del saldo (siempre positivo o cero).

---

### ¿Para qué sirve esto?
- **`SIGN`**: Es útil para clasificar números en categorías (positivos, negativos o cero).
- **`ABS`**: Es útil cuando solo te interesa la magnitud de un número, sin importar su signo (por ejemplo, para cálculos de diferencias o distancias).

---


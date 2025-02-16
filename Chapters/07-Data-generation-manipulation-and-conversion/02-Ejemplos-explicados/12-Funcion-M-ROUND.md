
### La Sentencia SQL:
```sql
SELECT ROUND(72.0909, 1), ROUND(72.0909, 2), ROUND(72.0909, 3);
```

### ¿Qué hace esta sentencia?

1. **SELECT**: Es una palabra clave en SQL que se utiliza para seleccionar datos. En este caso, estamos seleccionando el resultado de la función `ROUND` aplicada al número `72.0909` con diferentes precisiones.

2. **ROUND**: Es una función en SQL que redondea un número a un número específico de decimales. La función `ROUND` toma dos argumentos:
    - **Primer argumento**: El número que se va a redondear. En este caso, es `72.0909`.
    - **Segundo argumento**: El número de decimales al que se redondeará el número. En este caso, se prueba con `1`, `2` y `3` decimales.

---

### Desglosando los argumentos:

#### 1. `ROUND(72.0909, 1)`:
- El número proporcionado es `72.0909`.
- El segundo argumento es `1`, lo que significa que queremos redondear el número a **1 decimal**.
- El primer decimal es `0` (en `72.0`**9**09), y el siguiente dígito es `9`, que es mayor o igual a 5, por lo que redondeamos hacia arriba.
- Por lo tanto, `ROUND(72.0909, 1)` devuelve `72.1`.

#### 2. `ROUND(72.0909, 2)`:
- El número proporcionado es `72.0909`.
- El segundo argumento es `2`, lo que significa que queremos redondear el número a **2 decimales**.
- El segundo decimal es `9` (en `72.09`**0**9), y el siguiente dígito es `0`, que es menor que 5, por lo que no redondeamos hacia arriba.
- Por lo tanto, `ROUND(72.0909, 2)` devuelve `72.09`.

#### 3. `ROUND(72.0909, 3)`:
- El número proporcionado es `72.0909`.
- El segundo argumento es `3`, lo que significa que queremos redondear el número a **3 decimales**.
- El tercer decimal es `0` (en `72.090`**9**), y el siguiente dígito es `9`, que es mayor o igual a 5, por lo que redondeamos hacia arriba.
- Por lo tanto, `ROUND(72.0909, 3)` devuelve `72.091`.

---

### Resultado:
```sql
+-------------------+-------------------+-------------------+
| ROUND(72.0909, 1) | ROUND(72.0909, 2) | ROUND(72.0909, 3) |
+-------------------+-------------------+-------------------+
|              72.1 |             72.09 |            72.091 |
+-------------------+-------------------+-------------------+
```

---

### Resumen:
- **`ROUND`**: Redondea un número a un número específico de decimales.
    - `ROUND(72.0909, 1)` = `72.1` (redondeado a 1 decimal).
    - `ROUND(72.0909, 2)` = `72.09` (redondeado a 2 decimales).
    - `ROUND(72.0909, 3)` = `72.091` (redondeado a 3 decimales).

Esta función es muy útil cuando necesitas controlar la precisión de los números en tus consultas SQL, ya sea para cálculos matemáticos, informes o análisis de datos.


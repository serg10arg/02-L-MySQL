
### La Sentencia SQL:
```sql
SELECT TRUNCATE(72.0909, 1), TRUNCATE(72.0909, 2), TRUNCATE(72.0909, 3);
```

### ¿Qué hace esta sentencia?

1. **SELECT**: Es una palabra clave en SQL que se utiliza para seleccionar datos. En este caso, estamos seleccionando el resultado de la función `TRUNCATE` aplicada al número `72.0909` con diferentes precisiones.

2. **TRUNCATE**: Es una función en SQL que **trunca** un número a un número específico de decimales. A diferencia de `ROUND`, que redondea el número, `TRUNCATE` simplemente elimina los dígitos más allá del número de decimales especificado, **sin redondear**. La función `TRUNCATE` toma dos argumentos:
    - **Primer argumento**: El número que se va a truncar. En este caso, es `72.0909`.
    - **Segundo argumento**: El número de decimales que se conservarán. En este caso, se prueba con `1`, `2` y `3` decimales.

---

### Desglosando los argumentos:

#### 1. `TRUNCATE(72.0909, 1)`:
- El número proporcionado es `72.0909`.
- El segundo argumento es `1`, lo que significa que queremos truncar el número a **1 decimal**.
- Simplemente eliminamos todos los dígitos después del primer decimal **sin redondear**.
- Por lo tanto, `TRUNCATE(72.0909, 1)` devuelve `72.0`.

#### 2. `TRUNCATE(72.0909, 2)`:
- El número proporcionado es `72.0909`.
- El segundo argumento es `2`, lo que significa que queremos truncar el número a **2 decimales**.
- Simplemente eliminamos todos los dígitos después del segundo decimal **sin redondear**.
- Por lo tanto, `TRUNCATE(72.0909, 2)` devuelve `72.09`.

#### 3. `TRUNCATE(72.0909, 3)`:
- El número proporcionado es `72.0909`.
- El segundo argumento es `3`, lo que significa que queremos truncar el número a **3 decimales**.
- Simplemente eliminamos todos los dígitos después del tercer decimal **sin redondear**.
- Por lo tanto, `TRUNCATE(72.0909, 3)` devuelve `72.090`.

---

### Resultado:
```sql
+----------------------+----------------------+----------------------+
| TRUNCATE(72.0909, 1) | TRUNCATE(72.0909, 2) | TRUNCATE(72.0909, 3) |
+----------------------+----------------------+----------------------+
|                 72.0 |                72.09 |               72.090 |
+----------------------+----------------------+----------------------+
```

---

### Resumen:
- **`TRUNCATE`**: Trunca un número a un número específico de decimales **sin redondear**.
    - `TRUNCATE(72.0909, 1)` = `72.0` (truncado a 1 decimal).
    - `TRUNCATE(72.0909, 2)` = `72.09` (truncado a 2 decimales).
    - `TRUNCATE(72.0909, 3)` = `72.090` (truncado a 3 decimales).

### Diferencia entre `TRUNCATE` y `ROUND`:
- **`ROUND`**: Redondea el número al número de decimales especificado.
- **`TRUNCATE`**: Simplemente elimina los dígitos adicionales **sin redondear**.

Por ejemplo:
- `ROUND(72.0909, 1)` = `72.1` (redondea hacia arriba).
- `TRUNCATE(72.0909, 1)` = `72.0` (trunca sin redondear).

---


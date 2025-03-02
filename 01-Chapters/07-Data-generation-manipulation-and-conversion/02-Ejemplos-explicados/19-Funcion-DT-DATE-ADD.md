
---

### 1. Primera Sentencia:
```sql
SELECT DATE_ADD(CURRENT_DATE(), INTERVAL 5 DAY);
```

#### ¿Qué hace esta sentencia?
- **`CURRENT_DATE()`**: Devuelve la fecha actual (en este caso, `2019-06-05`).
- **`DATE_ADD`**: Es una función que suma un intervalo de tiempo a una fecha.
- **`INTERVAL 5 DAY`**: Especifica que se deben sumar **5 días** a la fecha actual.

#### Resultado:
```sql
+------------------------------------------+
| DATE_ADD(CURRENT_DATE(), INTERVAL 5 DAY) |
+------------------------------------------+
| 2019-06-10                               |
+------------------------------------------+
```
- La fecha actual es `2019-06-05`.
- Al sumar 5 días, el resultado es `2019-06-10`.

---

### 2. Segunda Sentencia:
```sql
UPDATE rental
SET return_date = DATE_ADD(return_date, INTERVAL '3:27:11' HOUR_SECOND)
WHERE rental_id = 99999;
```

#### ¿Qué hace esta sentencia?
- **`UPDATE rental`**: Actualiza la tabla `rental`.
- **`SET return_date = DATE_ADD(...)`**: Modifica la columna `return_date` sumándole un intervalo de tiempo.
- **`INTERVAL '3:27:11' HOUR_SECOND`**: Especifica que se deben sumar **3 horas, 27 minutos y 11 segundos** a la fecha y hora actuales en `return_date`.
- **`WHERE rental_id = 99999`**: Aplica la actualización solo a la fila donde `rental_id` es `99999`.

#### Ejemplo:
Si `return_date` es `2019-09-17 14:30:00`, después de la actualización será `2019-09-17 17:57:11`.

---

### 3. Tercera Sentencia:
```sql
UPDATE employee
SET birth_date = DATE_ADD(birth_date, INTERVAL '9-11' YEAR_MONTH)
WHERE emp_id = 4789;
```

#### ¿Qué hace esta sentencia?
- **`UPDATE employee`**: Actualiza la tabla `employee`.
- **`SET birth_date = DATE_ADD(...)`**: Modifica la columna `birth_date` sumándole un intervalo de tiempo.
- **`INTERVAL '9-11' YEAR_MONTH`**: Especifica que se deben sumar **9 años y 11 meses** a la fecha actual en `birth_date`.
- **`WHERE emp_id = 4789`**: Aplica la actualización solo a la fila donde `emp_id` es `4789`.

#### Ejemplo:
Si `birth_date` es `1980-05-15`, después de la actualización será `1990-04-15`.

---

### Resumen de las funciones y cláusulas utilizadas:

1. **`DATE_ADD`**:
    - Suma un intervalo de tiempo a una fecha.
    - Sintaxis: `DATE_ADD(fecha, INTERVAL valor unidad)`.
    - Ejemplo: `DATE_ADD('2019-06-05', INTERVAL 5 DAY)`.

2. **`INTERVAL`**:
    - Define el intervalo de tiempo que se sumará o restará.
    - Puede ser en días (`DAY`), horas (`HOUR`), minutos (`MINUTE`), segundos (`SECOND`), meses (`MONTH`), años (`YEAR`), o combinaciones como `HOUR_SECOND` o `YEAR_MONTH`.

3. **`UPDATE`**:
    - Modifica datos existentes en una tabla.
    - Siempre debe usarse con `SET` para especificar las columnas a actualizar.
    - Es recomendable usar `WHERE` para limitar las filas afectadas.

4. **`CURRENT_DATE()`**:
    - Devuelve la fecha actual del sistema en formato `YYYY-MM-DD`.

---

### Ejemplos adicionales de `DATE_ADD`:

1. Sumar 2 horas y 30 minutos a una fecha y hora:
   ```sql
   SELECT DATE_ADD('2023-10-15 08:00:00', INTERVAL '2:30' HOUR_MINUTE);
   ```
   Resultado: `2023-10-15 10:30:00`.

2. Sumar 1 año y 6 meses a una fecha:
   ```sql
   SELECT DATE_ADD('2023-10-15', INTERVAL '1-6' YEAR_MONTH);
   ```
   Resultado: `2025-04-15`.

3. Restar 10 días a una fecha:
   ```sql
   SELECT DATE_ADD('2023-10-15', INTERVAL -10 DAY);
   ```
   Resultado: `2023-10-05`.

---


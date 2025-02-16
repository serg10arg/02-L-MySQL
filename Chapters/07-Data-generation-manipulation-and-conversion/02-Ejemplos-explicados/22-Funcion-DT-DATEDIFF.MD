
### La Sentencia SQL:
```sql
SELECT DATEDIFF('2019-09-03 23:59:59', '2019-06-21 00:00:01');
```

### ¿Qué hace esta sentencia?

1. **SELECT**: Es una palabra clave en SQL que se utiliza para seleccionar datos. En este caso, estamos seleccionando el resultado de la función `DATEDIFF`.

2. **DATEDIFF**: Es una función en SQL que calcula la **diferencia en días** entre dos fechas. Toma dos argumentos:
    - **Primer argumento**: La fecha de fin (en este caso, `'2019-09-03 23:59:59'`).
    - **Segundo argumento**: La fecha de inicio (en este caso, `'2019-06-21 00:00:01'`).

---

### Desglosando los argumentos:

#### 1. `DATEDIFF('2019-09-03 23:59:59', '2019-06-21 00:00:01')`:
- La función `DATEDIFF` calcula la diferencia en días entre las dos fechas proporcionadas.
- **Fecha de fin**: `'2019-09-03 23:59:59'`.
- **Fecha de inicio**: `'2019-06-21 00:00:01'`.
- La función ignora la parte de la hora y solo considera la parte de la fecha (`YYYY-MM-DD`).
- Por lo tanto, `DATEDIFF` calcula la diferencia entre `2019-09-03` y `2019-06-21`.

---

### Cálculo de la diferencia:
1. **Fecha de inicio**: `2019-06-21`.
2. **Fecha de fin**: `2019-09-03`.
3. **Días transcurridos**:
    - Junio tiene 30 días, así que del 21 al 30 de junio hay **10 días**.
    - Julio tiene **31 días**.
    - Agosto tiene **31 días**.
    - Septiembre tiene **3 días** (hasta el 3 de septiembre).
4. **Total**: `10 + 31 + 31 + 3 = 75 días`.

   **Nota**: El resultado en la consulta es `74` porque `DATEDIFF` excluye el día de inicio en el cálculo. Es decir, cuenta los días **entre** las dos fechas, no incluyendo el día de inicio.

---

### Resultado:
```sql
+--------------------------------------------------------+
| DATEDIFF('2019-09-03 23:59:59', '2019-06-21 00:00:01') |
+--------------------------------------------------------+
|                                                     74 |
+--------------------------------------------------------+
```

---

### Resumen:
- **`DATEDIFF`**: Calcula la diferencia en días entre dos fechas.
- En este caso, la diferencia entre `2019-09-03` y `2019-06-21` es de **74 días**.
- La función ignora la parte de la hora y solo considera la parte de la fecha (`YYYY-MM-DD`).

---

### ¿Para qué sirve esto?
- La función `DATEDIFF` es útil cuando necesitas calcular la diferencia en días entre dos fechas. Por ejemplo:
    - Calcular la antigüedad de un empleado.
    - Determinar cuántos días han pasado desde un evento.
    - Calcular plazos o vencimientos.

---

### Ejemplos adicionales de `DATEDIFF`:

1. Diferencia entre dos fechas cercanas:
   ```sql
   SELECT DATEDIFF('2023-10-15', '2023-10-10');
   ```
   Resultado: `5` (días entre el 10 y el 15 de octubre de 2023).

2. Diferencia entre fechas en diferentes años:
   ```sql
   SELECT DATEDIFF('2024-01-01', '2023-12-31');
   ```
   Resultado: `1` (días entre el 31 de diciembre de 2023 y el 1 de enero de 2024).

3. Diferencia entre fechas con horas:
   ```sql
   SELECT DATEDIFF('2023-10-15 23:59:59', '2023-10-10 00:00:01');
   ```
   Resultado: `5` (la función ignora la hora y solo considera las fechas).

---

### Nota importante:
- **`DATEDIFF`** siempre devuelve la diferencia en días, incluso si las fechas tienen horas, minutos o segundos diferentes.
- Si necesitas calcular la diferencia en horas, minutos o segundos, puedes usar otras funciones como `TIMESTAMPDIFF`.

---


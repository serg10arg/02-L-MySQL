
### La Sentencia SQL:
```sql
SELECT EXTRACT(YEAR FROM '2019-09-18 22:19:05');
```

### ¿Qué hace esta sentencia?

1. **SELECT**: Es una palabra clave en SQL que se utiliza para seleccionar datos. En este caso, estamos seleccionando el resultado de la función `EXTRACT`.

2. **EXTRACT**: Es una función en SQL que se utiliza para extraer una parte específica de una fecha o hora (como el año, mes, día, hora, etc.). Toma dos argumentos:
    - **Primer argumento**: La parte que se desea extraer (en este caso, `YEAR`).
    - **Segundo argumento**: La fecha o hora de la cual se extraerá la parte (en este caso, `'2019-09-18 22:19:05'`).

---

### Desglosando los argumentos:

#### 1. `EXTRACT(YEAR FROM '2019-09-18 22:19:05')`:
- La función `EXTRACT` extrae el **año** de la fecha y hora proporcionada.
- La fecha y hora proporcionada es `'2019-09-18 22:19:05'`.
- El año en esta fecha es `2019`.
- Por lo tanto, `EXTRACT(YEAR FROM '2019-09-18 22:19:05')` devuelve `2019`.

---

### Resultado:
```sql
+------------------------------------------+
| EXTRACT(YEAR FROM '2019-09-18 22:19:05') |
+------------------------------------------+
|                                     2019 |
+------------------------------------------+
```

---

### Resumen:
- **`EXTRACT`**: Extrae una parte específica de una fecha o hora (como el año, mes, día, hora, etc.).
- En este caso, `EXTRACT(YEAR FROM '2019-09-18 22:19:05')` devuelve `2019`, porque el año en la fecha proporcionada es `2019`.

---

### ¿Para qué sirve esto?
- La función `EXTRACT` es útil cuando necesitas trabajar con partes específicas de una fecha o hora. Por ejemplo:
    - Extraer el año de una fecha para agrupar datos por año.
    - Extraer el mes para realizar análisis mensuales.
    - Extraer la hora para filtrar eventos en un momento específico del día.

---

### Ejemplos adicionales de `EXTRACT`:

1. Extraer el mes de una fecha:
   ```sql
   SELECT EXTRACT(MONTH FROM '2019-09-18 22:19:05');
   ```
   Resultado: `9` (porque septiembre es el noveno mes).

2. Extraer el día de una fecha:
   ```sql
   SELECT EXTRACT(DAY FROM '2019-09-18 22:19:05');
   ```
   Resultado: `18`.

3. Extraer la hora de una fecha y hora:
   ```sql
   SELECT EXTRACT(HOUR FROM '2019-09-18 22:19:05');
   ```
   Resultado: `22`.

4. Extraer los minutos de una fecha y hora:
   ```sql
   SELECT EXTRACT(MINUTE FROM '2019-09-18 22:19:05');
   ```
   Resultado: `19`.

5. Extraer los segundos de una fecha y hora:
   ```sql
   SELECT EXTRACT(SECOND FROM '2019-09-18 22:19:05');
   ```
   Resultado: `5`.

---

### Partes que se pueden extraer con `EXTRACT`:
- **`YEAR`**: Año.
- **`MONTH`**: Mes (1-12).
- **`DAY`**: Día del mes (1-31).
- **`HOUR`**: Hora (0-23).
- **`MINUTE`**: Minutos (0-59).
- **`SECOND`**: Segundos (0-59).

---


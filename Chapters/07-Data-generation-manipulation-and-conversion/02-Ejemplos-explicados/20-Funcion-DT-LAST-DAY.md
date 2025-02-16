
### La Sentencia SQL:
```sql
SELECT LAST_DAY('2019-09-17');
```

### ¿Qué hace esta sentencia?

1. **SELECT**: Es una palabra clave en SQL que se utiliza para seleccionar datos. En este caso, estamos seleccionando el resultado de la función `LAST_DAY`.

2. **LAST_DAY**: Es una función en SQL que devuelve el **último día del mes** para una fecha dada. Toma un argumento:
    - **Argumento**: Una fecha (en formato `DATE` o una cadena que pueda convertirse a `DATE`). En este caso, es `'2019-09-17'`.

---

### Desglosando los argumentos:

#### 1. `LAST_DAY('2019-09-17')`:
- La fecha proporcionada es `'2019-09-17'`.
- La función `LAST_DAY` calcula el último día del mes correspondiente a esa fecha.
- Para septiembre de 2019, el último día es `30`.
- Por lo tanto, `LAST_DAY('2019-09-17')` devuelve `2019-09-30`.

---

### Resultado:
```sql
+------------------------+
| LAST_DAY('2019-09-17') |
+------------------------+
| 2019-09-30             |
+------------------------+
```

---

### Resumen:
- **`LAST_DAY`**: Devuelve el último día del mes para una fecha dada.
- En este caso, `LAST_DAY('2019-09-17')` devuelve `2019-09-30`, porque el último día de septiembre de 2019 es el 30.

---

### ¿Para qué sirve esto?
- La función `LAST_DAY` es útil cuando necesitas trabajar con el último día de un mes. Por ejemplo:
    - Calcular el último día del mes actual.
    - Generar informes que cubran todo el mes.
    - Realizar cálculos que dependen del final del mes.

---

### Ejemplos adicionales de `LAST_DAY`:

1. Último día del mes actual:
   ```sql
   SELECT LAST_DAY(CURRENT_DATE());
   ```
   Si hoy es `2023-10-15`, el resultado sería `2023-10-31`.

2. Último día de febrero en un año bisiesto:
   ```sql
   SELECT LAST_DAY('2024-02-15');
   ```
   Resultado: `2024-02-29`.

3. Último día de un mes con 31 días:
   ```sql
   SELECT LAST_DAY('2023-01-15');
   ```
   Resultado: `2023-01-31`.

---

### Casos especiales:
- **Febrero**: Depende de si el año es bisiesto o no. En un año bisiesto, el último día es `29`; en un año no bisiesto, es `28`.
- **Meses con 30 o 31 días**: La función `LAST_DAY` siempre devuelve el día correcto, ya sea `30` o `31`.

---


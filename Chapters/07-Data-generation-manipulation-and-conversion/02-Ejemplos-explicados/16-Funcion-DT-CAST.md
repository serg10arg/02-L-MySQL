
### La Sentencia SQL:
```sql
SELECT CAST('2019-09-17' AS DATE) date_field,
       CAST('108:17:57' AS TIME) time_field;
```

### ¿Qué hace esta sentencia?

1. **SELECT**: Es una palabra clave en SQL que se utiliza para seleccionar datos. En este caso, estamos seleccionando dos columnas:
    - `date_field`: El resultado de convertir la cadena `'2019-09-17'` a un tipo de dato `DATE`.
    - `time_field`: El resultado de convertir la cadena `'108:17:57'` a un tipo de dato `TIME`.

2. **CAST**: Es una función en SQL que se utiliza para convertir un valor de un tipo de dato a otro. En este caso, estamos convirtiendo cadenas de texto (`VARCHAR`) a tipos de dato `DATE` y `TIME`.

---

### Desglosando los argumentos:

#### 1. `CAST('2019-09-17' AS DATE)`:
- La cadena `'2019-09-17'` representa una fecha en formato `YYYY-MM-DD`.
- La función `CAST` convierte esta cadena en un tipo de dato `DATE`, que es un formato específico para almacenar fechas en SQL.
- El resultado es `2019-09-17`, que es una fecha válida.

#### 2. `CAST('108:17:57' AS TIME)`:
- La cadena `'108:17:57'` representa una duración en formato `HH:MM:SS`.
- La función `CAST` convierte esta cadena en un tipo de dato `TIME`, que es un formato específico para almacenar horas o duraciones en SQL.
- En SQL, el tipo `TIME` permite valores de horas mayores a 24 (por ejemplo, para representar duraciones). Por eso, `108:17:57` es un valor válido y se interpreta como 108 horas, 17 minutos y 57 segundos.

---

### Resultado:
```sql
+------------+------------+
| date_field | time_field |
+------------+------------+
| 2019-09-17 | 108:17:57  |
+------------+------------+
```

---

### Resumen:
- **`CAST('2019-09-17' AS DATE)`**:
    - Convierte la cadena `'2019-09-17'` en un tipo de dato `DATE`.
    - El resultado es `2019-09-17`, que es una fecha válida.
- **`CAST('108:17:57' AS TIME)`**:
    - Convierte la cadena `'108:17:57'` en un tipo de dato `TIME`.
    - El resultado es `108:17:57`, que representa una duración de 108 horas, 17 minutos y 57 segundos.

---

### ¿Para qué sirve esto?
- **`CAST`**: Es útil cuando necesitas convertir un valor de un tipo de dato a otro. Por ejemplo:
    - Convertir una cadena de texto en una fecha (`DATE`) para realizar operaciones de fecha.
    - Convertir una cadena de texto en una hora (`TIME`) para realizar operaciones de tiempo o duración.

---

### Nota sobre el formato `TIME`:
- En SQL, el tipo `TIME` no solo se usa para horas del día (como `12:34:56`), sino también para duraciones (como `108:17:57`).
- Si el valor de horas es mayor a 24, se interpreta como una duración en horas, minutos y segundos.

---


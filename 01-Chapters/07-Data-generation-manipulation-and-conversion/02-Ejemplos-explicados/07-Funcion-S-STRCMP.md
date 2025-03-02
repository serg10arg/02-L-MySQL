
---

### **1. Eliminar todos los datos de la tabla**

```sql
DELETE FROM string_tbl;
```

#### **¿Qué hace esta consulta?**
- Elimina todas las filas de la tabla `string_tbl`.
- En este caso, se eliminó **1 fila**.

#### **Resultado**
```sql
Query OK, 1 row affected (0.00 sec)
```

---

### **2. Insertar nuevas filas en la tabla**

```sql
INSERT INTO string_tbl(vchar_fld)
VALUES ('abcd'),
       ('xyz'),
       ('QRSTUV'),
       ('qrstuv'),
       ('12345');
```

#### **¿Qué hace esta consulta?**
- Inserta 5 filas en la tabla `string_tbl`, especificando valores para el campo `vchar_fld`.
- Los valores insertados son: `'abcd'`, `'xyz'`, `'QRSTUV'`, `'qrstuv'`, y `'12345'`.

#### **Resultado**
```sql
Query OK, 5 rows affected (0.05 sec)
Records: 5  Duplicates: 0  Warnings: 0
```

---

### **3. Seleccionar y ordenar los valores de `vchar_fld`**

```sql
SELECT vchar_fld
FROM string_tbl
ORDER BY vchar_fld;
```

#### **¿Qué hace esta consulta?**
- Selecciona todos los valores del campo `vchar_fld`.
- Ordena los valores alfabéticamente en orden ascendente.

#### **Resultado**
```sql
+-----------+
| vchar_fld |
+-----------+
| 12345     |
| abcd      |
| QRSTUV    |
| qrstuv    |
| xyz       |
+-----------+
5 rows in set (0.00 sec)
```

- **Explicación**:
    - Los valores se ordenan alfabéticamente:
        1. `'12345'` (los números tienen prioridad sobre las letras).
        2. `'abcd'` (las letras minúsculas tienen prioridad sobre las mayúsculas).
        3. `'QRSTUV'` (las letras mayúsculas vienen después de las minúsculas).
        4. `'qrstuv'` (las letras minúsculas vienen después de las mayúsculas).
        5. `'xyz'` (las letras minúsculas vienen después de las mayúsculas).

---

### **4. Comparar cadenas con `STRCMP`**

```sql
SELECT STRCMP('12345','12345') 12345_12345,
       STRCMP('abcd','xyz') abcd_xyz,
       STRCMP('abcd','QRSTUV') abcd_QRSTUV,
       STRCMP('qrstuv','QRSTUV') qrstuv_QRSTUV,
       STRCMP('12345','xyz') 12345_xyz,
       STRCMP('xyz','qrstuv') xyz_qrstuv;
```

#### **¿Qué hace esta consulta?**
- **`STRCMP(cadena1, cadena2)`**:
    - Compara dos cadenas (`cadena1` y `cadena2`) y devuelve:
        - **0**: Si las cadenas son iguales.
        - **-1**: Si `cadena1` es menor que `cadena2` (en orden alfabético).
        - **1**: Si `cadena1` es mayor que `cadena2` (en orden alfabético).

#### **Resultado**
```sql
+-------------+----------+-------------+---------------+-----------+------------+
| 12345_12345 | abcd_xyz | abcd_QRSTUV | qrstuv_QRSTUV | 12345_xyz | xyz_qrstuv |
+-------------+----------+-------------+---------------+-----------+------------+
|           0 |       -1 |          -1 |             0 |        -1 |          1 |
+-------------+----------+-------------+---------------+-----------+------------+
1 row in set (0.00 sec)
```

---

### **Explicación de los Resultados**

1. **`STRCMP('12345','12345')`**:
    - Las cadenas son iguales.
    - **Resultado**: `0`.

2. **`STRCMP('abcd','xyz')`**:
    - `'abcd'` es menor que `'xyz'` en orden alfabético.
    - **Resultado**: `-1`.

3. **`STRCMP('abcd','QRSTUV')`**:
    - `'abcd'` es menor que `'QRSTUV'` en orden alfabético (las letras minúsculas tienen prioridad sobre las mayúsculas).
    - **Resultado**: `-1`.

4. **`STRCMP('qrstuv','QRSTUV')`**:
    - Las cadenas son iguales (la comparación es **insensible a mayúsculas y minúsculas**).
    - **Resultado**: `0`.

5. **`STRCMP('12345','xyz')`**:
    - `'12345'` es menor que `'xyz'` en orden alfabético (los números tienen prioridad sobre las letras).
    - **Resultado**: `-1`.

6. **`STRCMP('xyz','qrstuv')`**:
    - `'xyz'` es mayor que `'qrstuv'` en orden alfabético.
    - **Resultado**: `1`.

---

### **Resumen**

- **`STRCMP`**:
    - Compara dos cadenas y devuelve:
        - `0` si son iguales.
        - `-1` si la primera cadena es menor que la segunda.
        - `1` si la primera cadena es mayor que la segunda.
    - La comparación es **insensible a mayúsculas y minúsculas**.

- **Orden alfabético**:
    - Los números tienen prioridad sobre las letras.
    - Las letras minúsculas tienen prioridad sobre las mayúsculas.

---

### **Notas Importantes**

1. **Sensibilidad a mayúsculas y minúsculas**:
    - La función `STRCMP` es **insensible a mayúsculas y minúsculas**.
    - Por eso, `'qrstuv'` y `'QRSTUV'` se consideran iguales.

2. **Orden de prioridad**:
    - Números < Letras minúsculas < Letras mayúsculas.

3. **Uso de `STRCMP`**:
    - Es útil para comparar cadenas en consultas SQL, especialmente cuando necesitas determinar el orden alfabético entre dos cadenas.

---

### **¿Qué es la colación (collation)?**

La **colación** es un conjunto de reglas que define cómo se comparan y ordenan las cadenas de texto. En MySQL, la colación determina:
1. El orden de los caracteres (números, letras minúsculas, letras mayúsculas, etc.).
2. Si la comparación es sensible a mayúsculas y minúsculas.
3. Si se tienen en cuenta los acentos y otros caracteres especiales.

---

### **Colación por defecto en MySQL**

En MySQL, la colación por defecto suele ser **`utf8mb4_general_ci`** o **`utf8mb4_unicode_ci`**. Estas colaciones son **insensibles a mayúsculas y minúsculas** (el sufijo `_ci` significa _case-insensitive_).

- **`utf8mb4_general_ci`**:
    - Ordena los caracteres de manera general, sin distinción entre mayúsculas y minúsculas.
    - Los números tienen prioridad sobre las letras.
    - Las letras minúsculas y mayúsculas se tratan como iguales.

- **`utf8mb4_unicode_ci`**:
    - Ordena los caracteres según las reglas Unicode, lo que permite un manejo más preciso de caracteres especiales y acentos.
    - También es insensible a mayúsculas y minúsculas.

---

### **¿Por qué `'qrstuv'` y `'xyz'` están después de `'QRSTUV'`?**

En la colación **insensible a mayúsculas y minúsculas** (`_ci`), las letras minúsculas y mayúsculas se tratan como **iguales**. Por lo tanto:
- `'qrstuv'` y `'QRSTUV'` se consideran **iguales**.
- `'xyz'` y `'XYZ'` se considerarían **iguales**.

Sin embargo, en el resultado que obtuviste:

```sql
+-----------+
| vchar_fld |
+-----------+
| 12345     |
| abcd      |
| QRSTUV    |
| qrstuv    |
| xyz       |
+-----------+
```

El orden no es estrictamente alfabético en términos de mayúsculas y minúsculas, sino que depende de cómo MySQL implementa la colación. En este caso:
1. **`'12345'`**: Los números tienen prioridad sobre las letras.
2. **`'abcd'`**: Las letras minúsculas tienen prioridad sobre las mayúsculas.
3. **`'QRSTUV'`**: Aunque `'QRSTUV'` y `'qrstuv'` son iguales en una colación insensible a mayúsculas y minúsculas, MySQL puede mostrar primero la versión en mayúsculas debido a cómo se implementa internamente la colación.
4. **`'qrstuv'`**: Se muestra después de `'QRSTUV'` porque MySQL las considera iguales, pero puede decidir mostrar primero la versión en mayúsculas.
5. **`'xyz'`**: Se muestra al final porque es la última cadena en orden alfabético.

---

### **¿Cómo asegurar un orden específico?**

Si deseas un orden **sensible a mayúsculas y minúsculas**, puedes usar una colación que distinga entre mayúsculas y minúsculas, como **`utf8mb4_bin`** o **`utf8mb4_0900_as_cs`**.

#### **Ejemplo con colación sensible a mayúsculas y minúsculas**:

```sql
SELECT vchar_fld
FROM string_tbl
ORDER BY vchar_fld COLLATE utf8mb4_bin;
```

#### **Resultado esperado**:
```sql
+-----------+
| vchar_fld |
+-----------+
| 12345     |
| QRSTUV    |
| abcd      |
| qrstuv    |
| xyz       |
+-----------+
```

- **Explicación**:
    - `'QRSTUV'` aparece antes que `'abcd'` porque las mayúsculas tienen prioridad sobre las minúsculas en una colación sensible a mayúsculas y minúsculas.

---

### **Resumen**

1. **Colación insensible a mayúsculas y minúsculas**:
    - `'qrstuv'` y `'QRSTUV'` se consideran iguales.
    - MySQL puede mostrar primero la versión en mayúsculas debido a la implementación interna de la colación.

2. **Colación sensible a mayúsculas y minúsculas**:
    - Las mayúsculas tienen prioridad sobre las minúsculas.
    - `'QRSTUV'` aparecería antes que `'abcd'`.

3. **Orden alfabético**:
    - Los números tienen prioridad sobre las letras.
    - Las letras minúsculas tienen prioridad sobre las mayúsculas en colaciones insensibles a mayúsculas y minúsculas.

---

### **Conclusión**

El orden que obtuviste (`'12345'`, `'abcd'`, `'QRSTUV'`, `'qrstuv'`, `'xyz'`) es el resultado de la colación **insensible a mayúsculas y minúsculas** (`_ci`). Si necesitas un orden específico (por ejemplo, sensible a mayúsculas y minúsculas), puedes cambiar la colación en la consulta.


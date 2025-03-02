
---

### **Consulta SQL**

```sql
SELECT INSERT('goodbye world', 9, 0, 'cruel ') string;
```

---

### **¿Qué hace esta consulta?**

1. **Función `INSERT`**:
    - La función `INSERT` se utiliza para insertar una cadena dentro de otra cadena en una posición específica.
    - Sintaxis: `INSERT(cadena_original, posición, longitud, cadena_a_insertar)`.
        - **`cadena_original`**: La cadena en la que se realizará la inserción.
        - **`posición`**: La posición en la cadena original donde se insertará la nueva cadena.
        - **`longitud`**: El número de caracteres que se reemplazarán en la cadena original. Si es `0`, no se reemplaza ningún carácter, solo se inserta la nueva cadena.
        - **`cadena_a_insertar`**: La cadena que se insertará.

2. **Parámetros en esta consulta**:
    - **`cadena_original`**: `'goodbye world'`.
    - **`posición`**: `9`.
    - **`longitud`**: `0` (no se reemplaza ningún carácter).
    - **`cadena_a_insertar`**: `'cruel '`.

3. **Efecto**:
    - La cadena `'cruel '` se inserta en la posición **9** de la cadena `'goodbye world'`.
    - Como la longitud es `0`, no se reemplaza ningún carácter de la cadena original.

---

### **Resultado de la Consulta**

```sql
+---------------------+
| string              |
+---------------------+
| goodbye cruel world |
+---------------------+
1 row in set (0.00 sec)
```

- **Explicación**:
    - La cadena original es `'goodbye world'`.
    - En la posición **9** de la cadena original, se inserta `'cruel '`.
    - El resultado es `'goodbye cruel world'`.

---

### **Desglose de la Cadena**

Para entender mejor, vamos a numerar las posiciones de la cadena original:

| Posición | Carácter |
|----------|----------|
| 1        | g        |
| 2        | o        |
| 3        | o        |
| 4        | d        |
| 5        | b        |
| 6        | y        |
| 7        | e        |
| 8        | (espacio)|
| 9        | w        |
| 10       | o        |
| 11       | r        |
| 12       | l        |
| 13       | d        |

- **Inserción**:
    - En la posición **9**, se inserta `'cruel '`.
    - La cadena resultante es:
        - `'goodbye '` (posiciones 1 a 8).
        - `'cruel '` (cadena insertada).
        - `'world'` (posiciones 9 a 13 de la cadena original).

---

### **Resumen**

1. **Función `INSERT`**:
    - Inserta una cadena dentro de otra cadena en una posición específica.
    - Si la longitud es `0`, no se reemplaza ningún carácter, solo se inserta la nueva cadena.

2. **Resultado**:
    - La cadena `'goodbye world'` se convierte en `'goodbye cruel world'`.

---

### **Notas Importantes**

1. **Posición**:
    - La posición se cuenta desde **1** (no desde 0).
    - En este caso, la posición **9** corresponde al carácter `'w'` en `'goodbye world'`.

2. **Longitud**:
    - Si la longitud es mayor que `0`, se reemplazarán los caracteres en la cadena original.
    - Por ejemplo, si la consulta fuera:
      ```sql
      SELECT INSERT('goodbye world', 9, 5, 'cruel ') string;
      ```
      El resultado sería:
      ```sql
      +---------------------+
      | string              |
      +---------------------+
      | goodbye cruel rld   |
      +---------------------+
      ```
        - Aquí, se reemplazan **5 caracteres** a partir de la posición **9** (`'world'`) con `'cruel '`.

3. **Uso de `INSERT`**:
    - Es útil para modificar cadenas en SQL, especialmente cuando necesitas insertar o reemplazar partes de una cadena.

---

### **Ejemplo Adicional**

Si quisieras reemplazar parte de la cadena en lugar de solo insertar, podrías usar una longitud mayor que `0`. Por ejemplo:

```sql
SELECT INSERT('goodbye world', 9, 5, 'hello') string;
```

#### **Resultado**:
```sql
+-------------------+
| string            |
+-------------------+
| goodbye hello     |
+-------------------+
```

- **Explicación**:
    - Se reemplazan **5 caracteres** a partir de la posición **9** (`'world'`) con `'hello'`.
    - El resultado es `'goodbye hello'`.

---


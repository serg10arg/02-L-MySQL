
---

### **Consulta SQL**

```sql
SELECT LENGTH(char_fld) char_length,
       LENGTH(vchar_fld) varchar_length,
       LENGTH(text_fld) text_length
FROM string_tbl;
```

---

### **¿Qué hace esta consulta?**

1. **`LENGTH()`**:
    - La función `LENGTH()` devuelve la longitud de una cadena en **bytes**.
    - Es importante destacar que `LENGTH()` mide la longitud en bytes, no en caracteres. Esto es relevante si la cadena contiene caracteres multibyte (como caracteres UTF-8 que ocupan más de un byte).

2. **Campos involucrados**:
    - **`char_fld`**: Es un campo de tipo `CHAR`. Los campos `CHAR` tienen una longitud fija y rellenan con espacios si la cadena es más corta que la longitud definida.
    - **`vchar_fld`**: Es un campo de tipo `VARCHAR`. Los campos `VARCHAR` tienen una longitud variable y solo ocupan el espacio necesario para almacenar la cadena.
    - **`text_fld`**: Es un campo de tipo `TEXT`. Los campos `TEXT` también tienen una longitud variable y están diseñados para almacenar cadenas largas.

3. **Resultado**:
    - La consulta devuelve la longitud en bytes de cada campo.

---

### **Resultado de la Consulta**

```sql
+-------------+----------------+-------------+
| char_length | varchar_length | text_length |
+-------------+----------------+-------------+
|          28 |             28 |          28 |
+-------------+----------------+-------------+
1 row in set (0.00 sec)
```

---

### **¿Por qué los tres campos tienen la misma longitud?**

1. **Caso 1: Todos los campos contienen la misma cadena**:
    - Si los tres campos (`char_fld`, `vchar_fld`, y `text_fld`) contienen exactamente la misma cadena, entonces la longitud en bytes será la misma para todos.
    - Por ejemplo, si la cadena es `"This is a sample string with 28 bytes"`, entonces:
        - `char_fld`: Almacena la cadena y la rellena con espacios si es necesario (pero en este caso, la cadena ya tiene 28 bytes).
        - `vchar_fld`: Almacena la cadena sin relleno.
        - `text_fld`: Almacena la cadena sin relleno.

2. **Caso 2: La cadena no tiene caracteres multibyte**:
    - Si la cadena está compuesta únicamente por caracteres ASCII (que ocupan 1 byte cada uno), entonces la longitud en bytes será igual a la longitud en caracteres.
    - Por ejemplo, la cadena `"abcdefghijklmnopqrstuvwxyz12"` tiene 28 caracteres y ocupa 28 bytes.

3. **Caso 3: La cadena tiene caracteres multibyte**:
    - Si la cadena contiene caracteres multibyte (por ejemplo, caracteres UTF-8 como `'ñ'` o `'ö'`), la longitud en bytes será mayor que la longitud en caracteres.
    - Sin embargo, en este caso, los tres campos aún tendrían la misma longitud en bytes, ya que todos almacenan la misma cadena.

---

### **Ejemplo Práctico**

Supongamos que la tabla `string_tbl` tiene la siguiente fila:

| char_fld                          | vchar_fld                         | text_fld                          |
|-----------------------------------|-----------------------------------|-----------------------------------|
| "This is a sample string with 28" | "This is a sample string with 28" | "This is a sample string with 28" |

- La cadena `"This is a sample string with 28"` tiene **28 caracteres** y ocupa **28 bytes** (porque todos los caracteres son ASCII).

---

### **Resumen**

- **`LENGTH()`** mide la longitud en bytes, no en caracteres.
- Si los tres campos (`char_fld`, `vchar_fld`, y `text_fld`) contienen la misma cadena y la cadena no tiene caracteres multibyte, entonces la longitud en bytes será la misma para todos.
- La diferencia entre `CHAR`, `VARCHAR`, y `TEXT` no afecta la longitud en bytes de la cadena almacenada, sino cómo se almacena y gestiona internamente.

---

### **Notas Importantes**

1. **Diferencias entre `CHAR`, `VARCHAR`, y `TEXT`**:
    - **`CHAR`**: Longitud fija. Si defines un `CHAR(50)` y almacenas una cadena de 10 caracteres, ocupará 50 bytes (rellenando con espacios).
    - **`VARCHAR`**: Longitud variable. Si defines un `VARCHAR(50)` y almacenas una cadena de 10 caracteres, ocupará solo 10 bytes (más 1 o 2 bytes adicionales para almacenar la longitud).
    - **`TEXT`**: Longitud variable. Diseñado para cadenas largas. No tiene un límite fijo de longitud.

2. **Longitud en bytes vs longitud en caracteres**:
    - Si la cadena contiene caracteres multibyte (como UTF-8), la longitud en bytes será mayor que la longitud en caracteres.
    - Puedes usar la función `CHAR_LENGTH()` para obtener la longitud en caracteres en lugar de bytes.

---

### **Ejemplo con `CHAR_LENGTH()`**

Si quieres medir la longitud en caracteres en lugar de bytes, puedes usar `CHAR_LENGTH()`:

```sql
SELECT CHAR_LENGTH(char_fld) char_length,
       CHAR_LENGTH(vchar_fld) varchar_length,
       CHAR_LENGTH(text_fld) text_length
FROM string_tbl;
```

---


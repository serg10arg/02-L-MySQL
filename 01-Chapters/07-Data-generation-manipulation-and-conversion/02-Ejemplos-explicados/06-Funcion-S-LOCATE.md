
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
Query OK, 1 row affected (0.02 sec)
```

---

### **2. Insertar una nueva fila en la tabla**

```sql
INSERT INTO string_tbl (char_fld, vchar_fld, text_fld)
VALUES ('This string is 28 characters',
        'This string is 28 characters',
        'This string is 28 characters');
```

#### **¿Qué hace esta consulta?**
- Inserta una nueva fila en la tabla `string_tbl` con los siguientes valores:
    - `char_fld`: `'This string is 28 characters'`.
    - `vchar_fld`: `'This string is 28 characters'`.
    - `text_fld`: `'This string is 28 characters'`.

#### **Resultado**
```sql
Query OK, 1 row affected (0.00 sec)
```

---

### **3. Buscar la subcadena `'is'` a partir de la posición 5**

```sql
SELECT LOCATE('is', vchar_fld, 5)
FROM string_tbl;
```

#### **¿Qué hace esta consulta?**
- **`LOCATE('is', vchar_fld, 5)`**:
    - Busca la subcadena `'is'` dentro del campo `vchar_fld`.
    - La búsqueda comienza desde la posición **5**.
    - Devuelve la posición (índice) de la primera ocurrencia de `'is'` a partir de la posición **5**.

#### **Resultado**
```sql
+----------------------------+
| LOCATE('is', vchar_fld, 5) |
+----------------------------+
|                         13 |
+----------------------------+
1 row in set (0.02 sec)
```

---

### **¿Por qué el resultado es 13?**

Para entender por qué la función `LOCATE` devuelve **13**, vamos a desglosar la cadena almacenada en `vchar_fld` y numerar cada carácter:

| Posición | Carácter |
|----------|----------|
| 1        | T        |
| 2        | h        |
| 3        | i        |
| 4        | s        |
| 5        | (espacio)|
| 6        | s        |
| 7        | t        |
| 8        | r        |
| 9        | i        |
| 10       | n        |
| 11       | g        |
| 12       | (espacio)|
| 13       | i        |
| 14       | s        |
| 15       | (espacio)|
| 16       | 2        |
| 17       | 8        |
| 18       | (espacio)|
| 19       | c        |
| 20       | h        |
| 21       | a        |
| 22       | r        |
| 23       | a        |
| 24       | c        |
| 25       | t        |
| 26       | e        |
| 27       | r        |
| 28       | s        |

---

### **Búsqueda de la Subcadena `'is'`**

1. **La búsqueda comienza desde la posición 5**:
    - Ignoramos las posiciones **1** a **4** porque la búsqueda comienza desde la posición **5**.

2. **Avanzamos hasta la posición 13**:
    - En la posición **13**, encontramos el carácter `'i'`.
    - Verificamos si los siguientes caracteres coinciden con `'is'`:
        - Posición 13: `'i'` (coincide con el primer carácter de `'is'`).
        - Posición 14: `'s'` (coincide con el segundo carácter de `'is'`).

3. **Conclusión**:
    - La subcadena `'is'` comienza en la posición **13**.

---

### **Resumen**

- **`LOCATE('is', vchar_fld, 5)`**:
    - Busca la subcadena `'is'` dentro del campo `vchar_fld`.
    - La búsqueda comienza desde la posición **5**.
    - Devuelve la posición **13** porque es donde comienza la primera ocurrencia de `'is'` a partir de la posición **5**.

---

### **Notas Importantes**

1. **Conteo de posiciones**:
    - Las posiciones se cuentan desde **1**.
    - Cada carácter (incluyendo espacios) se cuenta como una posición.

2. **Sensibilidad a mayúsculas y minúsculas**:
    - La función `LOCATE` es sensible a mayúsculas y minúsculas. Si la subcadena no coincide exactamente (en cuanto a mayúsculas y minúsculas), no se encontrará.

3. **Si la subcadena no se encuentra**:
    - Si la subcadena no está presente en la cadena (o no se encuentra después de la posición especificada), la función devuelve **0**.

---

### **Ejemplo Adicional**

Si la cadena fuera `"This is a test string"` y ejecutaras:

```sql
SELECT LOCATE('is', vchar_fld, 5);
```

El resultado sería **6**, porque:

- La primera ocurrencia de `'is'` está en la posición **3** (`"This"`), pero como la búsqueda comienza desde la posición **5**, se ignora esa ocurrencia.
- La siguiente ocurrencia de `'is'` comienza en la posición **6** (`"is a test string"`).

---


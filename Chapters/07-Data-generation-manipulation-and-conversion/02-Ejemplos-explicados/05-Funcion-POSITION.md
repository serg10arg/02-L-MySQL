
---

### **Consulta SQL**

```sql
SELECT POSITION('characters' IN vchar_fld)
FROM string_tbl;
```

---

### **¿Qué hace esta consulta?**

1. **Función `POSITION()`**:
    - La función `POSITION(subcadena IN cadena)` devuelve la **posición** (índice) de la primera ocurrencia de la `subcadena` dentro de la `cadena`.
    - La posición se cuenta desde **1** (no desde 0, como en algunos lenguajes de programación).
    - Si la subcadena no se encuentra en la cadena, la función devuelve **0**.

2. **`'characters' IN vchar_fld`**:
    - Aquí, `'characters'` es la subcadena que se está buscando.
    - `vchar_fld` es el campo de la tabla `string_tbl` donde se realiza la búsqueda.

3. **Resultado**:
    - La consulta devuelve la posición de la primera ocurrencia de `'characters'` dentro del valor del campo `vchar_fld`.

---

### **Resultado de la Consulta**

```sql
+-------------------------------------+
| POSITION('characters' IN vchar_fld) |
+-------------------------------------+
|                                  19 |
+-------------------------------------+
1 row in set (0.12 sec)
```

---

### **¿Qué significa este resultado?**

- El valor devuelto es **19**, lo que significa que la subcadena `'characters'` comienza en la posición **19** del valor almacenado en el campo `vchar_fld`.

---

### **Ejemplo Práctico**

Supongamos que la tabla `string_tbl` tiene la siguiente fila:

| vchar_fld                                      |
|------------------------------------------------|
| "This string is 28 characters" |

- La cadena almacenada en `vchar_fld` es: `"This string is 28 characters"`.
- Si buscamos la subcadena `'characters'` dentro de esta cadena:
    - La subcadena `'characters'` comienza en la posición **19** de la cadena.

---

### **Desglose de la Cadena**

Para entender mejor, podemos desglosar la cadena y numerar las posiciones de cada carácter:

```
Posición: 1   -   -    -    19   -   -   
          |   |    |    |   |    |    |    
Cadena:   This string is 28 characters
```

- La subcadena `'characters'` comienza en la posición **19**:
    - `'c'` está en la posición **19**.
    - `'h'` está en la posición **20**.
    - `'a'` está en la posición **21**.
    - Y así sucesivamente.

---

### **Resumen**

- **`POSITION('characters' IN vchar_fld)`**:
    - Busca la subcadena `'characters'` dentro del valor del campo `vchar_fld`.
    - Devuelve la posición (índice) de la primera ocurrencia de la subcadena.
    - En este caso, la subcadena `'characters'` comienza en la posición **19**.

---

### **Notas Importantes**

1. **Búsqueda sensible a mayúsculas y minúsculas**:
    - La función `POSITION` es sensible a mayúsculas y minúsculas. Si la subcadena no coincide exactamente (en cuanto a mayúsculas y minúsculas), no se encontrará.

2. **Si la subcadena no se encuentra**:
    - Si la subcadena no está presente en la cadena, la función devuelve **0**.

3. **Alternativa: `LOCATE()`**:
    - MySQL también tiene una función llamada `LOCATE(subcadena, cadena, inicio)`, que es similar a `POSITION`, pero permite especificar una posición de inicio para la búsqueda.
    - Ejemplo:
      ```sql
      SELECT LOCATE('characters', vchar_fld, 10);
      ```
      Esto buscaría `'characters'` a partir de la posición **10**.

---

### **Ejemplo con `LOCATE()`**

Si quisieras buscar `'characters'` a partir de la posición **20**, podrías usar:

```sql
SELECT LOCATE('characters', vchar_fld, 20)
FROM string_tbl;
```

- En este caso, el resultado sería **0**, porque `'characters'` comienza en la posición **19**, y la búsqueda comienza desde la posición **20**.

---


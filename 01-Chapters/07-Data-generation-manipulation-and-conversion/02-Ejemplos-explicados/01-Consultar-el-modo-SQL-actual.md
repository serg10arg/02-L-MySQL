
### **Contexto**

En MySQL, la variable `sql_mode` define el comportamiento del servidor SQL en cuanto a la validación de consultas, el manejo de errores y otras configuraciones. Cambiar el modo SQL puede afectar cómo se ejecutan las consultas y cómo se manejan los datos.

---

### **1. Consultar el modo SQL actual**

```sql
SELECT @@session.sql_mode;
```

#### **¿Qué hace esta consulta?**
- **`@@session.sql_mode`**: Esta variable contiene el modo SQL actual para la sesión en curso.
- **`SELECT @@session.sql_mode`**: Muestra el valor actual de `sql_mode` para la sesión.

#### **Resultado**
```sql
+----------------------------------------------------------------+
| @@session.sql_mode                                             |
+----------------------------------------------------------------+
| STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION                     |
+----------------------------------------------------------------+
1 row in set (0.00 sec)
```

- **Explicación**:
    - El modo SQL actual es `STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION`.
    - **`STRICT_TRANS_TABLES`**: Hace que MySQL sea más estricto al manejar datos inválidos en tablas transaccionales. Por ejemplo, si intentas insertar un valor incorrecto en una columna, MySQL generará un error en lugar de ajustar el valor automáticamente.
    - **`NO_ENGINE_SUBSTITUTION`**: Evita que MySQL sustituya automáticamente un motor de almacenamiento no disponible por otro. Si intentas crear una tabla con un motor no disponible, MySQL generará un error.

---

### **2. Cambiar el modo SQL a `ansi`**

```sql
SET sql_mode='ansi';
```

#### **¿Qué hace esta consulta?**
- **`SET sql_mode='ansi'`**: Cambia el modo SQL de la sesión actual a `ansi`.
- **`ansi`**: Es un modo predefinido en MySQL que habilita varias opciones para cumplir con el estándar ANSI SQL.

#### **Resultado**
```sql
Query OK, 0 rows affected (0.08 sec)
```

- **Explicación**:
    - La consulta se ejecutó correctamente y cambió el modo SQL a `ansi`.

---

### **3. Consultar el nuevo modo SQL**

```sql
SELECT @@session.sql_mode;
```

#### **¿Qué hace esta consulta?**
- Muestra el valor actual de `sql_mode` después de haberlo cambiado a `ansi`.

#### **Resultado**
```sql
+--------------------------------------------------------------------------------+
| @@session.sql_mode                                                             |
+--------------------------------------------------------------------------------+
| REAL_AS_FLOAT,PIPES_AS_CONCAT,ANSI_QUOTES,IGNORE_SPACE,ONLY_FULL_GROUP_BY,ANSI |
+--------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

- **Explicación**:
    - El modo SQL ahora incluye varias opciones que forman parte del modo `ansi`:
        - **`REAL_AS_FLOAT`**: Trata el tipo de datos `REAL` como un sinónimo de `FLOAT`.
        - **`PIPES_AS_CONCAT`**: Trata el operador `||` como un operador de concatenación de cadenas (en lugar de un operador lógico OR).
        - **`ANSI_QUOTES`**: Permite usar comillas dobles (`"`) para delimitar identificadores (por ejemplo, nombres de columnas o tablas).
        - **`IGNORE_SPACE`**: Ignora los espacios en blanco entre el nombre de una función y el paréntesis de apertura (por ejemplo, `COUNT (*)` se trata como `COUNT(*)`).
        - **`ONLY_FULL_GROUP_BY`**: Hace que MySQL sea más estricto al usar `GROUP BY`. Todas las columnas en la cláusula `SELECT` deben estar en la cláusula `GROUP BY` o ser agregadas (por ejemplo, con `COUNT`, `SUM`, etc.).
        - **`ANSI`**: Es un alias que incluye todas las opciones anteriores.

---

### **Resumen**

1. **Consulta inicial del modo SQL**:
    - El modo SQL original era `STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION`.
    - Este modo es útil para garantizar la integridad de los datos y evitar sustituciones automáticas de motores de almacenamiento.

2. **Cambio del modo SQL a `ansi`**:
    - El modo `ansi` habilita varias opciones para cumplir con el estándar ANSI SQL.
    - Esto puede ser útil si estás trabajando con aplicaciones que requieren compatibilidad con SQL estándar.

3. **Consulta del nuevo modo SQL**:
    - Después del cambio, el modo SQL incluye opciones como `REAL_AS_FLOAT`, `PIPES_AS_CONCAT`, `ANSI_QUOTES`, `IGNORE_SPACE`, `ONLY_FULL_GROUP_BY` y `ANSI`.

---

### **Notas Importantes**

1. **Efecto del modo SQL**:
    - Cambiar el modo SQL puede afectar cómo se ejecutan las consultas y cómo se manejan los errores.
    - Por ejemplo, el modo `ONLY_FULL_GROUP_BY` puede hacer que algunas consultas que funcionaban antes fallen si no cumplen con las reglas de `GROUP BY`.

2. **Alcance del cambio**:
    - El cambio de `sql_mode` solo afecta a la sesión actual. Si reinicias la conexión a la base de datos, el modo SQL volverá a su valor predeterminado.
    - Si deseas cambiar el modo SQL de manera permanente, debes modificar la configuración del servidor MySQL.

3. **Modos comunes**:
    - **`STRICT_TRANS_TABLES`**: Útil para garantizar la integridad de los datos.
    - **`ANSI`**: Útil para cumplir con el estándar ANSI SQL.
    - **`TRADITIONAL`**: Similar a `STRICT_TRANS_TABLES`, pero con reglas aún más estrictas.

---


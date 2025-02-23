
---

### **¿Qué es la Lógica Condicional en SQL?**

La **lógica condicional** en SQL permite que una consulta tome decisiones basadas en los valores de ciertas columnas o expresiones. Esto es similar a las estructuras `if-else` en otros lenguajes de programación. En SQL, la herramienta principal para implementar lógica condicional es la expresión `CASE`.

---

### **Expresión `CASE`**

La expresión `CASE` es una estructura que permite evaluar condiciones y devolver un valor dependiendo de si se cumple o no una condición. Puede usarse en varias partes de una consulta, como en la cláusula `SELECT`, `WHERE`, `ORDER BY`, `GROUP BY`, e incluso en declaraciones `INSERT`, `UPDATE` y `DELETE`.

#### Sintaxis Básica:
```sql
CASE
    WHEN condición1 THEN valor1
    WHEN condición2 THEN valor2
    ...
    ELSE valor_por_defecto
END
```

- **`WHEN`:** Evalúa una condición. Si es verdadera, devuelve el valor asociado (`THEN`).
- **`THEN`:** Especifica el valor a devolver si la condición es verdadera.
- **`ELSE`:** Especifica el valor a devolver si ninguna de las condiciones es verdadera (opcional).
- **`END`:** Cierra la expresión `CASE`.

---

### **Ejemplo Práctico**

El texto proporciona un ejemplo en el que se utiliza la expresión `CASE` para traducir el valor de la columna `active` (que almacena `1` para "activo" y `0` para "inactivo") en una cadena legible.

#### Consulta:
```sql
SELECT first_name, last_name,
       CASE
           WHEN active = 1 THEN 'ACTIVO'
           ELSE 'INACTIVO'
       END AS activity_type
FROM customer;
```

#### Resultado:
| first_name | last_name | activity_type |
|------------|-----------|---------------|
| MARY       | SMITH     | ACTIVO        |
| PATRICIA   | JOHNSON   | ACTIVO        |
| LINDA      | WILLIAMS  | ACTIVO        |
| BARBARA    | JONES     | ACTIVO        |
| ELIZABETH  | BROWN     | ACTIVO        |
| JENNIFER   | DAVIS     | ACTIVO        |
| KENT       | ARSENAULT | ACTIVO        |
| TERRANCE   | ROUSH     | INACTIVO      |
| RENE       | MCALISTER | ACTIVO        |
| EDUARDO    | HIATT     | ACTIVO        |

---

### **Explicación del Ejemplo**

1. **Columna `active`:**
    - Almacena `1` si el cliente está activo.
    - Almacena `0` si el cliente está inactivo.

2. **Expresión `CASE`:**
    - Evalúa el valor de `active`.
    - Si `active = 1`, devuelve `'ACTIVO'`.
    - Si no, devuelve `'INACTIVO'`.

3. **Alias `activity_type`:**
    - La expresión `CASE` se etiqueta como `activity_type` para que aparezca como una columna en el resultado.

---

### **Ventajas de Usar `CASE`**

1. **Legibilidad:**
    - Permite traducir valores numéricos o codificados en cadenas legibles.
    - Ejemplo: Convertir `1` y `0` en `'ACTIVO'` e `'INACTIVO'`.

2. **Flexibilidad:**
    - Puede manejar múltiples condiciones y devolver diferentes valores para cada una.
    - Ejemplo:
      ```sql
      CASE
          WHEN active = 1 THEN 'ACTIVO'
          WHEN active = 0 THEN 'INACTIVO'
          ELSE 'DESCONOCIDO'
      END
      ```

3. **Portabilidad:**
    - La expresión `CASE` es estándar en SQL, por lo que funciona en la mayoría de los sistemas de bases de datos (MySQL, PostgreSQL, Oracle, SQL Server, etc.).

---

### **Otros Usos de `CASE`**

La expresión `CASE` no se limita a la cláusula `SELECT`. Aquí hay algunos ejemplos adicionales:

#### 1. En la Cláusula `WHERE`:
```sql
SELECT first_name, last_name
FROM customer
WHERE
    CASE
        WHEN active = 1 THEN last_name LIKE 'S%'
        ELSE last_name LIKE 'M%'
    END;
```
- **Explicación:** Filtra clientes activos cuyo apellido comienza con "S" y clientes inactivos cuyo apellido comienza con "M".

#### 2. En la Cláusula `ORDER BY`:
```sql
SELECT first_name, last_name, active
FROM customer
ORDER BY
    CASE
        WHEN active = 1 THEN last_name
        ELSE first_name
    END;
```
- **Explicación:** Ordena clientes activos por apellido y clientes inactivos por nombre.

#### 3. En la Cláusula `UPDATE`:
```sql
UPDATE customer
SET status =
    CASE
        WHEN active = 1 THEN 'ACTIVO'
        ELSE 'INACTIVO'
    END;
```
- **Explicación:** Actualiza la columna `status` basándose en el valor de `active`.

---

### **Conclusión**

- **Expresión `CASE`:** Es una herramienta poderosa para implementar lógica condicional en SQL.
- **Usos Comunes:** Traducción de valores, filtrado condicional, ordenamiento condicional y actualizaciones condicionales.
- **Ventajas:** Mejora la legibilidad, es flexible y portable.

En resumen, la expresión `CASE` es esencial para manejar lógica condicional en SQL, permitiéndote tomar decisiones basadas en los datos y mejorar la claridad de tus consultas. ¡Es una herramienta que debes dominar para escribir consultas más eficientes y comprensibles! 😊
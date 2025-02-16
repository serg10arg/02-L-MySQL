
---

### **Consulta SQL**

```sql
UPDATE string_tbl
SET text_fld = CONCAT(text_fld, ', but now it is longer');
```

---

### **¿Qué hace esta consulta?**

1. **`UPDATE string_tbl`**:
    - Indica que vamos a actualizar la tabla `string_tbl`.

2. **`SET text_fld = CONCAT(text_fld, ', but now it is longer')`**:
    - **`CONCAT(text_fld, ', but now it is longer')`**:
        - La función `CONCAT` concatena (une) dos o más cadenas.
        - En este caso, toma el valor actual del campo `text_fld` y le añade la cadena `', but now it is longer'`.
    - **`SET text_fld = ...`**:
        - Asigna el resultado de la concatenación al campo `text_fld`.

3. **Efecto**:
    - El valor actual del campo `text_fld` se modifica para incluir la cadena adicional `', but now it is longer'`.

---

### **Resultado de la Consulta**

```sql
Query OK, 1 row affected (0.03 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

- **Explicación**:
    - **`Query OK`**: La consulta se ejecutó correctamente.
    - **`1 row affected`**: Se modificó **1 fila** en la tabla.
    - **`Rows matched: 1`**: La consulta encontró **1 fila** que coincidía con la condición (en este caso, no había una cláusula `WHERE`, por lo que se aplicó a todas las filas).
    - **`Changed: 1`**: Se modificó **1 fila**.
    - **`Warnings: 0`**: No hubo advertencias durante la ejecución.

---

### **Ejemplo Práctico**

Supongamos que la tabla `string_tbl` tiene la siguiente fila antes de la actualización:

| text_fld                     |
|------------------------------|
| This is the original text    |

---

#### **Después de la Actualización**

Después de ejecutar la consulta:

```sql
UPDATE string_tbl
SET text_fld = CONCAT(text_fld, ', but now it is longer');
```

El campo `text_fld` se actualizará a:

| text_fld                                     |
|----------------------------------------------|
| This is the original text, but now it is longer |

---

### **Resumen**

1. **`CONCAT`**:
    - Une dos o más cadenas.
    - En este caso, añade `', but now it is longer'` al valor actual de `text_fld`.

2. **`UPDATE`**:
    - Modifica el valor del campo `text_fld` en la tabla `string_tbl`.

3. **Efecto**:
    - El campo `text_fld` ahora contiene la cadena original más la cadena adicional.

---

### **Notas Importantes**

1. **Concatenación**:
    - La función `CONCAT` es muy útil para modificar cadenas en SQL.
    - Puedes concatenar tantas cadenas como necesites, por ejemplo:
      ```sql
      SET text_fld = CONCAT(text_fld, ' part 1', ' part 2', ' part 3');
      ```

2. **Modificación de todas las filas**:
    - Como no se especificó una cláusula `WHERE`, la actualización se aplicó a **todas las filas** de la tabla.
    - Si solo deseas modificar una fila específica, puedes agregar una condición `WHERE`, por ejemplo:
      ```sql
      UPDATE string_tbl
      SET text_fld = CONCAT(text_fld, ', but now it is longer')
      WHERE id = 1;
      ```

3. **Advertencias**:
    - Si el campo `text_fld` tiene un límite de longitud (por ejemplo, `VARCHAR(255)`), y la concatenación excede ese límite, MySQL truncará la cadena y generará una advertencia.

---


---

### **Consulta SQL**

```sql
SELECT CONCAT(first_name, ' ', last_name,
       ' has been a customer since ', DATE(create_date)) cust_narrative
FROM customer;
```

---

### **¿Qué hace esta consulta?**

1. **`CONCAT`**:
    - La función `CONCAT` une varias cadenas en una sola.
    - En este caso, concatena:
        - El campo `first_name`.
        - Un espacio (`' '`).
        - El campo `last_name`.
        - La cadena `' has been a customer since '`.
        - La fecha formateada del campo `create_date` (usando la función `DATE`).

2. **`DATE(create_date)`**:
    - La función `DATE` extrae la parte de la fecha (sin la hora) del campo `create_date`.

3. **`cust_narrative`**:
    - Es un alias para la columna resultante de la concatenación.

4. **`FROM customer`**:
    - Especifica que estamos trabajando con la tabla `customer`.

---

### **Resultado de la Consulta**

El resultado es una lista de cadenas formateadas que describen cuándo cada cliente se registró. Por ejemplo:

```sql
+---------------------------------------------------------+
| cust_narrative                                          |
+---------------------------------------------------------+
| MARY SMITH has been a customer since 2006-02-14         |
| PATRICIA JOHNSON has been a customer since 2006-02-14   |
| LINDA WILLIAMS has been a customer since 2006-02-14     |
| BARBARA JONES has been a customer since 2006-02-14      |
| ELIZABETH BROWN has been a customer since 2006-02-14    |
| JENNIFER DAVIS has been a customer since 2006-02-14     |
| MARIA MILLER has been a customer since 2006-02-14       |
| SUSAN WILSON has been a customer since 2006-02-14       |
| MARGARET MOORE has been a customer since 2006-02-14     |
| DOROTHY TAYLOR has been a customer since 2006-02-14     |
...
| RENE MCALISTER has been a customer since 2006-02-14     |
| EDUARDO HIATT has been a customer since 2006-02-14      |
| TERRENCE GUNDERSON has been a customer since 2006-02-14 |
| ENRIQUE FORSYTHE has been a customer since 2006-02-14   |
| FREDDIE DUGGAN has been a customer since 2006-02-14     |
| WADE DELVALLE has been a customer since 2006-02-14      |
| AUSTIN CINTRON has been a customer since 2006-02-14     |
+---------------------------------------------------------+
599 rows in set (0.00 sec)
```

---

### **Explicación del Resultado**

1. **Formato de la cadena**:
    - Cada fila del resultado es una cadena formateada que incluye:
        - El nombre completo del cliente (`first_name` + `last_name`).
        - La frase `' has been a customer since '`.
        - La fecha de creación del cliente (`create_date`), formateada solo como fecha (sin la hora).

2. **Ejemplo de una fila**:
    - `MARY SMITH has been a customer since 2006-02-14`:
        - `MARY SMITH`: Nombre completo del cliente.
        - `has been a customer since`: Texto fijo añadido.
        - `2006-02-14`: Fecha de creación del cliente.

3. **Número de filas**:
    - La consulta devuelve **599 filas**, lo que significa que hay 599 clientes en la tabla `customer`.

---

### **Resumen**

1. **`CONCAT`**:
    - Une varias cadenas en una sola.
    - En este caso, crea una narrativa descriptiva para cada cliente.

2. **`DATE`**:
    - Extrae la parte de la fecha de un campo `DATETIME` o `TIMESTAMP`.

3. **Alias (`cust_narrative`)**:
    - Proporciona un nombre descriptivo para la columna resultante.

4. **Resultado**:
    - Una lista de cadenas formateadas que describen cuándo cada cliente se registró.

---

### **Notas Importantes**

1. **Formato de fecha**:
    - La función `DATE` elimina la parte de la hora del campo `create_date`, mostrando solo la fecha en formato `YYYY-MM-DD`.

2. **Uso de `CONCAT`**:
    - `CONCAT` es muy útil para crear cadenas personalizadas a partir de varios campos o valores.

3. **Alias**:
    - Usar un alias (en este caso, `cust_narrative`) hace que el resultado sea más legible y fácil de entender.

4. **Sin cláusula `WHERE`**:
    - Como no hay una cláusula `WHERE`, la consulta devuelve resultados para **todos los clientes** en la tabla `customer`.

---

### **Ejemplo Adicional**

Si quisieras incluir la hora en la narrativa, podrías omitir la función `DATE`:

```sql
SELECT CONCAT(first_name, ' ', last_name,
       ' has been a customer since ', create_date) cust_narrative
FROM customer;
```

Esto devolvería algo como:

```sql
+---------------------------------------------------------------+
| cust_narrative                                                |
+---------------------------------------------------------------+
| MARY SMITH has been a customer since 2006-02-14 22:04:36      |
| PATRICIA JOHNSON has been a customer since 2006-02-14 22:04:36|
| LINDA WILLIAMS has been a customer since 2006-02-14 22:04:36  |
...
+---------------------------------------------------------------+
```

---


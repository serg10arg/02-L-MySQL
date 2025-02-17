
---

### **Cómo Se Manejan los Valores Nulos en Funciones de Agregación**

En SQL, los valores nulos (`NULL`) pueden afectar los resultados de las funciones de agregación. Es importante entender cómo cada función maneja los valores nulos para evitar errores o resultados inesperados.

---

### **Creación de la Tabla y Datos de Ejemplo**

Primero, se crea una tabla llamada `number_tbl` con una columna `val` de tipo `SMALLINT`:

```sql
CREATE TABLE number_tbl (val SMALLINT);
```

Luego, se insertan los valores `1`, `3` y `5` en la tabla:

```sql
INSERT INTO number_tbl VALUES (1);
INSERT INTO number_tbl VALUES (3);
INSERT INTO number_tbl VALUES (5);
```

---

### **Consulta Inicial**

Se ejecuta la siguiente consulta para calcular varias funciones de agregación:

```sql
SELECT COUNT(*) num_rows,
       COUNT(val) num_vals,
       SUM(val) total,
       MAX(val) max_val,
       AVG(val) avg_val
FROM number_tbl;
```

#### **Resultado:**
```sql
+----------+----------+-------+---------+---------+
| num_rows | num_vals | total | max_val | avg_val |
+----------+----------+-------+---------+---------+
|        3 |        3 |     9 |       5 |  3.0000 |
+----------+----------+-------+---------+---------+
```

#### **Explicación:**
- **`COUNT(*)`**: Cuenta el número total de filas en la tabla. Devuelve `3` porque hay 3 filas.
- **`COUNT(val)`**: Cuenta el número de valores no nulos en la columna `val`. Devuelve `3` porque no hay valores nulos.
- **`SUM(val)`**: Suma los valores de la columna `val`. Devuelve `9` (1 + 3 + 5).
- **`MAX(val)`**: Devuelve el valor máximo de la columna `val`. Devuelve `5`.
- **`AVG(val)`**: Calcula el promedio de los valores de la columna `val`. Devuelve `3.0000` (9 / 3).

---

### **Inserción de un Valor Nulo**

Se inserta un valor nulo en la tabla:

```sql
INSERT INTO number_tbl VALUES (NULL);
```

Ahora la tabla tiene 4 filas: `1`, `3`, `5` y `NULL`.

---

### **Consulta con Valor Nulo**

Se ejecuta la misma consulta nuevamente:

```sql
SELECT COUNT(*) num_rows,
       COUNT(val) num_vals,
       SUM(val) total,
       MAX(val) max_val,
       AVG(val) avg_val
FROM number_tbl;
```

#### **Resultado:**
```sql
+----------+----------+-------+---------+---------+
| num_rows | num_vals | total | max_val | avg_val |
+----------+----------+-------+---------+---------+
|        4 |        3 |     9 |       5 |  3.0000 |
+----------+----------+-------+---------+---------+
```

#### **Explicación:**
- **`COUNT(*)`**: Ahora devuelve `4` porque hay 4 filas en total (incluyendo la fila con `NULL`).
- **`COUNT(val)`**: Sigue devolviendo `3` porque solo cuenta los valores no nulos en la columna `val` (ignora el `NULL`).
- **`SUM(val)`**: Sigue devolviendo `9` porque ignora el valor nulo y solo suma `1 + 3 + 5`.
- **`MAX(val)`**: Sigue devolviendo `5` porque ignora el valor nulo y encuentra el máximo entre `1`, `3` y `5`.
- **`AVG(val)`**: Sigue devolviendo `3.0000` porque ignora el valor nulo y calcula el promedio de `1 + 3 + 5` dividido por `3`.

---

### **Resumen del Comportamiento de las Funciones de Agregación con Valores Nulos**

| Función      | Comportamiento con `NULL`                                                                 |
|--------------|------------------------------------------------------------------------------------------|
| **`COUNT(*)`** | Cuenta todas las filas, incluyendo aquellas con valores nulos.                           |
| **`COUNT(col)`** | Cuenta solo las filas donde `col` no es nula. Ignora los valores nulos.                  |
| **`SUM(col)`**  | Ignora los valores nulos y suma solo los valores no nulos.                               |
| **`MAX(col)`**  | Ignora los valores nulos y devuelve el máximo entre los valores no nulos.                |
| **`AVG(col)`**  | Ignora los valores nulos y calcula el promedio solo con los valores no nulos.            |

---

### **¿Por qué es importante entender esto?**

- **Evitar errores**: Si no se tiene en cuenta cómo las funciones de agregación manejan los valores nulos, los resultados pueden ser incorrectos o engañosos.
- **Análisis preciso**: Para obtener métricas precisas, es crucial saber si los valores nulos están siendo ignorados o no.
- **Diseño de consultas**: Al escribir consultas, debes decidir si los valores nulos deben ser incluidos o excluidos en los cálculos.

---

### **Ejemplo Adicional**

Si quisieras contar el número de valores nulos en la columna `val`, podrías usar:

```sql
SELECT COUNT(*) - COUNT(val) num_nulls
FROM number_tbl;
```

- **`COUNT(*)`**: Cuenta todas las filas (4).
- **`COUNT(val)`**: Cuenta solo los valores no nulos (3).
- **Resultado**: `1` (hay 1 valor nulo).

---

### **Conclusión**

- **`COUNT(*)`**: Cuenta todas las filas, incluyendo las que tienen valores nulos.
- **`COUNT(col)`**: Cuenta solo las filas donde `col` no es nula.
- **Funciones como `SUM`, `MAX`, `AVG`**: Ignoran los valores nulos y solo operan con valores no nulos.

---


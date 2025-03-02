
---

# El Operador `intersect`

La especificación ANSI SQL incluye el operador `intersect` para realizar intersecciones. Desafortunadamente, la versión 8.0 de MySQL no implementa el operador `intersect`. Si estás utilizando Oracle o SQL Server 2008, podrás usar `intersect`; sin embargo, dado que utilizo MySQL para todos los ejemplos en este libro, los conjuntos de resultados para las consultas de ejemplo en esta sección son fabricados y no pueden ejecutarse con ninguna versión hasta la versión 8.0 inclusive. También evito mostrar el prompt de MySQL (`mysql>`), ya que las declaraciones no están siendo ejecutadas por el servidor MySQL.

Si las dos consultas en una consulta compuesta devuelven conjuntos de datos no superpuestos, entonces la intersección será un conjunto vacío. Considera la siguiente consulta:

```sql
SELECT c.first_name, c.last_name
FROM customer c
WHERE c.first_name LIKE 'D%' AND c.last_name LIKE 'T%'
INTERSECT
SELECT a.first_name, a.last_name
FROM actor a
WHERE a.first_name LIKE 'D%' AND a.last_name LIKE 'T%';
Empty set (0.04 sec)
```

Aunque hay tanto actores como clientes que tienen las iniciales DT, estos conjuntos están completamente no superpuestos, por lo que la intersección de los dos conjuntos produce el conjunto vacío. Sin embargo, si cambiamos a las iniciales JD, la intersección producirá una sola fila:

```sql
SELECT c.first_name, c.last_name
FROM customer c
WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'
INTERSECT
SELECT a.first_name, a.last_name
FROM actor a
WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%';
+------------+-----------+
| first_name | last_name |
+------------+-----------+
| JENNIFER   | DAVIS     |
+------------+-----------+
1 row in set (0.00 sec)
```

La intersección de estas dos consultas produce Jennifer Davis, que es el único nombre encontrado en los conjuntos de resultados de ambas consultas.

Junto con el operador `intersect`, que elimina cualquier fila duplicada encontrada en la región superpuesta, la especificación ANSI SQL incluye un operador `intersect all`, que no elimina duplicados. El único servidor de base de datos que actualmente implementa el operador `intersect all` es el DB2 Universal Server de IBM.

---

### ¿Qué hace `INTERSECT`?

`INTERSECT` es una operación que devuelve solo los registros que son comunes entre dos consultas. En otras palabras, solo se incluirán los resultados que aparecen en **ambas** consultas. Esto es diferente de `UNION`, que combina todos los resultados de las dos consultas y elimina duplicados.

### Desglose de la consulta

### Primera consulta:

```sql
SELECT c.first_name, c.last_name
FROM customer c
WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%';
```

Esta consulta busca en la tabla `customer` y selecciona los clientes cuyo nombre (`first_name`) comienza con "J" y cuyo apellido (`last_name`) comienza con "D".

### Segunda consulta:

```sql
SELECT a.first_name, a.last_name
FROM actor a
WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%';
```

Esta consulta busca en la tabla `actor` y selecciona los actores cuyo nombre comienza con "J" y cuyo apellido comienza con "D".

### Combinación con `INTERSECT`:

`INTERSECT`

La palabra clave `INTERSECT` se utiliza para combinar los resultados de las dos consultas anteriores, pero solo devuelve los registros que son comunes entre ambas consultas. En otras palabras, solo se incluirán los nombres y apellidos que aparecen tanto en la tabla `customer` como en la tabla `actor`.

---

### Ejemplo práctico

Supongamos que tenemos los siguientes datos en nuestras tablas:

### Tabla `customer`:

| first_name | last_name |
| --- | --- |
| John | Doe |
| Jane | Davis |
| Jack | Smith |

### Tabla `actor`:

| first_name | last_name |
| --- | --- |
| John | Doe |
| Jack | Daniels |
| Julia | Davis |

### Resultados de las consultas individuales:

1. **Consulta en `customer`**:
    - Resultados:


        | first_name | last_name |
        | --- | --- |
        | John | Doe |
        | Jane | Davis |
2. **Consulta en `actor`**:
    - Resultados:


        | first_name | last_name |
        | --- | --- |
        | John | Doe |

### Resultado de la combinación con `INTERSECT`:

Solo se incluirán los registros que aparecen en ambas consultas. En este caso, el único registro común es:

| first_name | last_name |
| --- | --- |
| John | Doe |

---

### Importante sobre `INTERSECT`

1. **Compatibilidad de columnas**: Al igual que con `UNION`, las dos consultas deben tener el mismo número de columnas y tipos de datos compatibles.
2. **Duplicados**: `INTERSECT` elimina duplicados automáticamente, por lo que solo se mostrará una vez cada registro común.

### Resumen

La consulta que has proporcionado busca los nombres y apellidos que cumplen con las condiciones (`first_name LIKE 'J%'` y `last_name LIKE 'D%'`) tanto en la tabla `customer` como en la tabla `actor`. Solo se devolverán los registros que son comunes entre ambas tablas.
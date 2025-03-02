
---

# El Operador `except`

La especificación ANSI SQL incluye el operador `except` para realizar la operación de excepción. Una vez más, desafortunadamente, la versión 8.0 de MySQL no implementa el operador `except`, por lo que se aplican las mismas reglas para esta sección que para la sección anterior. Si estás utilizando Oracle Database, necesitarás utilizar el operador `minus` que no es compatible con ANSI.

El operador `except` devuelve el primer conjunto de resultados menos cualquier superposición con el segundo conjunto de resultados. Aquí tienes el ejemplo de la sección anterior, pero usando `except` en lugar de `intersect`, y con el orden de las consultas invertido:

```sql
SELECT a.first_name, a.last_name
FROM actor a
WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
EXCEPT
SELECT c.first_name, c.last_name
FROM customer c
WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%';
+------------+-----------+
| first_name | last_name |
+------------+-----------+
| JUDY       | DEAN      |
| JODIE      | DEGENERES |
| JULIANNE   | DENCH     |
+------------+-----------+
3 rows in set (0.00 sec)
```

En esta versión de la consulta, el conjunto de resultados consiste en las tres filas de la primera consulta menos Jennifer Davis, que se encuentra en los conjuntos de resultados de ambas consultas. También hay un operador `except all` especificado en la especificación ANSI SQL, pero una vez más, solo el DB2 Universal Server de IBM ha implementado el operador `except all`.

El operador `except all` es un poco complicado, así que aquí tienes un ejemplo que demuestra cómo se manejan los datos duplicados. Supongamos que tienes dos conjuntos de datos que se ven como los siguientes:

### Conjunto A

```sql
+----------+
| actor_id |
+----------+
|       10 |
|       11 |
|       12 |
|       10 |
|       10 |
+----------+
```

### Conjunto B

```sql
+----------+
| actor_id |
+----------+
|       10 |
|       10 |
+----------+
```

La operación `A except B` produce lo siguiente:

```sql
+----------+
| actor_id |
+----------+
|       11 |
|       12 |
+----------+
```

Si cambias la operación a `A except all B`, verás lo siguiente:

```sql
+----------+
| actor_id |
+----------+
|       10 |
|       11 |
|       12 |
+----------+
```

Por lo tanto, la diferencia entre las dos operaciones es que `except` elimina todas las ocurrencias de datos duplicados del conjunto A, mientras que `except all` elimina solo una ocurrencia de datos duplicados del conjunto A por cada ocurrencia en el conjunto B.

---


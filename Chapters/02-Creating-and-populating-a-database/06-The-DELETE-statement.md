### Eliminar Datos

Parece que William y Susan no se están llevando muy bien, así que uno de ellos debe irse. Dado que William estaba allí primero, Susan se irá mediante la sentencia `DELETE`:

```sql
mysql> DELETE FROM person
    -> WHERE person_id = 2;
```

**Resultado**:
```
Query OK, 1 row affected (0.01 sec)
```

### Explicación del Resultado

La clave primaria se está utilizando para aislar la fila de interés, por lo que se elimina una sola fila de la tabla. Al igual que con la sentencia `UPDATE`, se pueden eliminar más de una fila dependiendo de las condiciones en tu cláusula `WHERE`. Además, si se omite la cláusula `WHERE`, se eliminarán todas las filas en la tabla.

### Ejemplo de Eliminación de Múltiples Filas

Supongamos que queremos eliminar todas las filas donde `person_id` es menor que 10:

```sql
DELETE FROM person
WHERE person_id < 10;
```

Si omites la cláusula `WHERE`, se eliminarán todas las filas de la tabla `person`:

```sql
DELETE FROM person;
```

### Resumen

- **Uso de la Cláusula `WHERE`**: Especifica qué fila o filas se eliminarán.
- **Sin Cláusula `WHERE`**: Eliminará todas las filas en la tabla.
- **Resultado**: El servidor proporciona retroalimentación sobre cuántas filas fueron afectadas por la sentencia.

